# Contributing

Most contributions to this repo fall into one of three buckets. Pick the one that matches your change and follow the conventions for it.


## 1. Adding a new country

Copy the skeleton:

```bash
cp -r countries/_template countries/<code>
```

`<code>` is the lowercase ISO 3166-1 alpha-2 code for the country (e.g., `ar` for Argentina, `co` for Colombia). Use the same letter case everywhere — folder name, links, table rows.

Then, inside the new folder:

1. Rewrite `README.md` for your country. Mirror the structure of `countries/mx/README.md` or `countries/br/README.md` — both are good references.
2. Add a `Dev_Setup_Guide.md` covering the local rails, the anchors with self-service developer flows, the local stablecoin issuers, and gotchas that have cost builders hours.
3. Add a `Hackathon_Resources.md` pointing at the regional starter pack, Stellar reference docs, and any country-specific anchor docs.
4. Add any country-only walkthroughs (e.g., Brazil has `PIX_Guide.md` because PIX is a self-contained rail with its own dance).
5. Update the root `README.md` "Supported countries" table to list the new country.

Do **not** add a `Free_AI_Setup.md` inside the country folder. Free AI tooling is country-agnostic and lives at the repo root (`../../Free_AI_Setup.md`). If a provider is geographically restricted in your country, mention it inside the relevant section of the root file rather than forking the doc.

Cross-link the four country-agnostic root docs as `../../<filename>.md` (the existing MX and BR READMEs show the pattern).


## 2. Editing the root country-agnostic docs

`Claude_Code_Guide.md`, `Free_AI_Setup.md`, `Recommended_AI_Tools.md`, and `Starter_Prompts.md` at the repo root must stay country-agnostic. If you find yourself adding country-specific assets, anchors, or rails to one of them, the change probably belongs in the relevant `countries/<code>/` folder instead.

When you do edit one of these root docs, scan all country READMEs for stale references — anchor lists, asset names, and protocol notes drift across countries over time.


## 3. Editing a country's guide

Stay inside `countries/<code>/`. Don't pull country-specific content up to the root just to dedupe — the per-country guides are intentionally allowed to drift, because the rails, anchors, and gotchas in each country drift independently.


## Style

- Markdown only. No frameworks, no build step.
- Keep links relative within the repo.
- Use H2 (`##`) for section headers inside guides, H1 (`#`) only for the doc title.
- When you cite an anchor, asset, or contract address, link to a primary source (the anchor's docs, Stellar Expert, or the asset issuer).
- When you flag a gotcha, write down both the symptom (what the builder will see) and the cause (what's actually happening).
