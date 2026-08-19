# AI-Powered Personal Finance Management Platform

Parameterized template set for the **`ai-finance-platform`** blueprint
(category: `Platform`, archetype: `AIEnhanced`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A marketing-and-dashboard style finance web app with:

- Hero + sticky **Header** with theme toggle and anchor navigation
- **Analytics dashboard** — SVG income / expense / savings / investment trends
- **Expense tracker** — searchable, filterable transaction log with an inline
  "add transaction" form
- **AI budget insights** — personalized spending recommendations with impact
  and confidence levels
- **Savings goal tracker** — animated progress bars toward financial targets
- **Debt manager** — snowball / avalanche payoff strategies per debt
- **Financial education library** — articles, videos and tools

## Structure

```
templates/ai-finance-platform/
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
│   │   └── finance/
│   │       ├── AnalyticsDashboard.tsx.template
│   │       ├── BudgetInsights.tsx.template
│   │       ├── DebtManager.tsx.template
│   │       ├── EducationLibrary.tsx.template
│   │       ├── ExpenseTracker.tsx.template
│   │       └── SavingsGoals.tsx.template
│   └── data/ai-finance-platform.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/ai/banking/payments` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample transactions,
budgets, goals, debts and learning resources flow through
`src/data/ai-finance-platform.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/ai-finance-platform.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.