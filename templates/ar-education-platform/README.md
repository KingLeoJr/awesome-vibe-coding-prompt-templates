# Augmented Reality (AR) Education Platform

Parameterized template set for the **`ar-education-platform`** blueprint
(category: `Specialized`, archetype: `ImmersiveXR`). The instance generator
reads `blueprint.json.template` and resolves every `{{token}}` to emit a
ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

> The template ships as a responsive web shell for the platform (catalog,
> scanning flow, labs, quests, exploration). Native AR rendering — ARKit/ARCore,
> Photon multiplayer, OpenCV triggers — plugs into the WebXR/camera adapters
> described in `prompt.md.template`.

## What it generates

A child-friendly AR learning platform marketing-and-app shell:

- Sticky **Header** with theme toggle and section navigation
- **AR book scanner** — pick a book, run a simulated scan flow, unlock the
  linked AR lesson
- **Lesson catalog** — filterable, searchable library of AR/3D/interactive
  lessons across subjects and grade levels
- **Virtual lab** — science experiments with materials, safety and step counts
- **Quest tracker** — gamified progress: XP, level, streak, badges and active
  quests with animated progress
- **AR explorer** — field-trip destinations with AR landmark overlays

## Structure

```
templates/ar-education-platform/
├── README.md
├── blueprint.json.template          ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + domain constants)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/Header.tsx.template
│   │   └── education/
│   │       ├── ARBookScanner.tsx.template
│   │       ├── ARExplorer.tsx.template
│   │       ├── LessonCatalog.tsx.template
│   │       ├── ProgressTracker.tsx.template
│   │       └── VirtualLab.tsx.template
│   └── data/ar-education-platform.json.template  ← realistic sample content
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
| `frontend/ar/native/networking/cv/auth` | platform & adapter choices | prompt.md `## Stack` |

All lesson, book, lab, quest and destination content flows through
`src/data/ar-education-platform.json` — never hard-coded in components. Theme
colors are consumed via `var(--color-*)` CSS custom properties generated from
`{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk `src/`, `config/`, `styles/` and root files; materialize shared mixins
   from `templates/_shared/` into `src/components/shared/`.
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per
   `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` keeps imports and the data file
   aligned by construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts JSON data to exported component prop types; keep the
  shapes in `src/data/*.json` in sync with those interfaces.
- The scanner, explorer and lab components model the AR interaction flows with
  animated UI states; wire real camera feeds, ARCore/ARKit bridges and Photon
  sessions per the adapter contract in `prompt.md.template`.