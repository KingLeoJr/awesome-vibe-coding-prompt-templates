# IoT Smart Home Management Platform — Template Set

A parameterized, buildable skeleton for an **Internet of Things (IoT) Smart
Home Management Platform** (`slug: iot-smart-home`, archetype `Dashboard`). The
instance generator reads the blueprint record and resolves every Mustache token
to emit a ready-to-build Next.js project that serves as the web dashboard
reference for the Vue/Nuxt + MQTT ecosystem described in the source prompt.

## Structure

| File | Role |
|---|---|
| `blueprint.json.template` | Metamodel record: tech stack (Nuxt, MQTT, Node-RED, TensorFlow.js…), all 53 features, sections, data models, integrations, NFRs, design tokens, deployment. `extra_config` is the flat token source. |
| `prompt.md.template` | Original builder prompt, parameterized with `{{placeholders}}`. |
| `package.json.template` | Next.js 15 / React 19 / Tailwind dependency manifest. |
| `config/site.config.ts.template` | Site metadata, nav, contact, social, home automation defaults. |
| `styles/globals.css.template` | Tailwind directives + theme tokens from `{{theme.color.*}}`. |
| `src/app/page.tsx.template` | Main entry assembling the smart home dashboard. |
| `src/data/iot-smart-home.json.template` | Sample home data: rooms, devices, energy, scenes, alerts. |
| `src/components/layout/Header.tsx.template` | Sticky, responsive header with theme toggle and mobile menu. |
| `src/components/home/DeviceCard.tsx.template` | Device card with status toggle, sensor reading, and type icon. |
| `src/components/home/RoomSection.tsx.template` | Room grouping with devices and quick scene chips. |
| `src/components/home/EnergyDashboard.tsx.template` | Energy consumption bars, cost, and solar production. |
| `src/components/home/SceneControls.tsx.template` | One-tap scene/automation controls. |
| `src/components/home/AlertPanel.tsx.template` | Device alerts and anomaly notifications. |

## Parameterization points

- `{{app.*}}`, `{{theme.color.*}}` — branding and theme (hex values mirrored by
  `design_tokens[]`; `globals.css` maps them to CSS variables with `_dark` variants under
  `.dark`, consumed via `bg-[var(--color-primary)]` and friends).
- `{{header.*}}`, `{{seo.*}}`, `{{deployment.*}}`, `{{contact.*}}`, `{{social.*}}` — chrome + metadata.
- `{{#each sections}}` — nav + section registry from `sections[]`.
- Home topology (rooms, devices, energy, scenes, alerts) lives in the data file.

## Generating the app

```bash
generator load templates/iot-smart-home/blueprint.json.template
generator generate iot-smart-home --out ./out/iot-smart-home
```

## Shared mixins (materialized by the generator)

```
{{> _shared/ThemeToggle.tsx.template}}   → src/components/shared/ThemeToggle.tsx
{{> _shared/Card.tsx.template}}          → src/components/shared/Card.tsx
{{> _shared/Footer.tsx.template}}        → src/components/layout/Footer.tsx
{{> _shared/utils.ts.template}}          → src/lib/utils.ts
{{> _shared/SEOHead.tsx.template}}       → src/components/SEOHead.tsx
```

Also required in the generated project: `tailwind.config.ts` with
`darkMode: "class"`; `tsconfig.json` with `"@/*": ["src/*"]`.

## Customization

- Re-theme via `design_tokens[]` + `extra_config.theme` (keep light and `_dark` values in sync).
- Add rooms/devices in the data file — `RoomSection` renders them automatically.
- Wire real MQTT data by replacing the data-file fetch with a WebSocket hook at the page boundary.
- The full architecture brief (Nuxt SSR, Vuex-ORM, MQTT, Node-RED, TensorFlow.js,
  Hyperledger Fabric) lives in `blueprint.json.template` and `prompt.md.template`.
