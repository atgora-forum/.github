![ATgora Banner](https://raw.githubusercontent.com/atgora-forum/.github/main/assets/banner.svg)

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://opensource.org/licenses/AGPL-3.0)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🚧 Under Active Development

ATgora is in **pre-alpha development**. We're building privacy-focused, decentralized forum software on the [AT Protocol](https://atproto.com/).

**Current Status:** Planning complete, MVP implementation starting Q1 2026

📅 **Next Milestone:** Phase 0 Workspace Setup
🌐 **Website:** [atgora.forum](https://atgora.forum) (coming soon)
💬 **Community:** ATgora forum (dogfooding - launching with MVP)

---

## What is ATgora?

ATgora is a modern forum platform that gives users **portable identity and data ownership**:

- **One account, all forums** - Use your AT Protocol identity (Bluesky, etc.) everywhere
- **Your data, your control** - Content lives on your Personal Data Server, not locked in each forum
- **Cross-forum reputation** - Your contributions follow you across every ATgora instance
- **Open source** - AGPL backend, MIT frontend. Self-host or use managed hosting.
- **Privacy-first** - EU-hosted, GDPR-compliant, no tracking, no ads

**The Discourse/Flarum alternative for the decentralized web.**

---

## Why AT Protocol?

Traditional forums lock your identity and data into each platform. The AT Protocol changes this for both the people who run forums and the people who use them.

### For Forum Admins

- ✅ **Lower costs** - EUR 19-99/month managed hosting vs Discourse $100+/month
- ✅ **No vendor lock-in** - Self-host anytime, export everything, switch hosting with zero data loss
- ✅ **Frictionless signup** - Users authenticate via Bluesky OAuth (no email/password management for you)
- ✅ **Built-in spam resistance** - Imported reputation from the AT Protocol network reduces spam from day one
- ✅ **Organic discovery** - Global aggregator surfaces your community to users across the network
- ✅ **EU sovereignty** - GDPR-native, EU-hosted infrastructure, mandatory for regulated industries
- ✅ **Federated moderation** - AT Protocol labelers provide shared moderation across all forums
- ✅ **Extensible** - Plugin system for customization without forking the codebase

### For Forum Users & Participants

- ✅ **One identity everywhere** - Your AT Protocol account (Bluesky, etc.) works across all ATgora forums
- ✅ **You own your data** - Posts live on your Personal Data Server, not locked in each forum's database
- ✅ **Portable reputation** - Contributions and trust follow you to every community you join
- ✅ **No account per forum** - One login, all communities
- ✅ **No content hostage** - Migrate between forums or leave entirely without losing your posts
- ✅ **Cross-post to Bluesky** - Share topics to your Bluesky feed with one click
- ✅ **Privacy by default** - No tracking, no ads, no profiling

---

## Repository Structure

| Repository | Description | License | Status |
|------------|-------------|---------|--------|
| **[atgora-api](https://github.com/atgora-forum/atgora-api)** | AppView backend (Fastify, PostgreSQL, AT Protocol) | AGPL-3.0 | 🚧 Pre-alpha |
| **[atgora-web](https://github.com/atgora-forum/atgora-web)** | Forum frontend (Next.js, TailwindCSS) | MIT | 🚧 Pre-alpha |
| **[atgora-lexicons](https://github.com/atgora-forum/atgora-lexicons)** | AT Protocol schemas for forum data | MIT | 🚧 Pre-alpha |
| **[atgora-deploy](https://github.com/atgora-forum/atgora-deploy)** | Docker Compose templates for self-hosting | MIT | 🚧 Pre-alpha |
| **[atgora-website](https://github.com/atgora-forum/atgora-website)** | Marketing + documentation site | MIT | 📅 Planned |

---

## Features (Planned MVP)

**Core forum functionality:**
- Topics, replies, categories, reactions
- Markdown editor, search (full-text + optional semantic)
- Moderation tools, content reporting, self-labels
- Notifications, user profiles

**AT Protocol differentiators:**
- Portable identity (OAuth with any AT Protocol PDS)
- Cross-forum reputation
- Global aggregator (all ATgora forums in one feed)
- Social cross-posting (share topics to Bluesky/Frontpage)
- Content labeler integration

**Self-hosting & extensibility:**
- Docker Compose deployment
- Plugin system (core plugins in MVP, community plugins Phase 2+)
- API-first architecture (build custom frontends)

---

## Roadmap

### MVP Development (2026)

| Phase | Focus | What Gets Built |
|-------|-------|-----------------|
| **0: Workspace Setup** | Dev infrastructure | Shared configs (TypeScript, ESLint, Vitest, commitlint), Docker dev environment, CI templates, pnpm workspace |
| **1: Risk Validation** | Prove it works | AT Protocol integration PoC -- Tap firehose, OAuth flow, PDS record writes, Drizzle+pgvector, Next.js 16+shadcn/ui |
| **2: Lexicons** | Define the data | `forum.atgora.*` lexicon schemas, generated TypeScript types, Zod validation, npm package (`@atgora-forum/lexicons`) |
| **3: API** | Build the backend | Fastify AppView with 13 milestones: firehose indexing, OAuth auth, topics, replies, categories, reactions, moderation, search, notifications, profiles, cross-posting, admin, setup wizard |
| **4: Frontend** | Build the UI | Next.js frontend with 13 milestones: design system, auth flow, homepage, topic pages, editor, reactions, profiles, search, notifications, admin panel, SEO, accessibility audit (WCAG 2.2 AA) |
| **5: Deploy** | Ship it | Production Docker Compose, Caddy reverse proxy + auto-SSL, global aggregator config, installation docs, backup/restore scripts |

A professional security audit is scheduled after Phase 3 (API complete), with remediation time before frontend polish.

### Post-MVP

| Phase | Focus | What Gets Built |
|-------|-------|-----------------|
| **Self-Hosting + AI** | Empower self-hosters | Self-hosting documentation, AI moderation, summarization, translation, labeler integration, plugin sandbox |
| **Monetization + SaaS** | Sustainable business | Managed hosting tiers (Stripe billing), multi-tenant infrastructure, managed AI tier, PWA (push notifications, offline) |
| **Advanced Features** | Depth | Private categories, DMs, cross-community search, solved markers, plugin marketplace, cross-instance plugin discovery |
| **Migration & Growth** | Reach | Legacy forum migration tool (Discourse, Flarum, phpBB), multi-community management dashboard, federation tools |
| **Try-Then-Own** | Growth loop | Sandbox experience, free tier (50 active users), upgrade flow, conversion tracking |

---

## Contributing

We're not accepting code contributions yet (pre-alpha), but you can:

- ⭐ **Star the repos** - Show support and get notified of progress
- 💬 **Join the community** - ATgora forum launching with MVP
- 📖 **Read the docs** - Learn about AT Protocol and our architecture
- 🐛 **Report issues** - Found a bug in planning docs? Let us know

**Contributions will open** when we reach MVP.

See [CONTRIBUTING.md](https://github.com/atgora-forum/.github/blob/main/CONTRIBUTING.md) for guidelines.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Runtime | Node.js 24 LTS, TypeScript |
| Backend | Fastify, Drizzle ORM, PostgreSQL 16, Valkey |
| Frontend | Next.js 16, React 19, TailwindCSS, shadcn/ui |
| Protocol | AT Protocol SDK (@atproto/api, @atproto/tap) |
| Search | Full-text (PostgreSQL) + optional semantic (pgvector) |
| Hosting | Docker Compose, Hetzner VPS, Cloudflare CDN |
| Monitoring | Sentry (EU), Pino, Grafana (post-MVP) |

---

## License

**Dual-licensed for openness and sustainability:**

- **Backend (atgora-api):** AGPL-3.0 - Protects the core, competitors must share changes
- **Frontend (atgora-web):** MIT - Encourages customization and theming
- **Lexicons (atgora-lexicons):** MIT - Open standard, maximum adoption
- **Deploy (atgora-deploy):** MIT - Self-hosting should be freely usable

**Contributors sign a CLA** to allow future commercial licensing flexibility.

---

## Community

- 🌐 **Website:** [atgora.forum](https://atgora.forum) (coming soon)
- 💬 **Forum:** [atgora.forum](https://atgora.forum) (launching with MVP - dogfooding our own platform)
- 🐦 **Bluesky:** [@atgora.forum](https://bsky.app/profile/atgora.forum) (TBD)
- 📧 **Contact:** [@gxjansen](https://github.com/gxjansen)

---

## Acknowledgments

Built with:
- [AT Protocol](https://atproto.com/) - The foundation for everything
- [Bluesky](https://bsky.social/) - Inspiration and reference implementation
- Open source community - Standing on the shoulders of giants

---

**ATgora** - Forums for the open web

© 2026 ATgora. AGPL-3.0 (backend) + MIT (frontend/lexicons/deploy).
