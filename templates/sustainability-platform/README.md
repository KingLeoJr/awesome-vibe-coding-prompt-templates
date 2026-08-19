# Environmental Awareness and Sustainability Platform - Template Set

> Template folder: `templates/sustainability-platform/`
> Blueprint slug: `sustainability-platform` - Archetype: `ContentPlatform` - Category: `Vertical`

A parameterized, production-ready **Next.js 15 + React 19 + Tailwind CSS** skeleton for an
environmental awareness and sustainability platform: searchable articles and resources,
a community-driven impact dashboard, local initiative maps, and a gamified event calendar.
The source prompt targeted **WordPress + Elementor + Bootstrap 5**; that stack is preserved in
`tech_stack` and `prompt.md.template`, while the rendered application is a modern React app.

## Structure

```
templates/sustainability-platform/
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
    |   `-- page.tsx.template                    <- home page: hero, articles, resources, impact, map, events
    |-- components/
    |   |-- layout/
    |   |   `-- Header.tsx.template              <- sticky header, "Get Involved" CTA, mobile menu
    |   `-- sustainability/
    |       |-- ArticleCard.tsx.template         <- blog/article card with topic + read time
    |       |-- ResourceLibrary.tsx.template     <- searchable library with topic/format filters
    |       |-- ImpactDashboard.tsx.template     <- personal + collective environmental impact stats
    |       `-- CommunityMap.tsx.template        <- local initiatives, recycling centers, gardens
    |-- data/
    |   `-- sustainability-platform.json.template <- realistic sample data
    `-- lib/
        `-- types.ts.template                    <- shared TypeScript interfaces
```

## What is parameterized

| Token | Where used | Example value |
|---|---|---|
| `{{app.name}}`, `{{app.tagline}}` | Header, Footer, page metadata, data file | Environmental Awareness and Sustainability Platform |
| `{{theme.color.primary}}` | Brand color (`var(--color-primary)`) | `#059669` |
| `{{theme.color.secondary}}` | Gradient endpoints, accents | `#0d9488` |
| `{{theme.color.accent}}` | Highlights, badges, gamification | `#84cc16` |
| `{{header.cta_text}}` / `{{header.cta_href}}` | Header CTA | `Get Involved` / `/join` |
| `{{hero.headline}}` / `{{hero.subtext}}` / `{{hero.placeholder}}` | Hero copy + search placeholder | - |
| `{{impact.unit}}` / `{{impact.goal_co2}}` | Impact dashboard metrics | `kg CO2e` / `10000` |
| `{{nav.*}}` | Header navigation + `site.config.ts` | Learn, Resources, Impact, Community |
| `{{seo.title}}` / `{{seo.og_image}}` | `page.tsx` metadata | - |
| `{{deployment.url}}` / `{{deployment.analytics_id}}` | `site.config.ts` | - |
| `{{contact.email}}` / `{{contact.phone}}` | Footer / contact links | - |
| `{{social.github}}` / `{{social.linkedin}}` / `{{social.twitter}}` | Footer social links | - |

## How the generator uses this set

1. **Load** - read `blueprint.json.template` (the metamodel record for this app).
2. **Walk** - walk every `*.template` file under this folder.
3. **Resolve** - replace `{{token}}` with values from the record, iterate `{{#each ...}}`
   blocks, honor `{{#if ...}}` conditionals.
4. **Emit mixins** - `templates/_shared/` is rendered to `src/components/shared/`
   (Header, Footer, ThemeToggle, Button, Card, SEOHead) plus `src/lib/utils.ts`.
5. **Output** - a complete Next.js project (`npm install && npm run dev`). Assumes standard
   Next scaffolding: `tsconfig` with `@/* -> src/*` and `resolveJsonModule`, Tailwind config
   with `darkMode: "class"`, and PostCSS wired for Tailwind.

## Notes and conventions

- The page is a server component (exports `metadata`); interactive sections are `"use client"`
  components that receive typed data via props.
- Realistic sample data lives in `src/data/sustainability-platform.json` - swap it for CMS data
  (the source prompt's WordPress custom post types map directly to `articles`, `resources`,
  `events`, and `mapPoints`).
- Gamification (badges/rewards) is modeled in the data file and surfaced by the impact
  dashboard; the community map renders local initiatives without external map dependencies
  (normalized coordinates), so it works offline and can be swapped for Google Maps.
- Dark mode uses `dark:` variants toggled by the shared `ThemeToggle`.