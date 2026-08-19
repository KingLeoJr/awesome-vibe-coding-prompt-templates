# Personalized Disaster Preparedness and Emergency Response Platform

Parameterized template set for the **`disaster-preparedness`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance
generator reads `blueprint.json.template` and resolves every `{{token}}` in
these files to emit a ready-to-build Next.js 15 + React 19 + TypeScript +
Tailwind project.

## What it generates

A marketing-and-app style preparedness platform with:

- Sticky **Header** with theme toggle, anchor navigation, and CTA
- **Emergency plan builder** — household contacts, meeting points, evacuation
  routes, important documents, and family-sharing status
- **Disaster checklists** — hazard-tabbed supply checklists (hurricane,
  earthquake, wildfire, flood) with checkable items and progress bars
- **Training modules** — first aid, CPR, and emergency-response courses with
  progress and completion tracking
- **Local resources directory** — searchable shelters, food banks, medical and
  utility contacts with distance and rating
- **Alert center** — local emergency and weather-warning feed with severity
  badges and dismiss states
- **Community forum** — preparedness discussions, questions, and peer support
- Footer with social links and contact info

## Structure

```
templates/disaster-preparedness/
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
│   │   └── emergency/
│   │       ├── PlanBuilder.tsx.template
│   │       ├── DisasterChecklists.tsx.template
│   │       ├── TrainingModules.tsx.template
│   │       ├── EmergencyResources.tsx.template
│   │       ├── AlertCenter.tsx.template
│   │       └── CommunityForum.tsx.template
│   └── data/disaster-preparedness.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/alerts/notifications/maps/ai` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: the emergency plan,
checklists, training modules, local resources, alerts and forum posts all flow
through `src/data/disaster-preparedness.json`, which `src/app/page.tsx` imports
and passes to typed props. Theme colors are consumed via `var(--color-*)` CSS
custom properties generated from `{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk the `src/`, `config/`, `styles/` and root files; materialize shared
   mixins from `templates/_shared/` into `src/components/shared/`
   (`ThemeToggle`, `Button`, `Card`, `Footer`, `SEOHead`, `site.config`,
   `package.json`, `utils`).
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per
   `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` in the page/data/package.json
   imports and the import path `@/data/disaster-preparedness.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.
- Add hazards/checklists in the data file — every tab renders automatically.
- Wire real alerts by swapping the data-file fetch for a NOAA/NWS WebSocket or
  polling hook at the page boundary.