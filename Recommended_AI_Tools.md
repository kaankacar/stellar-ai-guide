# Recommended AI Tools for Stellar Builders

A curated map of the AI tool ecosystem you can pull from when building on Stellar. Covers Stellar-native integrations, coding assistants, and multi-agent frameworks. For local/open-source model setup, see your country's `Free_AI_Setup.md`.


## 1. Stellar-Native AI Tools

Start here. These are unique to the Stellar ecosystem.

| Resource | What it is | Where to find it |
|---|---|---|
| **Stella** | Official AI bot for Stellar dev questions (beta) | https://developers.stellar.org/docs/tools/developer-tools/ai-bot (yellow chat icon on docs site); also `#stella-help` on Discord |
| **llms.txt** | Machine-readable Stellar docs digest designed for feeding into LLMs | https://developers.stellar.org/llms.txt (covers Build, Learn, Tokens, Data, Tools, Networks, Validators) |
| **stellar-dev skill** | Claude Code skill (Jan 2026 playbook) covering Soroban, SDKs, RPC, wallet integration, passkeys, and security patterns | Invoke with `stellar-dev:stellar-dev` in Claude Code; repo: https://github.com/stellar/stellar-dev-skill |
| **stellar-build** (kaankacar) | One-command install that drops 42 skills + 6 SDF-DevRel-named AI personas into Claude Code and Codex CLI, covering the full Stellar journey: idea discovery, PRD/UX design, architecture, story-driven dev, mainnet deploy, and SCF grant submission. Bundles `stellar-dev-skill`, the LumenLoop ecosystem catalog (728 projects), and Electric Capital's Stellar taxonomy (9,027 repos). | `curl -fsSL https://raw.githubusercontent.com/kaankacar/stellar-build/main/install.sh \| bash`; repo: https://github.com/kaankacar/stellar-build |
| **OpenZeppelin skills** | Claude Code skills for secure Stellar contract development: `setup-stellar-contracts`, `upgrade-stellar-contracts`, and `develop-secure-contracts`. Installs optional MCP servers for AI-assisted contract generation. | Install: `/plugin marketplace add OpenZeppelin/openzeppelin-skills` in Claude Code; repo: https://github.com/OpenZeppelin/openzeppelin-skills |
| **Smart Account Kit** (kalepail) | TypeScript SDK for building passkey smart wallets on Soroban: `createWallet`, `connectWallet` (silent session restore), `signAndSubmit`, gasless tx via OZ Relayer, multiple signer types (passkeys, Ed25519, policies) | https://github.com/kalepail/smart-account-kit |
| **Stellar MCP Server** (kalepail) | MCP server running on Cloudflare Workers; exposes Stellar wallet, token, and contract tools to Claude and other AI clients | https://github.com/kalepail/stellar-mcp-server |
| **XDR MCP** (leighmcculloch) | MCP server that decodes and encodes Stellar XDR to/from JSON for AI agents | https://github.com/stellar-experimental/mcp-stellar-xdr |
| **Scaffold Stellar** | CLI tool for the full Stellar app development lifecycle — smart contract management, testing, and deployment with enforced best practices baked in. Install: `cargo install --locked stellar-scaffold-cli` | https://scaffoldstellar.org |
| **x402** | Per-request HTTP payment protocol for AI agents, powered by Soroban auth entry signing | https://developers.stellar.org/docs/build/apps/x402 |

### stellar-build: the full Stellar journey as Claude Code skills

stellar-build (https://github.com/kaankacar/stellar-build) is a one-command install that drops 42 skills plus curated Stellar ecosystem data into both `~/.claude/skills/` and `~/.codex/skills/`, so the same prompts work in Claude Code, Codex CLI, or any agent that reads those paths.

**Install:**

```bash
curl -fsSL https://raw.githubusercontent.com/kaankacar/stellar-build/main/install.sh | bash
```

**The 5-phase journey:**

| Phase | What it gives you |
|---|---|
| Idea | `find-stellar-idea`, `scf-round-watcher`, validation against the 728-project ecosystem |
| Planning | PRD, UX design, product brief — driven by SDF-DevRel-named personas |
| Solutioning | Architecture, epics, stories, plus the full `stellar/stellar-dev-skill` for Soroban + dapp + assets |
| Implementation | Story-driven dev, code review, debugging |
| Launch | Devnet→mainnet deploy plus 10 SCF grant lifecycle skills |

**Six AI agent personas** named for SDF DevRel team members — invoke any by name (`"talk to Tyler"`, `"Justin, who are my competitors?"`):

| Persona | Role |
|---|---|
| Justin | Business Analyst — market research, competitive analysis, requirements elicitation |
| Bri | Tech Writer — documentation, knowledge curation, technical writing |
| Nicole | Product Manager — PRDs, requirements discovery, stakeholder alignment |
| Kaan | UX Designer — interaction design, UX specifications |
| Tyler | System Architect — architecture decisions, Soroban contract design |
| Elliot | Senior Developer — code, story execution, implementation |

`/party-mode` spawns each persona as a real isolated subagent (one git worktree per persona). Your project folder needs to be a git repo with at least one commit for true multi-agent mode; otherwise it falls back to solo mode (single LLM, six voices). `git init && git commit --allow-empty -m "init"` is the fix.

**Curated data shipped with the install:**
- LumenLoop ecosystem catalog — 728 Stellar projects, SCF rounds, audits, tokens
- Electric Capital's Stellar developer taxonomy — 9,027 catalogued repos
- Stellar Foundation `ecosystem-resources` reference docs

**When it fits a hackathon:**
- You don't know what to build yet → `find-stellar-idea` + `scf-round-watcher`
- You want a paper trail (PRD, architecture decisions, stories) before writing code
- You plan to submit to SCF after the hackathon and want the grant skills already wired up
- You want a sounding-board team without recruiting one


### x402: HTTP Payments for AI Agents

x402 repurposes the HTTP 402 Payment Required status into a real payment mechanism. AI agents can autonomously pay for API calls without human intervention.

- **How it works:** Client hits a paywalled endpoint, receives a 402 with payment instructions, signs a Soroban auth entry, and retries with the payment header.
- **Supported wallets:** Freighter, Albedo, Hana, HOT, Klever, OneKey
- **Facilitator:** OpenZeppelin Relayer plugin (testnet + mainnet); see OpenZeppelin on Stellar below
- **Live demo:** https://x402-stellar-491bf9f7e30b.herokuapp.com/
- **Official monorepo:** https://github.com/stellar/x402-stellar (facilitator service, simple-paywall demo in Express + React, Heroku deployment config, high-throughput channel account setup)
- **Community demo** (jamesbachini): https://github.com/jamesbachini/x402-Stellar-Demo (minimal local demo showing payer client + protected Express server + local facilitator; good for understanding the flow end-to-end)

### OpenZeppelin on Stellar

OpenZeppelin's full platform for building and securing Stellar smart contracts: https://www.openzeppelin.com/networks/stellar

- **Stellar Contracts Library:** Audited, reusable Soroban components for asset issuance, stablecoins, and DeFi (the Stellar equivalent of OZ's battle-tested Ethereum libraries)
- **Contracts Wizard:** Visual configurator that generates audited Soroban contract code instantly, no boilerplate
- **Contracts MCP:** MCP server at https://mcp.openzeppelin.com/ (gives AI agents direct access to OZ contract generation tools; auto-installed with the Claude Code plugin above)
- **Relayer:** Simplifies backend transaction flow for anchors and apps (this is the facilitator used by x402 above)
- **Monitor:** Real-time anomaly detection and automated responses for deployed contracts
- **Soroban Security Detectors SDK:** Static analysis with prebuilt vulnerability checks for Soroban contracts


## 2. AI Coding Assistants

Tools for using AI while writing code. Most have generous free tiers.

| Tool | Free Tier | Best For | Link |
|---|---|---|---|
| **Claude Code** | Paid (or free via Ollama; see `Free_AI_Setup.md`) | Terminal-based agentic coding, full repo context | https://claude.com/product/claude-code |
| **Continue** | Fully free (open source) | VS Code/JetBrains, works with any local or cloud model | https://continue.dev |
| **Cursor** | 2,000 completions/mo | VS Code-like IDE with AI built in | https://cursor.com |
| **Aider** | Fully free | Terminal + Git integration, model-agnostic | https://aider.chat |
| **Windsurf** | 25 prompt credits/mo | Agentic IDE for large codebases | https://windsurf.com |
| **Jules** (Google) | 15 tasks/day | Autonomous GitHub-integrated coding agent (Gemini 2.5 Pro) | https://jules.google.com |
| **GitHub Copilot** | 2,000 completions + 50 chats/mo | In-editor suggestions, integrates into VS Code | https://github.com/features/copilot |


## 3. Multi-Agent Orchestration Frameworks

For teams building AI-powered applications, not just using AI to write code, but shipping products where AI agents do the work.

| Framework | Style | Link |
|---|---|---|
| **LangGraph** | Graph-based stateful workflows with explicit control flow | https://github.com/langchain-ai/langgraph |
| **CrewAI** | Role-driven agent teams, beginner-friendly API | https://github.com/joaomdmoura/crewai |
| **AutoGen (AG2)** | Conversation-based multi-agent collaboration | https://github.com/ag2-ai/ag2 |
| **OpenAI Agents SDK** | Production-grade, built-in tracing and agent handoffs | https://openai.github.io/openai-agents-python/ |


## 4. Rapid Prototyping and App Builders

Go from idea to deployed app fast. Useful for MVPs and demos.

| Tool | Free Tier | Best For | Link |
|---|---|---|---|
| **Bolt.new** | 1M tokens/mo | Full-stack app generated from a single prompt | https://bolt.new |
| **v0** (Vercel) | $5 credits/mo | React/Next.js UI components from text descriptions | https://v0.app |
| **Google AI Studio** | Fully free | Paste large codebases into a 1M-context window, Gemini 2.0 Flash | https://aistudio.google.com |
