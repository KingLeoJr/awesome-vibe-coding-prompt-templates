# SeoUtility — SEO Audit and Optimization Tool Template Set

Parameterized template set for the **`seo-utility`** blueprint
(category: `Utility`, archetype: `UtilityChecklist`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind app.

## What it generates

A polished SEO audit and optimization web tool with:

- **Header** — sticky brand, anchor navigation, CTA button, theme toggle
- **AuditChecklist** — comprehensive 35-item SEO audit form covering title tags, meta descriptions, headings, schema, sitemaps, page speed, internal links, canonical tags, hreflang, AMP, social media meta tags, image optimization, structured data, pagination, Core Web Vitals, international targeting, Google News, duplicate content, and more
- **ScoreGauge** — visual SEO score display (0-100) with category label and trend indicators
- **KeywordPlanner** — add and track keywords with search volume, difficulty, and priority rankings
- **ReportExporter** — export audit results as PDF, Excel, or HTML
- **BacklinkChecker** — analyze backlink profile with domain authority and toxic link detection

## Structure

```
templates/seo-utility/
├── README.md                                          ← this file
├── blueprint.json.template            ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + domain constants)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/Header.tsx.template
│   │   └── seo/
│   │       ├── AuditChecklist.tsx.template
│ │       ├── ScoreGauge.tsx.template
│ │       ├── KeywordPlanner.tsx.template
│ │       ├── ReportExporter.tsx.template
│ │       └── BacklinkChecker.tsx.template
│   └── data/seo-utility.json.template  ← realistic sample content
```

## Parameterization points

| Namespace | Token | Meaning |
|---|---|---|
| `app` | `{{app.name}}` `{{app.slug}}` `{{app.tagline}}` | product identity |
| `theme` | `{{theme.color.primary}}` `{{theme.color.secondary}}` `{{theme.color.accent}}` `{{theme.font.sans}}` `{{theme.radius.card}}` | design system → CSS vars in `globals.css` |
| `header` | `{{header.cta_text}}` `{{header.cta_href}}` | call-to-action |
| `seo` | `{{seo.og_image}}` `{{seo.schema_type}}` | Open Graph / schema |
| `deployment` | `{{deployment.url}}` `{{deployment.analytics_id}}` | deploy target |
| `contact` | `{{contact.email}}` `{{contact.phone}}` | footer contact |
| `social` | `{{social.github}}` `{{social.linkedin}}` `{{social.twitter}}` | footer socials |
| `frontend/backend/ui/state/auth/payments` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample audit results, keywords, scores, reports and backlinks flow through
`src/data/seo-utility.json`, which `src/app/page.tsx` imports and passes
to typed props. Theme colors are consumed via `var(--color-*)` CSS custom
properties generated from `{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk the `src/`, `config/`, `styles/` and root files; materialize shared
   mixins from `templates/_shared/` into `src/components/shared/`
   (`ThemeToggle`, `Button`, `Card`, `Footer`, `SEOHead`, `site.config`,
   `package.json`, `utils`).
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per
   `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` in the page/data/package.json
   imports and the import path `@/data/seo-utility.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.