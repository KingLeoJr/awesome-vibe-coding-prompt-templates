# Recipe Sharing and Meal Planning Template Set

Parameterized template set for the **`recipe-meal-planning`** blueprint
(category: `Content`, archetype: `ContentPlatform`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A recipe sharing and meal planning web app with:

- **Sticky Header** with theme toggle, brand name, anchor navigation, and CTA
- **Browse** — searchable recipe grid with cuisine, dietary, and ingredient filters
- **Meal Planner** — weekly meal plan builder from selected recipes
- **Grocery List** — auto-compiled ingredient list from meal plan
- **Recipe Submission** — form with photos, ingredients, and cooking instructions
- **Review & Rating** system for recipes
- **Community Forum** — cooking tips, questions, and culinary discussion
- **Social Media Integration** — share favorites and follow other cooks
- **FAQ Section** — common questions about submitting, planning, and using the platform
- **Blog Section** — cooking tips, seasonal recipes, and nutrition articles
- **Referral Program** — rewards for inviting friends
- **Interactive Cooking Challenge** — themed contests and showcases
- **Impact Reporting** — highlights how the platform promotes healthy eating

## Structure

| File | Role |
|---|---|
| `blueprint.json.template` | Metamodel record: tech stack, all 26 features, sections, components, data models, integrations, NFRs, design tokens, deployment. `extra_config` is the flat token source. |
| `prompt.md.template` | The original builder prompt, parameterized with `{{placeholders}}`. |
| `package.json.template` | Next.js 15 / React 19 / Tailwind dependency manifest. |
| `config/site.config.ts.template` | Site metadata, nav, contact, social, cooking defaults. |
| `styles/globals.css.template` | Tailwind directives + theme tokens from `{{theme.color.*}}`. |
| `src/app/page.tsx.template` | Main entry assembling all sections. |
| `src/data/recipe-meal-planning.json.template` | Sample recipes, meal plans, grocery lists. |
| `src/components/layout/Header.tsx.template` | Sticky, responsive header with theme toggle and mobile menu. |
| `src/components/recipes/RecipeCard.tsx.template` | Recipe card with image, title, rating, and action buttons. |
| `src/components/recipes/MealPlanner.tsx.template` | Weekly meal plan builder UI. |
| `src/components/recipes/GroceryList.tsx.template` | Auto-compiled ingredient list. |
| `src/components/recipes/RecipeSubmission.tsx.template` | Form to upload new recipes. |
| `src/components/recipes/ReviewCard.tsx.template` | Review/rating card for user feedback. |
| `src/components/recipes/ForumThread.tsx.template` | Community discussion thread. |

## Parameterization points

All configurable values flow through tokens — nothing is hard-coded.

- `{{app.name}}`, `{{app.slug}}`, `{{app.tagline}}` — branding (from blueprint `name`/`tagline`).
- `{{theme.color.*}}` — theme hex values mirrored by `design_tokens[]`. `globals.css` maps them
  to CSS custom properties (`--color-primary`, `--color-secondary`, `--color-accent` with `_dark`
  variants under `.dark`); components consume them via Tailwind arbitrary values such as
  `bg-[var(--color-primary)]`, `text-[var(--color-primary)]`, `ring-[color:var(--color-primary)]`.
- `{{header.cta_text}}`, `{{header.cta_href}}`, `{{header.is_fixed}}` — header behaviour.
- `{{seo.*}}`, `{{deployment.url}}`, `{{deployment.analytics_id}}`, `{{contact.*}}`, `{{social.*}}` — metadata.
- `{{#each sections}}` — drives header/footer nav from the blueprint `sections[]` rows.
- Business content (recipes, meal plans, grocery lists) lives in `src/data/recipe-meal-planning.json.template`,
  so organizers swap sample data without touching components.

## Generating the app

```bash
generator load templates/recipe-meal-planning/blueprint.json.template
generator generate recipe-meal-planning --out ./out/recipe-meal-planning
```

## Shared mixins (materialized by the generator)

The following `_shared/` files are referenced by imports and must be copied into
the output tree at the mapped paths:

```
{{> _shared/ThemeToggle.tsx.template}}   → src/components/shared/ThemeToggle.tsx
{{> _shared/Card.tsx.template}}          → src/components/shared/Card.tsx
{{> _shared/Footer.tsx.template}}        → src/components/layout/Footer.tsx
{{> _shared/utils.ts.template}}          → src/lib/utils.ts
{{> _shared/SEOHead.tsx.template}}       → src/components/SEOHead.tsx
```

Also required in the generated project: `tailwind.config.ts` with
`darkMode: "class"`; `tsconfig.json` with `"@/*": ["src/*"]` and
paths: `{ "@/*": ["src/*"] }`.

## Customization

- Re-theme via `design_tokens[]` + `extra_config.theme` (keep light and
  `_dark` values in sync).
- Add recipes by extending the data file — all components consume it automatically.
- Reorder sections by editing `sections[]` (order drives nav + page layout).
- Quality bar applied throughout: TypeScript, `dark:` variants, responsive,
  accessible (aria-labels, progressbar roles), Framer Motion, lucide-react.