# DirectoryWebsite - Template Set

> Template folder: `templates/directory-website/`
> Blueprint slug: `directory-website` - Archetype: `DirectoryListing` - Category: `Template`

A parameterized, production-ready **Next.js 15 + React 19 + Tailwind CSS** skeleton for a generic,
industry-agnostic directory website. The source prompt is written against a `{industry}` placeholder;
this template set lifts that to first-class parameters (`{{industry.name}}`, `{{industry.criteria}}`)
so one generator pass produces a directory for home services, restaurants, clinics, agencies, or
any other vertical.

## Structure

```
templates/directory-website/
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
    |   `-- page.tsx.template                    <- home page: hero, grid, search, gallery, reviews, compare
    |-- components/
    |   |-- layout/
    |   |   `-- Header.tsx.template              <- sticky header, "Add Listing" CTA, mobile menu
    |   `-- directory/
    |       |-- ListingCard.tsx.template         <- grid card with hover effects + quick view
    |       |-- SearchFilters.tsx.template       <- advanced search with industry-specific filters
    |       |-- ListingGallery.tsx.template      <- lazy-loaded gallery with lightbox
    |       `-- ReviewSystem.tsx.template        <- reviews, verified badges, interactive rating
    |-- data/
    |   `-- directory-website.json.template      <- realistic sample data (industry-driven)
    `-- lib/
        `-- types.ts.template                    <- shared TypeScript interfaces
```

## What is parameterized

| Token | Where used | Example value |
|---|---|---|
| `{{app.name}}`, `{{app.tagline}}` | Header, Footer, page metadata, data file | DirectoryWebsite |
| `{{industry.name}}` | Data file copy, section headings | `home services` |
| `{{industry.criteria}}` | Advanced search filter labels | service type, price, rating, distance |
| `{{industry.listing}}` | Listing terminology (business/service/venue) | `business` |
| `{{theme.color.primary}}` | Brand color (`var(--color-primary)`) | `#4f46e5` |
| `{{theme.color.secondary}}` | Gradient endpoints, accents | `#0891b2` |
| `{{theme.color.accent}}` | Highlights, badges | `#f43f5e` |
| `{{header.cta_text}}` / `{{header.cta_href}}` | Header CTA | `Add Listing` / `/listings/new` |
| `{{hero.headline}}` / `{{hero.subtext}}` / `{{hero.placeholder}}` | Hero copy + search placeholder | - |
| `{{nav.*}}` | Header navigation + `site.config.ts` | Browse, Add Listing, Pricing, Blog, FAQ |
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

- The page is a server component (exports `metadata`); all interactive sections are
  `"use client"` components that receive typed data via props.
- `src/data/directory-website.json` is the single source of realistic sample data; changing
  the `industry` parameter re-skins the copy throughout.
- Dark mode uses `dark:` variants toggled by the shared `ThemeToggle`.
- The source prompt's long page list (pricing, blog, FAQ, help center, careers, etc.) maps to
  routes that reuse these components; the README's data file already carries FAQ and
  testimonial content to feed those pages.