# Personalized Home Gardening and Landscaping Platform

Parameterized template set for the **`home-gardening`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance generator
reads `blueprint.json.template` and resolves every `{{token}}` in these files to
emit a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A personalized, community-driven gardening and landscaping web app with:

- Hero + sticky **Header** with theme toggle and anchor navigation
- **Garden planner** — interactive garden layout with plants, beds, and features
- **Plant database** — searchable, filterable care and growth reference
- **Service provider directory** — vetted landscapers and designers with reviews
- **Care reminders** — automated watering, feeding, and maintenance schedules
- **Garden health tracker** — growth rates, pest sightings, and milestone badges
- **Seasonal guide** — what to plant and maintain through the year, by region

## Structure

```
templates/home-gardening/
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
│   │   └── gardening/
│   │       ├── CareReminders.tsx.template
│   │       ├── GardenHealthTracker.tsx.template
│   │       ├── GardenPlanner.tsx.template
│   │       ├── PlantDatabase.tsx.template
│   │       ├── SeasonalGuide.tsx.template
│   │       └── ServiceProviderDirectory.tsx.template
│   └── data/home-gardening.json.template  ← realistic sample content
```

## Parameterization points

| Namespace | Token | Meaning |
|---|---|---|
| `app` | `{{app.name}}` `{{app.slug}}` `{{app.tagline}}` | product identity |
| `theme` | `{{theme.color.primary}}` `{{theme.color.secondary}}` `{{theme.color.accent}}` `{{theme.font.sans}}` `{{theme.radius.card}}` | design system → CSS vars in `globals.css` |
| `hero` | `{{hero.headline}}` `{{hero.subtext}}` | landing hero copy |
| `header` | `{{header.cta_text}}` `{{header.cta_href}}` | call-to-action |
| `seo` | `{{seo.og_image}}` `{{seo.schema_type}}` | Open Graph / schema |
| `deployment` | `{{deployment.url}}` `{{deployment.analytics_id}}` | deploy target |
| `contact` | `{{contact.email}}` `{{contact.phone}}` | footer contact |
| `social` | `{{social.github}}` `{{social.linkedin}}` `{{social.twitter}}` | footer socials |
| `frontend/backend/ui/state/auth/security/ai/weather/maps/email/analytics/api/calendar/gamification` | framework & provider choices | prompt.md feature lines |

Business content is **never hard-coded** in components: plots, plants, providers,
care tasks, health logs and seasonal guides flow through
`src/data/home-gardening.json`, which `src/app/page.tsx` imports and passes to
typed props. Theme colors are consumed via `var(--color-*)` CSS custom
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
   imports and the import path `@/data/home-gardening.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.