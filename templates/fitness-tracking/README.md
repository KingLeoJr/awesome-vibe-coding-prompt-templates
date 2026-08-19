# Fitness and Wellness Tracking Platform — Template Set

A parameterized, buildable skeleton for a **Fitness and Wellness Tracking
Platform** (`slug: fitness-tracking`, archetype `Dashboard`). The instance
generator reads the blueprint record and resolves every Mustache token to emit a
ready-to-build Next.js project that doubles as the web dashboard for the
Flutter/Firebase mobile app described in the source prompt.

## Structure

| File | Role |
|---|---|
| `blueprint.json.template` | Metamodel record: tech stack (Flutter, Firebase, Provider…), features, sections, data models, integrations, NFRs, design tokens, deployment. `extra_config` is the flat token source. |
| `prompt.md.template` | Original builder prompt, parameterized with `{{placeholders}}`. |
| `package.json.template` | Next.js 15 / React 19 / Tailwind dependency manifest. |
| `config/site.config.ts.template` | Site metadata, nav, contact, social, wellness defaults. |
| `styles/globals.css.template` | Tailwind directives + theme tokens from `{{theme.color.*}}`. |
| `src/app/page.tsx.template` | Main entry assembling the wellness dashboard. |
| `src/data/fitness-tracking.json.template` | Sample user data: activity rings, workouts, nutrition, weekly stats, badges. |
| `src/components/layout/Header.tsx.template` | Sticky, responsive header with theme toggle and mobile menu. |
| `src/components/fitness/ActivityRing.tsx.template` | SVG circular progress rings for steps, active minutes, hydration. |
| `src/components/fitness/WorkoutLog.tsx.template` | Workout logging with exercises (sets/reps/weight) and an inline add form. |
| `src/components/fitness/NutritionTracker.tsx.template` | Calorie + macro progress bars with a daily meal list. |
| `src/components/fitness/WeeklyOverview.tsx.template` | Weekly activity bar chart and goal summary. |
| `src/components/fitness/AchievementBadge.tsx.template` | Gamified badges and challenges grid. |

## Parameterization points

- `{{app.*}}`, `{{theme.color.*}}` — branding and theme (hex values mirrored by
  `design_tokens[]`; `globals.css` maps them to CSS variables with `_dark` variants under
  `.dark`, consumed via `bg-[var(--color-primary)]` and friends).
- `{{header.*}}`, `{{seo.*}}`, `{{deployment.*}}`, `{{contact.*}}`, `{{social.*}}` — chrome + metadata.
- `{{#each sections}}` — nav + section registry from `sections[]`.
- User data (activity, workouts, meals, badges) lives in the data file so
  dashboards can be re-skinned without touching components.

## Generating the app

```bash
generator load templates/fitness-tracking/blueprint.json.template
generator generate fitness-tracking --out ./out/fitness-tracking
```

## Shared mixins (materialized by the generator)

```
{{> _shared/ThemeToggle.tsx.template}}   → src/components/shared/ThemeToggle.tsx
{{> _shared/Card.tsx.template}}          → src/components/shared/Card.tsx
{{> _shared/Footer.tsx.template}}        → src/components/layout/Footer.tsx
{{> _shared/utils.ts.template}}          → src/lib/utils.ts
{{> _shared/SEOHead.tsx.template}}       → src/components/SEOHead.tsx
```

Also required in the generated project: `tailwind.config.ts` with
`darkMode: "class"`; `tsconfig.json` with `"@/*": ["src/*"]`.

## Customization

- Edit `design_tokens[]` + `extra_config.theme` to re-theme (keep light and `_dark` values in sync).
- Adjust daily goals (steps, calories, hydration) in the data file.
- The full product brief (Flutter, Firebase Auth, Provider, wearables) lives in
  `blueprint.json.template` and `prompt.md.template`; the React skeleton here is
  the reference dashboard UI for that mobile ecosystem.
