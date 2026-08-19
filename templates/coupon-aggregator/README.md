# Coupon Aggregator Website Template

Template set for a **coupon and deal aggregator** — searchable, category-based
deals, expiring-soon alerts, seasonal trend timeline, and savings-impact
reporting. The original prompt targets a PHP (Laravel) + Vue.js + Bootstrap 5
stack; this template set adapts that scope to the repo's Next.js + TypeScript +
Tailwind ecosystem while keeping every requirement as an ordered backlog item.

## Structure

```
templates/coupon-aggregator/
├── README.md
├── blueprint.json.template          # metamodel record (the generator's config)
├── prompt.md.template               # rendered builder prompt for AI builder tools
├── package.json.template
├── config/site.config.ts.template
├── styles/globals.css.template
└── src/
    ├── app/page.tsx.template
    ├── data/coupon-aggregator.json.template
    ├── components/
    │   ├── layout/Header.tsx.template
    │   └── coupons/
    │       ├── CouponCard.tsx.template
    │       ├── CouponBrowser.tsx.template
    │       ├── ExpiringTimeline.tsx.template
    │       └── SavingsImpact.tsx.template
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

This set ships its own `config/site.config.ts` (adds locale/currency-aware
constants) and `package.json`. A minimal `src/app/layout.tsx` and
`next.config.mjs` come from the archetype bundle / scaffold.

## Parameterization points

| Token | Meaning | Source |
|---|---|---|
| `{{app.name}}` / `{{app.slug}}` / `{{app.tagline}}` | branding | blueprint + `extra_config.app` |
| `{{theme.color.primary}}` / `.secondary` / `.accent` | theme palette (hex) | `extra_config.theme.color` |
| `{{theme.font.heading}}` / `.body` | typography | `extra_config.theme.font` |
| `{{theme.radius}}` | card radius | `extra_config.theme` |
| `{{header.cta_text}}` / `{{header.cta_href}}` | header CTA | `extra_config.header` |
| `{{i18n.default_locale}}` / `{{i18n.currency}}` | locale + currency | `extra_config.i18n` |
| `{{deployment.url}}` / `{{deployment.analytics_id}}` | deployment | `deployment[].config` (flattened) |
| `{{seo.og_image}}` / `{{seo.schema_type}}` | SEO | `extra_config.seo` |
| `{{contact.*}}` / `{{social.*}}` | footer/contact | `extra_config.contact` / `.social` |
| `{{#each sections}}` | nav + section render order | `sections[]` |
| `{{#each features}}` | requirement backlog | `features[]` |
| `{{#each data_models}}` | domain entities | `data_models[]` |

Design tokens are hex values; components consume them via CSS variables
(`bg-[var(--color-primary)]`, etc.) defined in `styles/globals.css`, keeping
generated Tailwind classes valid for any hex value.

### Business data

`src/data/coupon-aggregator.json` is the single content source — categories,
coupons, expiring deals, seasonal trends, impact stats, blog posts, and FAQs.
The page imports it and passes slices to components; each component also
defaults its props to the same file for standalone rendering.

## Generating an app

```bash
generator generate coupon-aggregator --out ./out/couponhub
cd ./out/couponhub && npm install && npm run dev
```

`npm run build`, `npm run typecheck`, and `npm run lint` pass clean on the
rendered project (JSON imports rely on Next.js default `resolveJsonModule`).