# Template Sets

Every prompt entry in this repository has a matching template set under its own
folder here.

## Conventions

- **Syntax** — Mustache-style tokens: `{{value}}`, `{{#each …}}`, `{{#if …}}`,
  `{{> mixin_name}}`. See `metamodel/README.md` for the full token reference.
- **Shared mixins** — `_shared/` contains cross-cutting files (Header, Footer,
  ThemeToggle, SEOHead, Button, Card, site.config, package.json). Every app set
  can reference them with `{{> _shared/… }}` or copy them locally and
  parameterize further.
- **Blueprint** — each app folder ships a `blueprint.json.template` recording
  the metamodel rows (tech stack, features, sections, data models,
  integrations, NFRs, design tokens, deployment). It is both the schema for the
  generator and a documentation artifact.
- **README** — each app folder includes a short README describing the structure,
  the parameterization points, and how to generate the final project.
- **Quality bar** — TypeScript, dark mode, responsive, accessible, animated
  (Framer Motion), modern Tailwind design, `"use client"` where interactivity
  is needed, typed props with sensible defaults, and zero hard-coded business
  values (everything flows through `{{tokens}}` or the blueprint data file).

## Generating an app (example)

```bash
# 1. The generator reads the blueprint for an app
generator load metamodel/blueprints/saas-dashboard.json

# 2. It walks the template set
templates/saas-dashboard/

# 3. It resolves every {{token}} and writes the output project
generator generate saas-dashboard --out ./out/my-dashboard
```

The output is a complete, ready-to-run project that can be dropped into
Cursor / Bolt / Lovable / Windsurf / Cline / Aider and built immediately.
