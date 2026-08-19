# Blockchain-based Supply Chain Management Platform

Template set for a **blockchain-anchored supply chain management platform**.
The original builder prompt lists 58 blockchain/IoT/AI capabilities; this set
realizes the core experience — shipment tracking, chain-of-custody timelines,
product authentication, and supplier reputation — as a production-grade
Next.js + TypeScript + Tailwind application, with the remaining features
carried as ordered, priority-tagged backlog items in the blueprint.

## Structure

```
templates/blockchain-supply-chain/
├── README.md
├── blueprint.json.template          # metamodel record (the generator's config)
├── prompt.md.template               # rendered builder prompt for AI builder tools
├── package.json.template
├── config/site.config.ts.template
├── styles/globals.css.template
└── src/
    ├── app/page.tsx.template
    ├── data/blockchain-supply-chain.json.template
    ├── components/
    │   ├── layout/Header.tsx.template
    │   └── supply-chain/
    │       ├── ShipmentCard.tsx.template
    │       ├── TrackingTimeline.tsx.template
    │       ├── ProductAuthentication.tsx.template
    │       └── SupplierReputation.tsx.template
```

## Generated project layout

The generator writes the output tree with shared mixins emitted into the
conventional locations so every import resolves:

| Source template | Generated path |
|---|---|
| `templates/_shared/Header.tsx.template` *(not used — local header)* | — |
| `templates/_shared/ThemeToggle.tsx.template` | `src/components/shared/ThemeToggle.tsx` |
| `templates/_shared/SEOHead.tsx.template` | `src/components/shared/SEOHead.tsx` |
| `templates/_shared/Footer.tsx.template` | `src/components/shared/Footer.tsx` |
| `templates/_shared/Button.tsx.template` | `src/components/shared/Button.tsx` |
| `templates/_shared/Card.tsx.template` | `src/components/shared/Card.tsx` |
| `templates/_shared/utils.ts.template` | `src/lib/utils.ts` |
| `templates/_shared/site.config.ts.template` *(overridden — see below)* | `config/site.config.ts` |

This set ships its own `config/site.config.ts` (adds `NETWORK`,
chain-aware constants) and `package.json` (adds `ethers`), overriding the
shared versions. `next.config.mjs` and a minimal `src/app/layout.tsx` that
renders `<html lang="en">` + `<body>` are expected from the archetype bundle
or generated project scaffold; this set intentionally omits them because they
carry no app-specific parameterization.

## Parameterization points

| Token | Meaning | Source |
|---|---|---|
| `{{app.name}}` / `{{app.slug}}` / `{{app.tagline}}` | branding | blueprint + `extra_config.app` |
| `{{theme.color.primary}}` / `.secondary` / `.accent` | theme palette (hex) | `extra_config.theme.color` |
| `{{theme.font.heading}}` / `.body` | typography | `extra_config.theme.font` |
| `{{theme.radius}}` | card radius | `extra_config.theme` |
| `{{header.cta_text}}` / `{{header.cta_href}}` | header CTA | `extra_config.header` |
| `{{chain.network}}` / `{{chain.explorer}}` / `{{chain.contract_address}}` | chain context | `extra_config.chain` |
| `{{deployment.url}}` / `{{deployment.analytics_id}}` | deployment | `deployment[].config` (flattened) |
| `{{seo.og_image}}` / `{{seo.schema_type}}` | SEO | `extra_config.seo` |
| `{{contact.*}}` / `{{social.*}}` | footer/contact | `extra_config.contact` / `.social` |
| `{{#each sections}}` | nav + section render order | `sections[]` |
| `{{#each features}}` | requirement backlog | `features[]` |
| `{{#each data_models}}` | domain entities | `data_models[]` |

### How the tokens map to the DOM

Design tokens are hex values in `design_tokens[]`. Components reference them
through CSS variables — e.g. `bg-[var(--color-primary)]`,
`from-[var(--color-primary)] to-[var(--color-secondary)]` — which are defined
in `styles/globals.css` from the same tokens. This keeps generated Tailwind
class names valid regardless of the hex value chosen at generation time.

### Business data

No business value is hard-coded in components. `src/data/blockchain-supply-chain.json`
holds the sample content (`stats`, `shipments`, `trackingEvents`, `products`,
`suppliers`); components import named collections from it as prop defaults,
and the page imports the same file to compose sections. Swap this file (or
point components at a live API) to customize content without touching markup.

## Generating an app

```bash
generator generate blockchain-supply-chain --out ./out/chainlogix
cd ./out/chainlogix && npm install && npm run dev
```

The generator must enable JSON imports (Next.js default tsconfig enables
`resolveJsonModule`). `npm run build`, `npm run typecheck`, and `npm run lint`
should pass clean on the rendered project.
