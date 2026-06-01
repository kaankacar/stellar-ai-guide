# Stellar AI Guide Brazil — Developer Guide

Brazil has one of the most active instant-payment systems in the world, with PIX moving money 24/7 across consumers, businesses, and financial apps. Stellar is built for exactly this kind of real-world money movement: fast settlement, low fees, local assets, and programmable rails that can connect builders to global liquidity.

This repo is a collection of guides put together by the SDF DevRel team to help you move fast. The goal is simple: every developer at this event should have the same shot at building something real, regardless of whether they have a paid AI subscription, years of Stellar experience, or a high-end laptop.

## Start here

**About to open Claude Code for the first time?** Start with `../../Starter_Prompts.md`. It has a ready-to-paste protocol context block, the correct way to describe what you're building so Claude doesn't default to the wrong architecture, and a CLAUDE.md template for Stellar projects.

**Building with Brazilian real rails?** The regional starter pack (`Hackathon_Resources.md`) is the fastest path. For Brazil, use Etherfuse as the primary self-service path: BRL to TESOURO via PIX, with the same portable TypeScript anchor library you can drop into any Node project. Alfred Pay, Abroad Finance, and Transfero are relevant Brazil ecosystem references too, but the starter pack treats them as secondary/honorable-mention providers because they do not expose the same self-service developer flow as Etherfuse.

**Need a PIX-specific walkthrough?** Read `PIX_Guide.md`. It explains the practical BRL via PIX <-> TESOURO flow, including Etherfuse sandbox setup, hosted onboarding, TESOURO asset lookup, on-ramp/off-ramp order flow, Stellar claim transactions, sandbox simulation endpoints, and the gotchas that usually block builders.

**Don't have a paid AI subscription?** Start with `Free_AI_Setup.md`. It now prioritizes free cloud options first: OpenRouter, Groq, Cerebras, Google AI Studio, NVIDIA NIM, xAI Grok credits, Hugging Face Spaces, free GPU notebooks, and startup credits. Local Ollama setup is still included, but it is intentionally at the end because most hackers should not spend the event fighting laptop inference.

**About to write code?** Read `Dev_Setup_Guide.md` first. Five minutes here saves hours later.

## Suggested reading order

1. `../../Starter_Prompts.md` before your first Claude Code session
2. `Free_AI_Setup.md` if you need a free AI setup
3. `Dev_Setup_Guide.md` before writing any code
4. `PIX_Guide.md` if your app touches BRL, PIX, TESOURO, or a Brazil on/off-ramp
5. `Hackathon_Resources.md` to orient yourself in the Stellar ecosystem
6. `../../Claude_Code_Guide.md` for commands, parallel agents, and browser automation
7. `../../Recommended_AI_Tools.md` to explore what else is available

## Starter_Prompts.md

The fastest way to avoid the most common Claude mistake at a hackathon: building a DeFi dApp with Freighter integration when you wanted a self-custodial wallet.

**Wallet vs. dApp:** A concrete before/after prompt showing how to describe your architecture so Claude builds the right thing on the first attempt. Not a DeFi interface. A self-custodial wallet that generates its own BIP-39 mnemonic, encrypts it with your password, and stores it in localStorage.

**Protocol context block:** A ready-to-paste block covering SDK version (v14, `rpc` namespace), Stellar Wallets Kit version, correct USDC issuer for DeFindex compatibility, and network configuration. Paste it at the top of your first message.

**Parallel agent prompt template:** The exact prompt to spawn three parallel agents (core logic + tests / state + routing / UI components) after your scaffold is done. From the DevRel experiment: same work, 40% less wall-clock time.

**CLAUDE.md template:** What to put in your project's CLAUDE.md for a Stellar wallet build, including Etherfuse and DeFindex specific notes.

**Quick corrective prompts:** One-liners for the most common mid-session corrections (wrong wallet type, wrong SDK version, wrong USDC issuer, stuck on DeFindex key, stuck polling sendTransaction).

## Dev_Setup_Guide.md

Everything you need before writing code, in one place.

**API keys:** Etherfuse is the important one to get early for Brazil because it is the primary self-service PIX/TESOURO path: https://devnet.etherfuse.com/ramp. DeFindex and Trustless Work are also self-service. Soroswap, Phoenix, Aquarius, and Blend require no key.

**Testnet contract addresses:** Verified DeFindex, Soroswap, Blend and Trustless Work addresses are included. Aquarius and Reflector Oracle link to their live registries. Phoenix is the only protocol still TBD.

**Auth patterns:** This is where most developers lose time. Etherfuse uses `Authorization: your-api-key` with no Bearer prefix. DeFindex uses `Authorization: Bearer your-api-key`. Trustless Work uses `Authorization: x-api-key: your-api-key`. They're opposites and none are documented clearly. The guide has correct code snippets for all three.

**Testnet asset registry:** Testnet USDC has multiple issuers that don't share liquidity; pick the wrong one and swaps silently fail. The guide keeps the existing USDC issuer unchanged and adds Brazil-specific references for Etherfuse TESOURO and Transfero BRZ.

**Critical gotchas** (pulled from 60 build runs):
- Etherfuse `customerId` and `bankAccountId` are per end-user: generate once per user, store, reuse forever — never per session
- Etherfuse: a G... address can only be registered to one customer; re-registration fails even in sandbox
- Etherfuse sandbox orders don't progress automatically: you have to POST to `/ramp/order/fiat_received` to simulate fiat arriving
- Etherfuse has a 3-10 second indexing delay after order creation: immediate status queries return 404
- Etherfuse hosted onboarding must be completed before order creation works reliably; otherwise `POST /ramp/order` can return `Proxy account not found`
- Etherfuse response envelopes are inconsistent across endpoints: the guide includes a normalizer
- Soroban: always simulate before sending (`simulateTransaction` + `assembleTransaction` before `sendTransaction`)
- `sendTransaction` returns PENDING, not success: you need to poll (or use `rpc.Server.pollTransaction`)
- DeFindex: classic Stellar assets must be SAC-deployed before depositing into vaults (all common ones already are)
- DeFindex: the endpoint is `/vault/` not `/vaults/`, amounts are always arrays, success is HTTP 201
- Trustless Work: Each role involved must establish a trustline for the asset being used to create the escrow before the escrow is initialized.
- Trustless Work: The platform role is the only role authorized to modify the escrow at any time. However, once the escrow holds a balance, modifications are restricted exclusively to adding milestones, and no other properties can be updated.
- Trustless Work: The trustline address must be the issuer address (starting with 'G'), not the contract ID.
- Trustless Work: The dispute resolver address must be unique.

**Section 6** covers known limitations and what to ask your DevRel mentor about, including undocumented API behaviors and known quirks.

## Hackathon_Resources.md

**Regional starter pack** (Brazil + Latin America): https://github.com/ElliotFriend/regional-starter-pack
A SvelteKit app with a portable TypeScript anchor library. The `/src/lib/anchors/` folder is framework-agnostic; copy it into any TypeScript or Node project and it works without SvelteKit. Covers SEP-1, SEP-6, SEP-10, SEP-12, SEP-24, SEP-31, and SEP-38. Also ships with pre-configured MCP servers for Claude Code (Svelte docs + Etherfuse docs) so Claude understands the stack out of the box.

**DeFi reference implementation**: https://github.com/kaankacar/stellar-defi-app
A full mainnet DeFi dashboard integrating Blend (lending/borrowing with health factor monitoring), Soroswap Aggregator (DEX routing), Phoenix DEX, Aquarius AMM, SDEX, and Reflector Oracle (on-chain USD price feeds). Most useful for seeing how the protocols compose: actual API call shapes, response formats, and how to wire everything together.

**AI Integration Series (carstenjacobsen):** Four focused Next.js + TypeScript apps built with Claude Code, each paired with a BUILD_REPORT.md:
- [ai-freighter-integration](https://github.com/carstenjacobsen/ai-freighter-integration): Freighter wallet connection, balance display, send payments, transaction history
- [ai-soroswap-integration](https://github.com/carstenjacobsen/ai-soroswap-integration): Multi-DEX swap aggregator across Soroswap, Phoenix, Aqua, and SDEX
- [ai-defindex-integration](https://github.com/carstenjacobsen/ai-defindex-integration): DeFindex yield vault deposits/withdrawals and dfToken balance tracking
- [ai-passkeys-integration](https://github.com/carstenjacobsen/ai-passkeys-integration): WebAuthn passkey smart wallet with Etherfuse ramp patterns that can be adapted to BRL/TESOURO
- [ai-etherfuse-integration](https://github.com/carstenjacobsen/ai-etherfuse-integration): Full Etherfuse ramp pattern plus DeFindex yield and Freighter wallet in one app

**Community resources:**
- Stellar Hackathon FAQ (briwylde08): https://github.com/briwylde08/stellar-hackathon-faq
- Stellar DeFi Gotchas (kaankacar): https://github.com/kaankacar/stellar-defi-gotchas (400+ findings from 60 vibe-coding runs, organized by protocol)
- Arroz Wallet build log (rice2000): full Python/Flask wallet with Etherfuse on/off-ramp across 14 hours of real development. Great for understanding what end-to-end actually looks like.
- Stellar Ecosystem DB (lumenloop): https://github.com/lumenloop/stellar-ecosystem-db (structured database of 646 Stellar projects with categories, contracts, and GitHub links). Useful for finding existing work in your space before building from scratch.

**Videos worth watching:**
- Scoping and Evaluating Your Project (a must-watch before writing any code)
- Vibe Coding 5 ZK Games in 90 Minutes (live demo of AI-assisted building)
- The Builder's Guide to AI Prompt Engineering
- A First Look: Nethermind's SPP (Stellar Private Payments, ZK-based)

All links are in the file.

## PIX_Guide.md

The practical Brazil payment guide for this repo. It focuses on the self-service Etherfuse path: BRL via PIX <-> TESOURO on Stellar.

**What it covers:** sandbox env vars, Etherfuse auth format, stable `customerId` and `bankAccountId` handling, hosted onboarding, TESOURO asset lookup, on-ramp/off-ramp quotes and orders, PIX payment instructions, Stellar claim transactions, and manual sandbox state transitions.

**Provider scope:** Etherfuse is the recommended implementation path. Transfero, Abroad Finance, and Alfred Pay are included as ecosystem notes, not as assumed drop-in APIs.

## Claude_Code_Guide.md

**Plan mode** (`Shift+Tab` or `/plan`): Claude reasons through the problem before touching any code. Use it for anything larger than a one-liner. It produces a numbered plan you can edit before it executes.

**Parallel agents:** The pattern that works: plan mode (20 min) → scaffold agent → 3 parallel agents (core logic + tests / state + routing / UI components) → integration agent → browser smoke test. In the DevRel experiment, this cut wall-clock time by roughly 40% on a 145-minute build. `../../Starter_Prompts.md` has the exact prompt to trigger this pattern.

**CLAUDE.md:** A project-level file that every Claude Code session reads automatically on startup. Put your tech stack, USDC issuer, testnet addresses, and any project-specific gotchas here so you never have to re-explain them.

**Browser integration testing:** Claude in Chrome can open your running app, click through every user flow, and verify on-chain results without you touching the keyboard. The guide has the full smoke test prompt (create wallet → Friendbot → trustline → lock/unlock → verify balance) and the fix for the `oninput` event issue in Svelte.

**Stellar-specific plugins:**
- `stellar-dev:stellar-dev`: 8-module playbook covering Soroban contract development (Rust), RPC vs Horizon API reference, frontend + wallet integration (Freighter, Wallets Kit v2, passkeys), classic assets + SAC bridge, 15 documented pitfalls with fixes, security checklist, testing strategy (unit + local Quickstart + testnet), and an ecosystem catalog of DeFi protocols, oracles, and tools. Pre-installed in Claude Code.
- `openzeppelin-skills`: three skills for secure Stellar contract development, plus the OZ MCP server for AI-assisted contract generation. Install with `/plugin marketplace add OpenZeppelin/openzeppelin-skills`.

Full slash command reference, keyboard shortcuts, and CLI flags are in the file.

## Recommended_AI_Tools.md

**Stellar-native tools:**
- **Stella**: SDF's official AI assistant for Stellar dev questions. Yellow chat icon at https://developers.stellar.org, also `#stella-help` on Discord.
- **llms.txt**: machine-readable Stellar docs digest for feeding into any LLM. https://developers.stellar.org/llms.txt
- **stellar-dev skill**: Claude Code playbook for Soroban, SDKs, RPC, wallet integration, passkeys, and security patterns.
- **OpenZeppelin on Stellar**: audited contract library, Contracts Wizard (visual configurator that generates Soroban code), Contracts MCP server, Relayer, Monitor, and Soroban Security Detectors SDK. https://www.openzeppelin.com/networks/stellar
- **x402**: per-request HTTP payment protocol for AI agents. Client hits a paywalled endpoint, receives a 402, signs a Soroban auth entry, and retries with payment. Lets AI agents pay for API calls autonomously.
- **Stellar MCP Server** (kalepail): exposes Stellar wallet, token, and contract tools to Claude via Cloudflare Workers.
- **XDR MCP** (leighmcculloch): decodes and encodes Stellar XDR to/from JSON for AI agents.

**Coding assistants with free tiers:**

| Tool | Free tier | Best for |
|---|---|---|
| Claude Code | Free via OpenRouter/Groq/Gemini or local Ollama (see `Free_AI_Setup.md`) | Agentic terminal coding, full repo context |
| Continue | Fully free | VS Code/JetBrains, any local or cloud model |
| Aider | Fully free | Terminal + Git, model-agnostic |
| Cursor | 2,000 completions/mo | VS Code-like IDE with AI built in |
| GitHub Copilot | 2,000 completions + 50 chats/mo | In-editor suggestions |
| Jules (Google) | 15 tasks/day | Autonomous GitHub-integrated agent |
| Windsurf | 25 prompt credits/mo | Agentic IDE for large codebases |

**Multi-agent frameworks** (for teams building AI-native products): LangGraph, CrewAI, AutoGen (AG2), OpenAI Agents SDK.

**Rapid prototyping:** Bolt.new (1M tokens/mo, full-stack from a single prompt), v0 by Vercel (React/Next.js UI from text), Google AI Studio (fully free, 1M context window).

## Free_AI_Setup.md

**Best first move:** use the free cloud stack before trying local inference. OpenRouter, Groq, Cerebras, Google AI Studio, NVIDIA NIM, xAI Grok, Hugging Face Spaces, Kaggle, Colab, and Lightning AI give most teams enough AI access to build without a paid subscription or a high-end laptop.

**Claude Code without paying for Claude API:** The guide includes OpenAI-compatible Claude Code configs for OpenRouter, Groq, and Google AI Studio. For local use, it keeps Ollama instructions at the end with Devstral and `gpt-oss:20b` as the practical starting points.

**Free trial credits and startup programs:** SambaNova, Scaleway, Nebius, Hyperbolic, Fireworks, AWS Activate, Google for Startups, Microsoft Founders Hub, and similar programs can cover a full hackathon weekend or an early demo if your project needs more hosted compute.

**Current practical open-source / open-weight models to know:**

| Model family | Best for | Practical access |
|---|---|---|
| Qwen3.5 / Qwen3-Coder-Next | Agentic coding, codebase edits, tool use | Hosted or smaller/GGUF local builds |
| DeepSeek V4 | Hard reasoning, coding, long-context debugging | Hosted/VPS |
| Kimi K2.5 | Agentic coding, multimodal/front-end work, long context | Hosted/VPS |
| GLM-4.6 | Coding agents, polished frontend generation, long context | Hosted/VPS |
| MiniMax M2.5 | SWE-bench-style coding, agentic tool use, full-stack tasks | Hosted/VPS |
| Devstral | Local/near-local coding agent workflows | High-end laptop/GPU or VPS |
| gpt-oss | Open-weight reasoning and agentic tasks | 20B local-ish; 120B VPS/cloud |

**No GPU?** Rent a GPU server on RunPod (~$0.20/hr for an RTX 4090) or Vast.ai (~$0.15/hr). SSH-tunnel the port back and Claude Code sees it as if it were local. A full hackathon weekend costs $10-20.

*SDF DevRel, March 2026*
