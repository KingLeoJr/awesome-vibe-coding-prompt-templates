# Event Management Platform — Template Set

A parameterized, buildable skeleton for an **Event Management Platform**
(`slug: event-management`, archetype `Dashboard`). The instance generator reads
the blueprint record (`blueprint.json.template`) and resolves every Mustache
token across the `.template` files to emit a ready-to-build Next.js project.

## Structure

| File | Role |
|---|---|
| `blueprint.json.template` | Metamodel record: tech stack, features, sections, components, data models, integrations, NFRs, design tokens, deployment. The `extra_config` block is the flat token source for `{{…}}` substitution. |
| `prompt.md.template` | The original builder prompt, parameterized with `{{placeholders}}` for every tech decision. |
| `package.json.template` | Next.js 15 / React 19 / Tailwind 3.4 dependency manifest. |
| `config/site.config.ts.template` | Site metadata, nav, contact, social, analytics (consumed by components/SEO). |
| `styles/globals.css.template` | Tailwind directives + theme tokens (CSS custom properties from `{{theme.color.*}}`). |
| `src/app/page.tsx.template` | Main entry — assembles all sections from `src/data/event-management.json`. |
| `src/data/event-management.json.template` | Content/config data the page and components consume (events, tiers, analytics, FAQs). |
| `src/components/layout/Header.tsx.template` | Sticky, responsive header with theme toggle and mobile menu. |
| `src/components/events/EventCard.tsx.template` | Event listing card (date, venue, capacity progress, price). |
| `src/components/events/EventCalendar.tsx.template` | Interactive month calendar with category/location filters and day drill-down. |
| `src/components/events/CountdownTimer.tsx.template` | Hero countdown to the next event with a CTA. |
| `src/components/events/TicketTierCard.tsx.template` | Pricing tier card with discount-code support. |
| `src/components/dashboard/AnalyticsOverview.tsx.template` | Organizer dashboard: revenue, tickets, attendance, weekly sales bars. |

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
- Business content (events, tiers, analytics) lives in `src/data/event-management.json.template`,
  so organizers swap sample data without touching components.

## Generating the app

```bash
generator load templates/event-management/blueprint.json.template
generator generate event-management --out ./out/event-management
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
`darkMode: "class"` and `content` globs; `tsconfig.json` with `baseUrl: "."` and
`paths: { "@/*": ["src/*"] }`.

## Customization

- Swap the theme by editing `design_tokens[]` + `extra_config.theme` (keep light and
  `_dark` values in sync).
- Add ticket types by extending `tickets[]` in the data file.
- Reorder sections by editing `sections[]` (order drives nav + page layout).
- Quality bar applied throughout: TypeScript, `dark:` variants, responsive,
  accessible (aria-labels, progressbar roles), Framer Motion, lucide-react.
