# Personalized Online Fitness Coaching Platform

Parameterized template set for the **`online-fitness-coaching`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A personalized fitness coaching web app with:

- **Header** — sticky navigation with brand, anchor navigation, CTA, theme toggle
- **Hero/Overview section** — user stats, progress summary
- **Workout Plan Card** — showcase workout plans with animated reveal
- **Coach Profile** — certified trainer information and credentials
- **Progress Tracker** — weight loss/muscle gain visualization over time
- **Nutrition Tracker** — meal logging, calorie tracking, dietary suggestions
- **Video Library** — exercise demonstrations across fitness levels
- **Community Forum** — user discussions, fitness tips, and support
- **Analytics Dashboard** — activity insights, goal achievement tracking
- **Resource Center** — articles, videos, and tips on fitness, nutrition, and well-being
- **Emergency Contact** — local emergency numbers based on user location
- **Gamification** — badges, challenges, and daily motivational quotes
- **Referral Program** — rewards for inviting friends
- **Mobile Optimized** — responsive design from 320px to 4K

## Structure

```
templates/online-fitness-coaching/
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
│   │   └── fitness/
│   │       ├── WorkoutPlanCard.tsx.template
│   │       ├── CoachProfile.tsx.template
│   │       ├── ProgressTracker.tsx.template
│   │       ├── NutritionTracker.tsx.template
│   │       ├── VideoLibrary.tsx.template
│   │       └── CommunityForum.tsx.template
│   └── data/online-fitness-coaching.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/ai/payments` | framework & provider choices | prompt.md `## Stack` |
| `#each sections` | drives header/footer nav from blueprint `sections[]` | |
| `#each features` | iterates feature list in blueprint | |

Business content is **never hard-coded** in components: sample workouts, progress data,
nutrition logs, videos, community posts and resource articles flow through
`src/data/online-fitness-coaching.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/online-fitness-coaching.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.