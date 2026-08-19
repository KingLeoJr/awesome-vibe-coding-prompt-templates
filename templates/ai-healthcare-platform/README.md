# Artificial Intelligence-Powered Healthcare Platform

Parameterized template set for the **`ai-healthcare-platform`** blueprint
(category: `Platform`, archetype: `AIEnhanced`). The instance generator reads
`blueprint.json.template` and resolves every `{{token}}` to emit a ready-to-build
Next.js 15 + React 19 + TypeScript + Tailwind project.

## What it generates

A patient-and-care-team web app covering the highest-value features of a modern
digital health platform:

- Sticky **Header** with theme toggle and section navigation
- **AI symptom checker** — pick symptoms, get ranked condition matches with
  severity triage and next-step advice
- **Telemedicine scheduler** — select a provider, pick an available slot, confirm
  a secure video consult
- **Medication manager** — doses, schedules, refill alerts and drug-interaction
  warnings
- **Health metrics dashboard** — wearable / remote-monitoring vitals with trend
  charts
- **Secure message center** — patient ⇄ provider threads with encryption
  framing

## Structure

```
templates/ai-healthcare-platform/
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
│   │   └── healthcare/
│   │       ├── CareMessageCenter.tsx.template
│   │       ├── HealthMetricsDashboard.tsx.template
│   │       ├── MedicationManager.tsx.template
│   │       ├── SymptomChecker.tsx.template
│   │       └── TelemedicineScheduler.tsx.template
│   └── data/ai-healthcare-platform.json.template  ← realistic sample content
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
| `frontend/backend/ui/state/auth/ai/interop/comms` | framework & provider choices | prompt.md `## Stack` |

Clinical content flows through `src/data/ai-healthcare-platform.json` (symptom
library, conditions, providers, slots, medications, vitals, threads) — never
hard-coded in components. Theme colors are consumed via `var(--color-*)` CSS
custom properties generated from `{{theme.color.*}}`.

## How the generator uses it

1. Load `blueprint.json.template` as the metamodel record for this app.
2. Walk `src/`, `config/`, `styles/` and root files; materialize shared mixins
   from `templates/_shared/` into `src/components/shared/`.
3. Resolve `{{tokens}}`, `{{#each ...}}` and `{{> _shared/... }}` per
   `metamodel/README.md`.
4. Emit a runnable project; `{{app.slug}}` keeps imports and the data file
   aligned by construction.

## Build notes

- `npm install && npm run dev` starts Next.js.
- `npm run lint` / `npm run typecheck` verify the emitted project.
- `src/app/page.tsx` casts JSON data to exported component prop types; keep the
  shapes in `src/data/*.json` in sync with those interfaces.
- The template simulates HIPAA-grade behaviors (encryption framing, MFA
  messaging) — wire real AES-256/TLS, FHIR and WebRTC via the backend contract
  in `prompt.md.template`.