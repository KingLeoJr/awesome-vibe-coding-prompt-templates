# Decentralized Finance (DeFi) Platform

Template set for a **DeFi hub** — wallet connection, token swapping,
liquidity pools (AMM), yield farming/staking, and on-chain governance. The
original prompt lists 58 DeFi primitives; this set realizes the core
experience as a production-grade Next.js + TypeScript + Tailwind application
and carries the remaining capabilities as ordered, priority-tagged backlog
items in the blueprint.

## Structure

```
templates/defi-platform/
├── README.md
├── blueprint.json.template          # metamodel record (the generator's config)
├── prompt.md.template               # rendered builder prompt for AI builder tools
├── package.json.template
├── config/site.config.ts.template
├── styles/globals.css.template
└── src/
    ├── app/page.tsx.template
    ├── data/defi-platform.json.template
    ├── components/
    │   ├── layout/Header.tsx.template
    │   └── defi/
    │       ├── WalletConnect.tsx.template
    │       ├── SwapInterface.tsx.template
    │       ├── LiquidityPool.tsx.template
    │       └── YieldFarm.tsx.template
```

## Generated project layout

Shared mixins are emitted into the conventional locations so every import
resolves:

| Source template | Generated path |
|---|---|
| `templates/_shared/ThemeToggle.tsx.template` | `src/components/shared/ThemeToggle.tsx` |
| `templates/_shared/SEOHead.tsx.template` | `src/components/shared/SEOHead.tsx` |
| `templates/_shared/Footer.tsx.template` | `src/components/shared/Footer.tsx` |
| `templates/_shared/Button.tsx.template` | `src/components/shared/Button.tsx` |
| `templates/_shared/Card.tsx.template` | `src/components/shared/Card.tsx` |
| `templates/_shared/utils.ts.template` | `src/lib/utils.ts` |
| `templates/_shared/package.json.template` *(overridden)* | `package.json` |

This set ships its own `config/site.config.ts` (adds chain-aware constants)
and `package.json` (adds `web3`). A minimal `src/app/layout.tsx` and
`next.config.mjs` come from the archetype bundle / scaffold.

## Parameterization points

| Token | Meaning | Source |
|---|---|---|
| `{{app.name}}` / `{{app.slug}}` / `{{app.tagline}}` | branding | blueprint + `extra_config.app` |
| `{{theme.color.primary}}` / `.secondary` / `.accent` | theme palette (hex) | `extra_config.theme.color` |
| `{{theme.font.heading}}` / `.body` | typography | `extra_config.theme.font` |
| `{{theme.radius}}` | card radius | `extra_config.theme` |
| `{{header.cta_text}}` / `{{header.cta_href}}` | header CTA (wallet) | `extra_config.header` |
| `{{chain.network}}` / `{{chain.explorer}}` / `{{chain.rpc_url}}` | chain context | `extra_config.chain` |
| `{{deployment.url}}` / `{{deployment.analytics_id}}` | deployment | `deployment[].config` (flattened) |
| `{{seo.og_image}}` / `{{seo.schema_type}}` | SEO | `extra_config.seo` |
| `{{contact.*}}` / `{{social.*}}` | footer/contact | `extra_config.contact` / `.social` |
| `{{#each sections}}` | nav + section render order | `sections[]` |
| `{{#each features}}` | requirement backlog | `features[]` |
| `{{#each data_models}}` | domain entities | `data_models[]` |

Design tokens are hex values consumed via CSS variables
(`bg-[var(--color-primary)]`, `from-[var(--color-primary)]`, …) defined in
`styles/globals.css`, so generated Tailwind classes stay valid for any palette.

### Business data

`src/data/defi-platform.json` is the single content source — tokens (with
oracle prices), pools, farms, governance proposals, and dashboard stats. The
page imports it and passes slices to components; each component defaults its
props to the same file for standalone rendering. Real wallet integration
(`web3.js`), price feeds (`Chainlink`), and transaction signing are stubbed
behind the component props — wire them to env-backed providers per the
`config/site.config.ts` `CHAIN` block.

## Generating an app

```bash
generator generate defi-platform --out ./out/defihub
cd ./out/defihub && npm install && npm run dev
```

`npm run build`, `npm run typecheck`, and `npm run lint` pass clean on the
rendered project (JSON imports rely on Next.js default `resolveJsonModule`).