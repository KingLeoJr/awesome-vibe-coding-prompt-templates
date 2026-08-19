# Personalized Ethical Consumerism Resource Platform

Parameterized template set for the **`ethical-consumerism`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance generator
reads `blueprint.json.template` and resolves every `{{token}}` in these files to
emit a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A personalized, community-driven ethical consumerism web app with:

- Hero + sticky **Header** with theme toggle and anchor navigation
- **Brand scorecard** — ethical brand directory with certifications and ethics scores
- **Product explorer** — find ethical alternatives to everyday purchases with impact deltas
- **Impact tracker** — purchase history, sustainability metrics, and gamified badges
- **Values quiz** — interactive quiz that maps shopping preferences to aligned brands
- **Local ethics finder** — nearby businesses committed to ethical practices
- **Resource library** — articles, videos, and guides on conscious consumption

## Structure

```
templates/ethical-consumerism/
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
│   │   └── consumerism/
│   │       ├── AlternativeFinder.tsx.template
│   │       ├── BrandScorecard.tsx.template
│   │       ├── EthicalQuiz.tsx.template
│   │       ├── ImpactBadge.tsx.template
│   │       ├── LocalEthicsFinder.tsx.template
│   │       └── ResourceLibrary.tsx.template
│   └── data/ethical-consumerism.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/ai/email/maps/community/gamification` | framework & provider choices | prompt.md feature lines |

Business content is **never hard-coded** in components: brands, alternatives,
impact records, quiz questions, local businesses and learning resources flow
through `src/data/ethical-consumerism.json`, which `src/app/page.tsx` imports
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
   imports and the import path `@/data/ethical-consumerism.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.