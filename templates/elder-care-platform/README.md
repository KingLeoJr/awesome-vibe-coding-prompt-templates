# Personalized Elder Care and Support Platform

Parameterized template set for the **`elder-care-platform`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance
generator reads `blueprint.json.template` and resolves every `{{token}}` in
these files to emit a ready-to-build Next.js 15 + React 19 + TypeScript +
Tailwind project.

## What it generates

A marketing-and-app style elder care platform with:

- Sticky **Header** with theme toggle, anchor navigation, and CTA
- **Care plan builder** — patient summary, daily routine timeline, medication
  schedule with taken-tracking, and appointment reminders
- **Caregiver matching** — vetted caregiver profiles with qualifications,
  experience, specialties, availability, and family reviews
- **Service directory** — searchable local services (home health aides, meal
  delivery, transportation, social activities) with ratings
- **Health tracker** — vital sign tiles, an SVG trend chart, and an inline
  "log reading" form
- **Events calendar** — local senior events and wellness workshops with
  category filters and RSVP toggles
- **Resource library** — articles, videos, and guides filtered by topic
- Footer with social links and contact info

## Structure

```
templates/elder-care-platform/
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
│   │   └── care/
│   │       ├── CarePlanBuilder.tsx.template
│   │       ├── CaregiverMatching.tsx.template
│   │       ├── ServiceDirectory.tsx.template
│   │       ├── HealthTracker.tsx.template
│   │       ├── EventsCalendar.tsx.template
│   │       └── ResourceLibrary.tsx.template
│   └── data/elder-care-platform.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/payments/notifications/telehealth/analytics/ai` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: the care plan,
caregivers, services, vital-sign series, events and resources all flow through
`src/data/elder-care-platform.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/elder-care-platform.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.
- Add medications/routines in the data file — the care plan renders
  automatically.
- Wire real vital-sign data by replacing the data-file fetch with an API hook
  at the page boundary.