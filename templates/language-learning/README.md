# Language Learning Platform — Template Set

A parameterized, buildable skeleton for a **Personalized Language Learning Platform** (`slug: language-learning`, archetype `PersonalizedVertical`). The instance generator reads the blueprint record (`blueprint.json.template`) and resolves every Mustache token across the `.template` files to emit a ready-to-build Next.js project.

## Structure

| File | Role |
|---|---|
| `blueprint.json.template` | Metamodel record: tech stack, features, sections, components, data models, integrations, NFRs, design tokens, deployment. The `extra_config` block is the flat token source for `{{…}}` substitution. |
| `prompt.md.template` | The original builder prompt, parameterized with `{{placeholders}}` for every tech decision. |
| `package.json.template` | Next.js 15 / React 19 / Tailwind 3.4 dependency manifest. |
| `config/site.config.ts.template` | Site metadata, nav, contact, social, analytics (consumed by components/SEO). |
| `styles/globals.css.template` | Tailwind directives + theme tokens (CSS custom properties from `{{theme.color.*}}`). |
| `src/app/page.tsx.template` | Main entry — assembles all sections from `src/data/language-learning.json`. |
| `src/data/language-learning.json.template` | Content/config data the page and components consume (lessons, vocabulary, progress, community). |
| `src/components/layout/Header.tsx.template` | Sticky, responsive header with theme toggle and mobile menu. |
| `src/components/language/LessonCard.tsx.template` | Lesson card with multimedia resources and progress tracking. |
| `src/components/language/VocabularyDeck.tsx.template` | Vocabulary builder with flashcards and spaced repetition. |
| `src/components/language/SpeakingPractice.tsx.template` | Speaking practice with audio playback and recording. |
| `src/components/language/ProgressDashboard.tsx.template` | Progress tracking dashboard with visual graphs and statistics. |
| `src/components/shared/ThemeToggle.tsx.template` | Theme toggle button (shared mixin). |
| `src/components/shared/Footer.tsx.template` | Footer with nav, description, and social links (shared mixin). |
| `src/components/shared/Header.tsx.template` | Header with brand, nav, CTA, theme toggle (shared mixin). |
| `src/components/shared/SEOHead.tsx.template` | SEO head with Open Graph and JSON-LD (shared mixin). |
| `src/lib/utils.ts.template` | Utility functions (shared mixin). |

## Parameterization points

All configurable values flow through tokens — nothing is hard-coded.

- `{{app.name}}`, `{{app.slug}}`, `{{app.tagline}}` — branding (from blueprint `name`/`tagline`).
- `{{theme.color.*}}` — theme hex values mirrored by `design_tokens[]`. `globals.css` maps them to CSS custom properties (`--color-primary`, `--color-secondary`, `--color-accent` with `_dark` variants under `.dark`); components consume them via Tailwind arbitrary values such as `bg-[var(--color-primary)]`, `text-[var(--color-primary)]`, `ring-[color:var(--color-primary)]`.
- `{{header.cta_text}}`, `{{header.cta_href}}`, `{{header.is_fixed}}` — header behaviour.
- `{{seo.*}}`, `{{deployment.url}}`, `{{deployment.analytics_id}}`, `{{contact.*}}`, `{{social.*}}` — metadata.
- `{{#each sections}}` — drives header/footer nav from the blueprint `sections[]` rows.
- Business content (lessons, vocabulary, progress) lives in `src/data/language-learning.json.template`, so learners swap sample data without touching components.

## Generating the app

```bash
generator load templates/language-learning/blueprint.json.template
generator generate language-learning --out ./out/language-learning
```

## Shared mixins (materialized by the generator)

The following `_shared/` files are referenced by imports and must be copied into the output tree at the mapped paths:

```
{{> _shared/ThemeToggle.tsx.template}}   → src/components/shared/ThemeToggle.tsx
{{> _shared/Footer.tsx.template}}        → src/components/layout/Footer.tsx
{{> _shared/Header.tsx.template}}        → src/components/layout/Header.tsx
{{> _shared/SEOHead.tsx.template}}       → src/components/SEOHead.tsx
{{> _shared/utils.ts.template}}          → src/lib/utils.ts
```

Also required in the generated project: `tailwind.config.ts` with `darkMode: "class"` and `content` globs; `tsconfig.json` with `baseUrl: "."` and `paths: { "@/*": ["src/*"] }`.

## Customization

- Swap the theme by editing `design_tokens[]` + `extra_config.theme` (keep light and `_dark` values in sync).
- Add lessons or vocabulary sets by extending the data file — `LessonCard` and `VocabularyDeck` render them automatically.
- Reorder sections by editing `sections[]` (order drives nav + page layout).
- Quality bar applied throughout: TypeScript, `dark:` variants, responsive, accessible (aria-labels, progressbar roles), Framer Motion, lucide-react.