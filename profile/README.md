# ATgora

**Community forums built on open protocol**

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://opensource.org/licenses/AGPL-3.0)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🚧 Under Active Development

ATgora is in **pre-alpha development**. We're building privacy-focused, decentralized forum software on the [AT Protocol](https://atproto.com/).

**Current Status:** Planning complete, MVP implementation starting Q1 2026

📅 **Next Milestone:** Phase 1 MVP (Q1 2026)  
🌐 **Website:** [atgora.forum](https://atgora.forum) (coming soon)  
💬 **Community:** ATgora forum (dogfooding - launching with Phase 1)

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

Traditional forums lock your identity and data into each platform. ATgora uses the AT Protocol to give you:

✅ **Portable identity** - One login across all ATgora forums  
✅ **Data ownership** - Your posts live on your PDS, not the forum's database  
✅ **Cross-forum features** - Reputation, search, aggregation across instances  
✅ **No vendor lock-in** - Migrate forums without losing users or content  
✅ **Federated moderation** - AT Protocol labelers work across all forums

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
- Markdown editor, search (full-text + semantic)
- Moderation tools, content maturity filtering
- Notifications, user profiles

**AT Protocol differentiators:**
- Portable identity (OAuth with any AT Protocol PDS)
- Cross-forum reputation
- Global aggregator (all ATgora forums in one feed)
- Social cross-posting (share topics to Bluesky/Frontpage)
- Content labeler integration

**Self-hosting & extensibility:**
- Docker Compose deployment
- Plugin system (Phase 2+)
- API-first architecture (build custom frontends)

---

## Roadmap

**Phase 1: MVP** (Q1 2026)
- Core forum features + AT Protocol integration
- Central managed hosting
- Staging environment deployment
- Global aggregator at atgora.forum

**Phase 2: Public Launch** (Q1 2026)
- Production deployment
- Self-hosting documentation
- Plugin system
- Community labeler support

**Phase 3: Monetization** (Q2 2026)
- Paid managed hosting tiers
- Admin dashboard enhancements
- Multi-tenant infrastructure

**Phase 4: Advanced Features** (Q2-Q3 2026)
- Private categories, cross-forum search
- Advanced moderation tools
- Mobile apps

**Phase 5: Migration & Growth** (Q3-Q4 2026)
- Legacy forum migration tool (Discourse, Flarum, phpBB)
- Multi-forum management
- Plugin marketplace

---

## Contributing

We're not accepting code contributions yet (pre-alpha), but you can:

- ⭐ **Star the repos** - Show support and get notified of progress
- 💬 **Join the community** - ATgora forum launching with Phase 1 (Q1 2026)
- 📖 **Read the docs** - Learn about AT Protocol and our architecture
- 🐛 **Report issues** - Found a bug in planning docs? Let us know

**Contributions will open** when we reach Phase 1 MVP (Q1 2026).

See [CONTRIBUTING.md](https://github.com/atgora-forum/.github/blob/main/CONTRIBUTING.md) for guidelines.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Runtime | Node.js 24 LTS, TypeScript |
| Backend | Fastify, Drizzle ORM, PostgreSQL 16, Valkey |
| Frontend | Next.js 16, React 19, TailwindCSS, shadcn/ui |
| Protocol | AT Protocol SDK (@atproto/api, @atproto/tap) |
| Hosting | Docker Compose, Hetzner VPS, Cloudflare CDN |
| Monitoring | Sentry, Pino, Grafana (Phase 3) |

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
- 💬 **Forum:** [atgora.forum](https://atgora.forum) (launching with Phase 1 - dogfooding our own platform)
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
