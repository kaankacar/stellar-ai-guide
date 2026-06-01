# Stellar AI Guide

A country-agnostic monorepo of developer guides for building on Stellar with AI tooling. Country-specific content lives in `countries/<code>/`; everything that applies across regions lives in `shared/`.

The structure follows the same pattern as [ElliotFriend/regional-starter-pack](https://github.com/ElliotFriend/regional-starter-pack): a single repo with regional folders, so a builder in any supported country can clone once and find the right guide for their rails.


## Layout

```
.
├── README.md                       # You are here
├── CONTRIBUTING.md                 # How to add a new country
├── shared/                         # Country-agnostic guides
│   ├── Claude_Code_Guide.md        # Claude Code commands, parallel agents, browser automation
│   ├── Recommended_AI_Tools.md     # Curated map of the Stellar / AI tool ecosystem
│   └── Starter_Prompts.md          # Protocol context block, CLAUDE.md template, corrective prompts
└── countries/
    ├── _template/                  # Skeleton for adding a new country
    ├── mx/                         # Mexico: SPEI, CETES, AlfredPay, BlindPay, Etherfuse
    │   ├── README.md
    │   ├── Dev_Setup_Guide.md
    │   ├── Free_AI_Setup.md
    │   └── Hackathon_Resources.md
    └── br/                         # Brazil: PIX, TESOURO, BRZ, Etherfuse, Transfero
        ├── README.md
        ├── Dev_Setup_Guide.md
        ├── Free_AI_Setup.md
        ├── Hackathon_Resources.md
        └── PIX_Guide.md
```


## Supported countries

| Code | Country | Rails covered | Primary anchor path |
|---|---|---|---|
| [`mx`](./countries/mx) | Mexico | SPEI | Etherfuse (MXN ↔ CETES), AlfredPay (MXN ↔ USDC), BlindPay (MXN ↔ USDB) |
| [`br`](./countries/br) | Brazil | PIX | Etherfuse (BRL ↔ TESOURO); Transfero (BRZ), Abroad Finance, Alfred Pay as ecosystem refs |


## How to use this repo

1. Read the shared docs first if you've never used Claude Code on a Stellar project:
   - [`shared/Starter_Prompts.md`](./shared/Starter_Prompts.md) — paste-ready protocol context, the wallet-vs-dApp distinction, CLAUDE.md template
   - [`shared/Claude_Code_Guide.md`](./shared/Claude_Code_Guide.md) — commands, parallel agents, browser automation
   - [`shared/Recommended_AI_Tools.md`](./shared/Recommended_AI_Tools.md) — what AI tools exist in and around the Stellar ecosystem
2. Open the country folder that matches your rails. Each country folder has its own `README.md` that walks through the local setup, the anchors with self-service developer flows, and the gotchas that have cost builders hours in past hackathons.
3. If your project touches more than one country, read both folders. The shared docs apply unchanged.


## Adding a new country

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) and copy [`countries/_template/`](./countries/_template) as a starting point.


## Lineage

This repo merges and supersedes two earlier single-country repos:

- [`kaankacar/stellar-ai-guide-mx`](https://github.com/kaankacar/stellar-ai-guide-mx) — Mexico (Hack+ Alebrije | CDMX 2026)
- [`kaankacar/stellar-ai-guide-br`](https://github.com/kaankacar/stellar-ai-guide-br) — Brazil

Both originals will keep working, but new edits land here.
