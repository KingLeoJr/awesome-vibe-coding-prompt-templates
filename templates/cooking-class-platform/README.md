# Personalized Cooking Class Platform

Parameterized template set for the **`cooking-class-platform`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance
generator reads `blueprint.json.template` and resolves every `{{token}}` in
these files to emit a ready-to-build Next.js 15 + React 19 + TypeScript +
Tailwind project.

## What it generates

A marketing-and-app style cooking platform with:

- Sticky **Header** with theme toggle, anchor navigation, and CTA
- **Live class booking** — filterable grid of live cooking classes with
  availability-aware booking toggles
- **On-demand video library** — tutorials filtered by cuisine, difficulty, and
  dietary focus
- **Recipe database** — cards with ingredients, prep/cook times, nutrition, and
  difficulty badges
- **Weekly meal planner** — day-tabbed weekly menus built from recipes
- **Chef profiles** — vetted chefs offering one-on-one consultations, with
  ratings, specialties, and session pricing
- **Community forum** — posts, categories, replies, and likes
- Footer with social links and contact info

## Structure

```
templates/cooking-class-platform/
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
│   │   └── classes/
│   │       ├── ClassBooking.tsx.template
│   │       ├── VideoLibrary.tsx.template
│   │       ├── RecipeLibrary.tsx.template
│   │       ├── MealPlanner.tsx.template
│   │       ├── ChefProfiles.tsx.template
│   │       └── CommunityForum.tsx.template
│   └── data/cooking-class-platform.json.template  ← realistic sample content
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
| `currency` | `{{currency.symbol}}` | pricing symbol |
| `frontend/backend/ui/state/auth/payments/notifications/video/ai` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: classes, tutorials,
recipes, the weekly meal plan, chef profiles and forum posts all flow through
`src/data/cooking-class-platform.json`, which `src/app/page.tsx` imports and
passes to typed props. Theme colors are consumed via `var(--color-*)` CSS custom
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
   imports and the import path `@/data/cooking-class-platform.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.
- Add classes/recipes in the data file — every grid renders automatically.
- Wire real bookings by swapping the data-file fetch for an API hook at the
  page boundary.
