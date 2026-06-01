# Stellar AI Guide — [Country Name]

> Replace this template with a country-specific guide. See `CONTRIBUTING.md` at the repo root for the full checklist.

[One-paragraph intro for this country: what makes the local payment rail interesting (instant settlement? remittance corridor? FX volume?), and how Stellar slots into it.]

**About to open Claude Code for the first time?** Start with `../../Starter_Prompts.md`.

**Building with [Local Currency] rails?** [Name the primary anchor for this country and the rail it covers. Link to the relevant section of `Dev_Setup_Guide.md`.]

**Don't have a paid AI subscription?** Start with `../../Free_AI_Setup.md` at the repo root — it's country-agnostic and applies to every country folder.


## Read order

1. `../../Starter_Prompts.md` before your first Claude Code session
2. `../../Free_AI_Setup.md` if you need free AI access
3. `Dev_Setup_Guide.md` for the protocol-by-protocol setup, anchor auth, and gotchas for this country
4. `Hackathon_Resources.md` to orient yourself in the Stellar ecosystem for this region
5. `../../Claude_Code_Guide.md` for commands, parallel agents, and browser automation
6. `../../Recommended_AI_Tools.md` to explore what else is available


## What's in this folder

- `Dev_Setup_Guide.md` — country-specific anchors, assets, rails, and gotchas
- `Hackathon_Resources.md` — regional starter pack, reference implementations, ecosystem links
- `[Optional] <Rail>_Guide.md` — a self-contained guide for the country's primary payment rail if it deserves its own doc (e.g., `PIX_Guide.md` in `countries/br/`)

Free AI tooling lives at the repo root (`../../Free_AI_Setup.md`) because the providers and setup don't change country to country. Don't duplicate it inside a country folder.


## Conventions

- Use lowercase ISO 3166-1 alpha-2 codes for country folder names.
- Link to shared docs with `../../<filename>.md`.
- Link to sibling docs inside this folder with just the filename.
