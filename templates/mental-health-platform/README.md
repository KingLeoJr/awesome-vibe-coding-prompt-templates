# Mental Health and Wellness Platform

Parameterized template set for the **`mental-health-platform`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance
generator reads `blueprint.json.template` and resolves every `{{token}}` in
these files to emit a ready-to-build Next.js 15 + React 19 + TypeScript +
Tailwind project that serves as the wellness frontend reference for the Django
+ React ecosystem described in the source prompt.

## What it generates

A calming, personalized wellness web app with:

- Sticky **Header** with theme toggle, anchor navigation, and a care CTA
- **Resources** — library of articles, videos, podcasts, and worksheets
  filtered by topic and format
- **Mood** — interactive mood tracker with daily mood/energy logging and a
  weekly pattern view
- **Check-in** — interactive self-assessment quiz with progress and a
  personalized result summary
- **Therapists** — licensed professional cards with specialties, modalities,
  languages, and booking availability
- **Community** — supportive forum feed with like and reply engagement
- **Insights** — wellness dashboard with engagement stats, weekly activity, and
  topics covered

## Structure

```
templates/mental-health-platform/
├── README.md
├── blueprint.json.template          ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + wellness defaults)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/Header.tsx.template
│   │   └── wellness/
│   │       ├── CommunityFeed.tsx.template
│   │       ├── MoodTracker.tsx.template
│   │       ├── ResourceCard.tsx.template
│   │       ├── SelfAssessment.tsx.template
│   │       ├── TherapistCard.tsx.template
│   │       └── WellnessDashboard.tsx.template
│   └── data/mental-health-platform.json.template  ← realistic sample content
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
| `frontend/backend/database/auth/ui/state/realtime/video/calendar/ai/analytics` | framework & provider choices | prompt.md `params:` block |

Business content is **never hard-coded** in components: resources, mood logs,
assessment questions, therapists, community posts, and wellness metrics flow
through `src/data/mental-health-platform.json`, which `src/app/page.tsx`
imports and passes to typed props. Theme colors are consumed via
`var(--color-*)` CSS custom properties generated from `{{theme.color.*}}`.

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
- This template ships a calm, accessible UI; keep contrast ratios and
  reduced-motion behavior intact when re-theming.