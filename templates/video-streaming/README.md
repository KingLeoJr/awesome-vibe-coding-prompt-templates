# Video Streaming Platform

Parameterized template set for the **`video-streaming`** blueprint
(category: `Platform`, archetype: `ContentPlatform`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A full-featured video streaming platform with:

- **Hero** section with featured content and trending now
- **VideoCard** components with thumbnail, title, and duration
- **SubscriptionPlan** components showing pricing tiers
- **SearchBar** with autocomplete and genre filters
- **ContinueWatching** row with partially viewed videos
- **WatchLater** collection for saved videos
- **PlaybackControls** with play/pause, seek, and volume
- **ChannelGrid** displaying video listings
- **LiveChat** for live stream interaction
- **Dashboard** with creator analytics and earnings
- **Trending** section with "What's Hot" content
- **Extras** section for behind-the-scenes and bonus content
- **Multi-user Profiles** with individualized recommendations
- **Audio Description** toggle for accessibility
- **Closed Captions** with customization options
- **Virtual Cinema** for premiere events and watch parties

## Structure

```
templates/video-streaming/
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
│   │   └── videos/
│   │       ├── VideoCard.tsx.template
│   │       ├── SubscriptionPlan.tsx.template
│   │       ├── SearchBar.tsx.template
│   │       ├── ContinueWatching.tsx.template
│   │       ├── WatchLater.tsx.template
│   │       ├── PlaybackControls.tsx.template
│   │       ├── ChannelGrid.tsx.template
│   │       └── LiveChat.tsx.template
│   └── data/video-streaming.json.template  ← realistic sample content
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

Business content is **never hard-coded** in components: sample videos, watch
history, subscriptions and chat messages flow through
`src/data/video-streaming.json`, which `src/app/page.tsx` imports and passes
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
   imports and the import path `@/data/video-streaming.json` align by
   construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep
  the shapes in `src/data/*.json` in sync with those interfaces.