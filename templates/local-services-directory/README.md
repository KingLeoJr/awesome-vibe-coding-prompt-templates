# Directory for Local Services - Template Set

> Template folder: `templates/local-services-directory/`
> Blueprint slug: `local-services-directory` - Archetype: `DirectoryListing` - Category: `Template`

A parameterized, production-ready **Next.js 15 + React 19 + Tailwind CSS** skeleton for an online
directory of local service providers (plumbing, electrical, cleaning, landscaping, HVAC, painting).
The original builder prompt targeted **Laravel + Vue.js + Bootstrap 5**; that source stack is preserved
in the blueprint's `tech_stack` and `prompt.md.template` so the generator can emit either variant,
while the rendered application is a modern, buildable React app.

## Structure

```
templates/local-services-directory/
|-- README.md                                    <- this file
|-- blueprint.json.template                      <- metamodel record (generator configuration)
|-- prompt.md.template                           <- original builder prompt, parameterized
|-- package.json.template                        <- npm scripts + dependencies
|-- config/
|   `-- site.config.ts.template                  <- site constants (title, nav, social, analytics)
|-- styles/
|   `-- globals.css.template                     <- Tailwind directives + design-token CSS variables
`-- src/
    |-- app/
    |   `-- page.tsx.template                    <- home page assembling every section
    |-- components/
    |   |-- layout/
    |   |   `-- Header.tsx.template              <- sticky header, mobile menu, theme toggle, CTA
    |   `-- directory/
    |       |-- ListingCard.tsx.template         <- featured provider card (uses _shared/Card)
    |       |-- SearchExplorer.tsx.template      <- hero search, category chips, filters
    |       |-- BookingWidget.tsx.template       <- pick provider + slot, confirm booking
    |       |-- ReviewTimeline.tsx.template      <- chronological rating/review timeline
    |       `-- ProviderDashboard.tsx.template   <- provider analytics (stats + weekly chart)
    |-- data/
    |   `-- local-services-directory.json.template <- realistic sample data
    `-- lib/
        `-- types.ts.template                    <- shared TypeScript interfaces
```

## What is parameterized

Every token below is resolved by the generator from the metamodel record
(`blueprint.json.template`, especially `extra_config`):

| Token | Where used | Example value |
|---|---|---|
| `{{app.name}}`, `{{app.tagline}}` | Header, Footer, page metadata, data file | Directory for Local Services Template |
| `{{theme.color.primary}}` | Brand color everywhere (`var(--color-primary)`) | `#2563eb` |
| `{{theme.color.secondary}}` | Gradient endpoints, accents | `#059669` |
| `{{theme.color.accent}}` | Highlights (ratings, alerts) | `#f59e0b` |
| `{{theme.font.family}}` | `globals.css` base font stack | system sans-serif stack |
| `{{header.cta_text}}` / `{{header.cta_href}}` | Header CTA | `List your service` / `/providers/new` |
| `{{hero.headline}}` / `{{hero.subtext}}` / `{{hero.placeholder}}` | Search hero copy + placeholder | - |
| `{{nav.*}}` | Header navigation + `site.config.ts` | Browse, Bookings, Reviews, For Providers |
| `{{seo.title}}` / `{{seo.og_image}}` | `page.tsx` metadata | - |
| `{{payments.provider}}` | Booking widget trust note | `Stripe` |
| `{{deployment.url}}` / `{{deployment.analytics_id}}` | `site.config.ts` | - |
| `{{contact.email}}` / `{{contact.phone}}` | Footer / contact links | - |
| `{{social.github}}` / `{{social.linkedin}}` / `{{social.twitter}}` | Footer social links | - |

## How the generator uses this set

1. **Load** - the generator reads `blueprint.json.template` (the metamodel record for this app).
2. **Walk** - it walks every `*.template` file under this folder.
3. **Resolve** - it replaces `{{token}}` with values from the record, iterates `{{#each ...}}`
   blocks (e.g. `{{#each nav}}` in the header), and honors `{{#if ...}}` conditionals.
4. **Emit mixins** - shared mixins under `templates/_shared/` are rendered to
   `src/components/shared/` (Header, Footer, ThemeToggle, Button, Card, SEOHead) and
   `src/lib/utils.ts` so local components can import `@/components/shared/...`.
5. **Output** - the result is a complete Next.js project:
   `npm install && npm run dev` (assumes standard Next.js scaffolding: `tsconfig` with
   `@/* -> src/*` paths + `resolveJsonModule`, a Tailwind config with `darkMode: "class"`,
   and PostCSS wired for Tailwind).

## Notes and conventions

- All interactive components carry `"use client"`; the page itself stays a server component.
- Components are typed via `src/lib/types.ts` and receive data through props (never
  hard-coded business values). Realistic sample data lives in
  `src/data/local-services-directory.json`.
- Dark mode is implemented with `dark:` variants toggled by the shared `ThemeToggle`.
- Extensions mentioned in the source prompt (community forum, referral program, loyalty,
  third-party API, dispute resolution) map to data models and integrations in the blueprint
  and can be rendered as additional routes by the generator.
