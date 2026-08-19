# Personalized Pet Care and Services Platform

Parameterized template set for the **`pet-care-platform`** blueprint
(category: `Vertical`, archetype: `PersonalizedVertical`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A personalized pet care web platform with:

- **Header** — navigation with brand, anchor navigation, CTA, theme toggle
- **Overview section** — pet stats, community highlights
- **Pet Card** — display pet information (type, breed, age, medical history)
- **Service Provider** — listing of vetted pet service providers (groomers, trainers, vets)
- **Booking System** — appointment scheduling with service providers
- **Vaccination Tracker** — track vaccination records and health metrics
- **Resource Library** — articles, videos, and guides on pet care topics
- **Cost Estimator** — estimate costs for various pet services
- **Community Forum** — user discussions, pet care questions, and tips
- **FAQ Section** — common questions about pet care services and scheduling
- **Blog Section** — expert advice on pet care trends and training tips
- **Referral Program** — rewards for inviting friends to join
- **Event Calendar** — local pet-related events (adoption days, training classes)
- **Gamification** — badges for completing pet care challenges
- **Notifications** — reminders for vet visits, grooming appointments, care tasks
- **Responsive Design** — from 320px mobile to 4K desktop

## Structure

```
templates/pet-care-platform/
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
│   │   └── pet/
│   │       ├── PetCard.tsx.template
│   │       ├── ServiceProvider.tsx.template
│   │       ├── VaccinationTracker.tsx.template
│   │       ├── BookingSystem.tsx.template
│   │       ├── ResourceLibrary.tsx.template
│   │       └── CostEstimator.tsx.template
│   └── data/pet-care-platform.json.template  ← realistic sample content
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
| `#each sections` | drives header/footer nav from blueprint `sections[]` | |
| `#each features` | iterates feature list in blueprint | |

Business content is **never hard-coded** in components: pet profiles, service provider
data, vaccination records, cost estimates, blog posts, and community posts flow through
`src/data/pet-care-platform.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/pet-care-platform.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.