# Marketplace for Handmade Goods Template

Parameterized template set for the **`handmade-marketplace`** blueprint
(category: `Template`, archetype: `Marketplace`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project that
serves as the marketplace frontend reference for the Rails + React ecosystem
described in the source prompt.

## What it generates

A marketplace storefront web app with:

- Sticky **Header** with theme toggle, anchor navigation, and a seller CTA
- **Browse** — interactive product grid with search plus category, location,
  price-range, and rating filters (`CategoryFilter` + `ProductCard`)
- **Makers** — spotlight cards for featured artisan vendors
- **Cart** — editable cart with quantity steppers, remove, and live totals
- **Analytics** — vendor dashboard with sales trend bars, top products, and
  low-inventory alerts
- **Reviews** — buyer review feed with star ratings and verified badges

## Structure

```
templates/handmade-marketplace/
├── README.md
├── blueprint.json.template          ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + marketplace defaults)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/Header.tsx.template
│   │   └── marketplace/
│   │       ├── CategoryFilter.tsx.template
│   │       ├── FeaturedMaker.tsx.template
│   │       ├── ProductCard.tsx.template
│   │       ├── ReviewsSection.tsx.template
│   │       ├── ShoppingCart.tsx.template
│   │       └── VendorDashboard.tsx.template
│   └── data/handmade-marketplace.json.template  ← realistic sample content
```

## Parameterization points

| Namespace | Token | Meaning |
|---|---|---|
| `app` | `{{app.name}}` `{{app.slug}}` `{{app.tagline}}` | product identity |
| `theme` | `{{theme.color.primary}}` `{{theme.color.secondary}}` `{{theme.color.accent}}` `{{theme.font.sans}}` `{{theme.radius.card}}` | design system → CSS vars in `globals.css` |
| `header` | `{{header.cta_text}}` `{{header.cta_href}}` `{{header.is_fixed}}` | call-to-action |
| `seo` | `{{seo.og_image}}` `{{seo.schema_type}}` | Open Graph / schema |
| `deployment` | `{{deployment.url}}` `{{deployment.analytics_id}}` | deploy target |
| `contact` | `{{contact.email}}` `{{contact.phone}}` | footer contact |
| `social` | `{{social.github}}` `{{social.linkedin}}` `{{social.twitter}}` | footer socials |
| `currency` | `{{currency.symbol}}` | price formatting |
| `frontend/backend/database/auth/ui/state/payments/email/shipping/realtime/analytics` | framework & provider choices | prompt.md `params:` block |

Business content is **never hard-coded** in components: products, makers, cart
items, vendor stats, and reviews flow through
`src/data/handmade-marketplace.json`, which `src/app/page.tsx` imports and
passes to typed props. Theme colors are consumed via `var(--color-*)` CSS
custom properties generated from `{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk the `src/`, `config/`, `styles/` and root files; materialize shared
   mixins from `templates/_shared/` into `src/components/shared/`
   (`ThemeToggle`, `Button`, `Card`, `Footer`, `SEOHead`, `site.config`,
   `package.json`, `utils`).
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per
   `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` in the data import path and
   `package.json` name align by construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.