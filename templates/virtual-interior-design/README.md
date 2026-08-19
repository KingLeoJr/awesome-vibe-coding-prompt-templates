# Virtual Interior Design Platform

Parameterized template set for the **`virtual-interior-design`** blueprint
(category: `Specialized`, archetype: `ImmersiveXR`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A immersive virtual interior design platform with:

- **RoomVisualizer** — 3D room design tool with floor plan creation and drag-and-drop furniture placement
- **FurniturePicker** — curated library of furniture and decor items with filtering by style, room type, and price
- **MoodBoard** — digital mood board creator with color palettes, texture selection, and item collaging
- **ColorPalette** — interactive color scheme designer with harmonious palettes and real-time preview
- **ProjectGallery** — showcase and browse completed design projects with filtering and submission
- **ProjectDashboard** — manage design projects with budgets, timelines, to-do lists, and designer communication
- **CommunityForum** — share projects, ask questions, and get feedback from design community
- **ResourceLibrary** — browse articles, videos, and DIY guides on interior design principles and trends
- **BudgetTool** — interactive cost estimator based on selected furniture and decor
- **ColorSchemeDesigner** — real-time color harmony generator with accessible contrast ratios
- **VirtualConsultation** — video call booking with professional interior designers
- **InspirationStream** — curated feed of trending designs and user submissions

## Structure

```
templates/virtual-interior-design/
├── README.md
├── blueprint.json.template          ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + domain constants)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/
│   │   │   └── Header.tsx.template
│   │   └── design/
│   │       ├── RoomVisualizer.tsx.template
│   │       ├── FurniturePicker.tsx.template
│   │       ├── MoodBoard.tsx.template
│   │       ├── ColorPalette.tsx.template
│   │       ├── ProjectGallery.tsx.template
│   │       └── ProjectDashboard.tsx.template
│   └── data/virtual-interior-design.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample rooms, furniture,
mood boards, budgets and designer bookings flow through
`src/data/virtual-interior-design.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/virtual-interior-design.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.