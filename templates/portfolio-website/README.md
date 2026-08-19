# Portfolio Website

Parameterized template set for the **`portfolio-website`** blueprint
(category: `Template`, archetype: `ResponsiveWebApp`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` in these files to emit
a ready-to-build Next.js 14 + React 18 + TypeScript + Tailwind project.

## What it generates

A marketing-and-portfolio style personal website with:

- **Fixed Header** — logo, navigation menu, contact button, mobile menu with animated hamburger, theme toggle, smooth scrolling via react-scroll
- **Hero Section** — animated text reveal with Framer Motion, optional particles.js background, custom cursor with react-custom-cursor, mouse trailer with react-mouse-particles
- **About Section** — professional headshot and bio, scroll-based storytelling with AOS library
- **Skills Section** — animated progress bars with Chart.js, interactive word cloud with react-wordcloud, tooltips with react-tooltip
- **Projects Showcase** — responsive masonry grid with react-masonry-css, filterable categories, modal popups for project details, lazy loading with next/image, WebGL transitions with Three.js
- **Testimonials** — carousel with Swiper.js (autoplay, pause on hover)
- **Experience Timeline** — vertical scroll animations with AOS library
- **Blog Section** — file-based routing with Astro's src/pages structure, markdown content with frontmatter, content collections, dynamic [slug].astro pages, SSG for optimal performance and SEO, Astro.glob() for blog post lists, syntax highlighting, custom 404 page, RSS feed, tagging system, reading time estimation, table of contents, related posts, social sharing buttons, newsletter subscription, comments with Disqus/Utterances, featured posts carousel, custom video player, "time to read" progress bar, "copy code" button, custom blockquote, "last updated" indicator
- **Contact Section** — contact form with client-side validation and reCAPTCHA integration, drag-and-drop file upload with react-dropzone, floating action button for social media, SVG animations with GSAP, dark mode toggle with localStorage, custom audio player, scroll-to-top button

## Structure

```
templates/portfolio-website/
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
│   │   │   ├── Header.tsx.template
│   │   │   └── Contact.tsx.template
│   │   ├── portfolio/
│   │   │   ├── Hero.tsx.template
│   │   │   ├── About.tsx.template
│   │   │   ├── Skills.tsx.template
│   │   │   ├── Projects.tsx.template
│   │   │   ├── Testimonials.tsx.template
│   │   │   └── Experience.tsx.template
│   │   └── shared/
│   │       └── Footer.tsx.template
│   └── data/portfolio-website.json.template  ← realistic sample content
```

## Parameterization points

| Namespace | Token | Meaning |
|---|---|---|
| `app` | `{{app.name}}` `{{app.slug}}` `{{app.tagline}}` | product identity |
| `theme` | `{{theme.color.primary}}` `{{theme.color.secondary}}` `{{theme.color.accent}}` `{{theme.font.sans}}` `{{theme.radius.card}}` | design system → CSS vars in `globals.css` |
| `header` | `{{header.cta_text}}` `{{header.cta_href}}` | call-to-action |
| `seo` | `{{seo.og_image}}` `{{seo.schema_type}}` | Open Graph / schema |
| `deployment` | `{{deployment.url}}` `{{deployment.analytics_id}}` | deploy target |
| `contact` | `{{contact.email}}` `{{contact.phone}}` `{{contact.recaptchaSiteKey}}` | footer contact |
| `social` | `{{social.github}}` `{{social.linkedin}}` `{{social.twitter}}` | footer socials |
| `frontend/backend/ui/state/animation/visuals/currency` | framework & provider choices | prompt.md `## Stack` |

Business content is **never hard-coded** in components: sample projects, testimonials, goals, blog posts and contact data flow through `src/data/portfolio-website.json`, which `src/app/page.tsx` imports and passes to typed props. Theme colors are consumed via `var(--color-*)` CSS custom properties generated from `{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk the `src/`, `config/`, `styles/` and root files; materialize shared mixins from `templates/_shared/` into `src/components/shared/` (`ThemeToggle`, `Button`, `Card`, `Footer`, `SEOHead`, `site.config`, `package.json`, `utils`).
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` in the page/data/package.json imports and the import path `@/data/portfolio-website.json` align by construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts the JSON data to exported component prop types; keep the shapes in `src/data/*.json` in sync with those interfaces.