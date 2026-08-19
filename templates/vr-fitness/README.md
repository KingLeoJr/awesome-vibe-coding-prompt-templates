# VrFitness — VR Fitness Training Platform Template Set

Parameterized template set for the **`vr-fitness`** blueprint
(category: `Specialized`, archetype: `ImmersiveXR`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind app.

## What it generates

A virtual reality fitness training web app with:

- **Header** — sticky brand, anchor navigation, CTA button, theme toggle
- **WorkoutLobby** — grid of VR fitness classes (yoga, HIIT, dance, strength, cardio) with filtering and selection
- **ImmersiveSession** — 3D VR workout session with progress bar and completion tracking
- **PerformanceMetrics** — workout analytics, calories burned, strength improvements, leaderboard
- **ClassBrowser** — filterable category browser for VR fitness classes
- **SocialFeed** — challenges, recent activities, user leaderboard with gamification
- **OnboardingTutorial** — guided onboarding for new VR fitness platform users

## Structure

```
templates/vr-fitness/
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
│   │   └── vr/
│   │       ├── WorkoutLobby.tsx.template
│ │       ├── ImmersiveSession.tsx.template
│ │       ├── PerformanceMetrics.tsx.template
│ │       ├── ClassBrowser.tsx.template
│ │       ├── SocialFeed.tsx.template
│ │       └── OnboardingTutorial.tsx.template
│   └── data/vr-fitness.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/payments/wearables/api` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample classes, workouts, progress, challenges and onboarding steps flow through
`src/data/vr-fitness.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/vr-fitness.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.