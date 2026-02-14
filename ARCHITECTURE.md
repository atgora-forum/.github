# Barazo Architecture Overview

## System Architecture

```mermaid
graph TB
    %% ── Styling ──
    classDef user fill:#f5f5f5,stroke:#333,color:#333
    classDef proxy fill:#2d6a4f,stroke:#1b4332,color:#fff
    classDef frontend fill:#264653,stroke:#1d3557,color:#fff
    classDef backend fill:#e76f51,stroke:#c1440e,color:#fff
    classDef datastore fill:#457b9d,stroke:#1d3557,color:#fff
    classDef atproto fill:#9b5de5,stroke:#7b2cbf,color:#fff
    classDef external fill:#6c757d,stroke:#495057,color:#fff

    %% ── Users ──
    Browser["Browser / API Client"]:::user

    %% ── Edge Layer ──
    subgraph edge ["Reverse Proxy"]
        Caddy["Caddy\n(SSL, routing)"]:::proxy
    end

    Browser -->|"HTTPS"| Caddy

    %% ── Application Layer ──
    subgraph app ["Application Layer"]
        direction LR
        Web["barazo-web\n(Next.js / React)\nSSR, SEO, a11y"]:::frontend
        API["barazo-api\n(Fastify)\nREST API, Auth,\nPlugin System"]:::backend
    end

    Caddy -->|"/*"| Web
    Caddy -->|"/api/*\n/docs"| API
    Web -->|"REST API calls\n(server & client)"| API

    %% ── Data Layer ──
    subgraph data ["Data Layer"]
        direction LR
        PG["PostgreSQL 16\n+ pgvector\n(Forum Index,\nFull-text & Semantic Search)"]:::datastore
        VK["Valkey\n(Session Cache,\nQuery Cache,\nRate Limiting)"]:::datastore
    end

    API --> PG
    API --> VK

    %% ── AT Protocol Layer ──
    subgraph atp ["AT Protocol Network"]
        direction LR
        Relay["Bluesky Relay\n(bsky.network)\nFull Firehose"]:::atproto
        PDS["User PDS\n(Bluesky, self-hosted,\nor other providers)\nSource of Truth"]:::atproto
    end

    subgraph firehose ["Firehose Consumer"]
        Tap["Tap\n(@atproto/tap)\nFilters for\nforum.barazo.*"]:::backend
    end

    Relay -->|"WebSocket\n(filtered subscription)"| Tap
    Tap -->|"Validated records\n(create/update/delete)"| API
    API -->|"OAuth (DPoP)\nRead/Write records"| PDS

    %% ── External Integrations ──
    subgraph ext ["External Services"]
        direction LR
        GT["GlitchTip\n(Error Tracking)"]:::external
        UM["Umami\n(Analytics)"]:::external
        AI["AI Provider\n(Ollama / OpenRouter /\nOpenAI / Anthropic)\nOptional"]:::external
    end

    API -.->|"Sentry SDK"| GT
    Web -.->|"Script tag"| UM
    API -.->|"Embeddings,\nModeration"| AI
```

## Data Flow

```mermaid
sequenceDiagram
    participant U as User (Browser)
    participant W as barazo-web
    participant A as barazo-api
    participant PG as PostgreSQL
    participant PDS as User's PDS
    participant R as Relay

    Note over U,R: Reading (90-95% of traffic)
    U->>W: GET /t/topic-slug/123
    W->>A: GET /api/topics/123
    A->>PG: Query indexed data
    PG-->>A: Topic + replies
    A-->>W: JSON response
    W-->>U: Server-rendered HTML

    Note over U,R: Writing (5-10% of traffic)
    U->>W: Submit new reply
    W->>A: POST /api/topics/123/replies
    A->>PDS: Create record on user's PDS
    PDS-->>A: Record created (AT URI + CID)
    A-->>W: 201 Created
    PDS->>R: Record propagated via firehose
    R->>A: Tap receives forum.barazo.* record
    A->>PG: Index the new record
```

## Deployment Architecture

```mermaid
graph TB
    classDef container fill:#264653,stroke:#1d3557,color:#fff
    classDef network fill:#e9ecef,stroke:#adb5bd,color:#333
    classDef volume fill:#457b9d,stroke:#1d3557,color:#fff

    subgraph docker ["Docker Compose"]

        subgraph fn ["Frontend Network"]
            C["Caddy\n:80 :443"]:::container
            W["barazo-web\n:3000"]:::container
            A1["barazo-api\n:3001"]:::container
        end

        subgraph bn ["Backend Network"]
            A2["barazo-api\n(bridged)"]:::container
            P["PostgreSQL\n:5432"]:::container
            V["Valkey\n:6379"]:::container
        end

        A1 ~~~ A2
    end

    subgraph volumes ["Persistent Volumes"]
        PGD["pgdata"]:::volume
        VKD["valkeydata"]:::volume
        CD["caddydata"]:::volume
        PL["plugins"]:::volume
    end

    P --- PGD
    V --- VKD
    C --- CD
    A1 --- PL

    Internet(("Internet\n(port 80/443 only)")) --> C

    note1["Only Caddy is\nexternally exposed"]
```

## Operating Modes

```mermaid
graph LR
    classDef single fill:#264653,stroke:#1d3557,color:#fff
    classDef global fill:#e76f51,stroke:#c1440e,color:#fff
    classDef relay fill:#9b5de5,stroke:#7b2cbf,color:#fff

    subgraph single_mode ["Single-Community Mode"]
        S_API["barazo-api\n(filters by\ncommunity DID)"]:::single
        S_DB["PostgreSQL\n(one community)"]:::single
        S_API --> S_DB
    end

    subgraph global_mode ["Global Aggregator Mode"]
        G_API["barazo-api\n(indexes ALL\nforum.barazo.*)"]:::global
        G_DB["PostgreSQL\n(all communities)"]:::global
        G_API --> G_DB
    end

    Relay["AT Protocol\nRelay"]:::relay
    Relay --> S_API
    Relay --> G_API

    note2["Same codebase,\ndifferent config:\nCOMMUNITY_MODE=single\nvs\nCOMMUNITY_MODE=global"]
```

## Hosting Tiers

| Tier | Frontend | Backend (AppView) | Cross-Community | Cost |
|------|----------|-------------------|-----------------|------|
| **Fully Managed** | Barazo hosts, your custom domain | Barazo central AppView | Automatic | Paid (EUR 19-99/mo) |
| **Custom Frontend** | You host barazo-web | Barazo central AppView API | Automatic | Lower paid |
| **Fully Self-Hosted** | You host | You host | Optional (via global API) | Free (AGPL) |

Migration between tiers is lossless -- user content lives on their PDS, not on servers.

## Key Differentiators vs. Traditional Forums

| Feature | Traditional (Discourse/Flarum) | Barazo |
|---------|-------------------------------|--------|
| Data ownership | Server stores all data | Users own data on their PDS |
| Identity | Per-forum accounts | Portable AT Protocol identity |
| Reputation | Per-forum only | Cross-community, portable |
| Migration | Export/import, lose identity | Change server, keep everything |
| Cross-instance | Not possible | Global aggregator built-in |
| Plugins | Server-side only | Can define portable Lexicon schemas |

## Tech Stack Summary

| Layer | Technology |
|-------|-----------|
| **Frontend** | Next.js 16, React 19, TailwindCSS, Radix Colors, Phosphor Icons |
| **Backend** | Fastify, TypeScript, Drizzle ORM, Zod validation |
| **Protocol** | @atproto/api, @atproto/oauth-client-node, @atproto/tap |
| **Database** | PostgreSQL 16 + pgvector, Valkey |
| **Infrastructure** | Docker Compose, Caddy, Hetzner VPS, Bunny.net CDN |
| **Monitoring** | GlitchTip (errors), Pino (logs), Umami (analytics) |
| **CI/CD** | GitHub Actions, GHCR (container registry) |
