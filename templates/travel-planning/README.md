# Travel Planning and Booking Platform

Parameterized template set for the **`travel-planning`** blueprint
(category: `Platform`, archetype: `Marketplace`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A full-featured travel web platform with:

- **Sticky Header** with theme toggle, brand name, and anchor navigation
- **DestinationCard** components with image, price, and rating
- **FlightSearch** with origin/destination inputs and date picker
- **HotelBooking** module with filters and date selection
- **CarRental** feature with pick-up/return location and date
- **ItineraryPlanner** day-by-day trip organizer
- **MapView** integrating Google Maps API for routes and points of interest
- **CommunityForum** for traveler experience sharing
- **ReviewCard** for flights, hotels, and attractions
- **BudgetPlanner** cost estimator for flights, hotels, meals, activities
- **VirtualAssistant** AI chatbot for user queries
- **Dashboard** tracking travel history and future bookings
- **Blog** with travel guides, tips, and personal stories
- **Push notifications** for special deals and last-minute offers

## Structure

```
templates/travel-planning/
├── README.md
├── blueprint.json.template          ← metamodel record (schema + documentation)
├── prompt.md.template               ← parameterized builder prompt
├── config/site.config.ts.template   ← site config (tokens + domain constants)
├── package.json.template            ← dependency manifest (mirrors _shared)
├── styles/globals.css.template      ← Tailwind directives + theme CSS variables
├── src/
│   ├── app/page.tsx.template        ← main entry, assembles all sections
│   ├── components/
│   │   ├── layout/
│   │   │   └── Header.tsx.template
│   │   └── travel/
│   │       ├── DestinationCard.tsx.template
│   │       ├── FlightSearch.tsx.template
│   │       ├── HotelBooking.tsx.template
│   │       ├── CarRental.tsx.template
│   │       ├── ItineraryPlanner.tsx.template
│   │       ├── MapView.tsx.template
│   │       ├── CommunityForum.tsx.template
│   │       ├── ReviewCard.tsx.template
│   │       ├── BudgetPlanner.tsx.template
│   │       └── VirtualAssistant.tsx.template
│   └── data/travel-planning.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample destinations,
itineraries, reviews, budgets and chat logs flow through
`src/data/travel-planning.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/travel-planning.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.