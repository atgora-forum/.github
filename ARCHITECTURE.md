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
        Caddy["Caddy (SSL, routing)"]:::proxy
    end

    Browser -->|"HTTPS"| Caddy

    %% ── Application Layer ──
    subgraph app ["Application Layer"]
        direction LR
        Web["barazo-web<br/>Next.js / React<br/>SSR, SEO, a11y"]:::frontend
        API["barazo-api<br/>Fastify REST API<br/>Auth, Plugin System"]:::backend
    end

    Caddy -->|"/*"| Web
    Caddy -->|"/api/*, /docs"| API
    Web -->|"REST API calls"| API

    %% ── Data Layer ──
    subgraph data ["Data Layer"]
        direction LR
        PG["PostgreSQL 16 + pgvector<br/>Forum Index, Full-text &<br/>Semantic Search"]:::datastore
        VK["Valkey<br/>Session Cache, Query Cache,<br/>Rate Limiting"]:::datastore
    end

    API --> PG
    API --> VK

    %% ── AT Protocol Layer ──
    subgraph atp ["AT Protocol Network"]
        direction LR
        Relay["Bluesky Relay<br/>bsky.network"]:::atproto
        PDS["User PDS<br/>Source of Truth"]:::atproto
    end

    subgraph firehose ["Firehose Consumer"]
        Tap["Tap (@atproto/tap)<br/>Filters for forum.barazo.*"]:::backend
    end

    Relay -->|"WebSocket"| Tap
    Tap -->|"Validated records"| API
    API -->|"OAuth (DPoP)"| PDS

    %% ── External Integrations ──
    subgraph ext ["External Services"]
        direction LR
        GT["GlitchTip (Error Tracking)"]:::external
        UM["Umami (Analytics)"]:::external
        AI["AI Provider (Optional)<br/>Ollama / OpenRouter / OpenAI"]:::external
    end

    API -.->|"Sentry SDK"| GT
    Web -.->|"Script tag"| UM
    API -.->|"Embeddings, Moderation"| AI
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
            C["Caddy :80 :443"]:::container
            W["barazo-web :3000"]:::container
            A1["barazo-api :3001"]:::container
        end

        subgraph bn ["Backend Network"]
            A2["barazo-api (bridged)"]:::container
            P["PostgreSQL :5432"]:::container
            V["Valkey :6379"]:::container
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

    Internet(("Internet (port 80/443 only)")) --> C

    note1["Only Caddy is externally exposed"]
```

## Operating Modes

```mermaid
graph LR
    classDef single fill:#264653,stroke:#1d3557,color:#fff
    classDef global fill:#e76f51,stroke:#c1440e,color:#fff
    classDef relay fill:#9b5de5,stroke:#7b2cbf,color:#fff

    subgraph single_mode ["Single-Community Mode"]
        S_API["barazo-api<br/>filters by community DID"]:::single
        S_DB["PostgreSQL (one community)"]:::single
        S_API --> S_DB
    end

    subgraph global_mode ["Global Aggregator Mode"]
        G_API["barazo-api<br/>indexes ALL forum.barazo.*"]:::global
        G_DB["PostgreSQL (all communities)"]:::global
        G_API --> G_DB
    end

    Relay["AT Protocol Relay"]:::relay
    Relay --> S_API
    Relay --> G_API

    note2["Same codebase, different config:<br/>COMMUNITY_MODE=single vs global"]
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
