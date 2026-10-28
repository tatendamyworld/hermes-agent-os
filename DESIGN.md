# Hermes Agent OS — Design Token Specification

> A design system for building user interfaces for Hermes agents.
> Terminal-native, dark-first, single-accent. Grounded in the anti-slop
> rules of `design-taste-frontend` and the dev-tool vocabularies of
> `popular-web-designs` (VoltAgent, Linear, OpenCode, Cursor).

## 1. Purpose & scope

This spec answers two questions:

1. **What** does a Hermes agent UI look like? — colour, typography, spacing,
   motion, and the shapes of its components, as machine-readable CSS custom
   properties.
2. **Where** does each rule come from? — every token cites the bridge reference
   that justifies it (`references/agent-skills-patterns.md` for the anti-slop
   rules, `references/popular-web-designs-bridge.md` for the dev-tool aesthetic).

It is imported by the `hermes-agent-os` agent skill (`SKILL.md`) and consumed
verbatim by the demo component lab (`demo/index.html`).

### The seven surfaces (surface-first)

Before touching tokens, name the surface. An agent UI spans several; pick the
**primary** and treat the rest as secondary, never average them.

| Surface | What it is | Agent UI example |
|---|---|---|
| **Monitor** | Watching state change | Agent activity stream, run log |
| **Operate** | Taking action on things | Command palette, file builder, tool panel |
| **Compare** | Weighing options | Run history, model selector, plan comparison |
| **Configure** | Setting things up | Settings, env vars, skill loader |
| **Decide / Learn** | Being convinced or taught | Landing, docs, onboarding |
| **Explore** | Browsing an open space | File explorer, knowledge base search |
| **Command / Inspect** | Keyboard-driven drill-in | `/inspect`, detail pane, property editor |

This spec primarily serves **Operate** and **Monitor** (the working surfaces an
agent hand to a human), with tokens that scale down to **Configure** and up to
**Command/Inspect**.

## 2. Colour

### 2.1 Ground model

One ground, one ink, one accent. Colour appears **on data and on the single
primary action**; it never colours chrome (sidebars, nav rows, cards).

```
--os-canvas    #09090d   /* deep terminal canvas — the ground */
--os-surface   #0f0f16   /* panels, inputs, cards — one step up */
--os-raised    #171721   /* floating menus, modal, detail pane */
--os-ink       #e4e6ec   /* primary text — soft off-white, never pure #fff */
--os-muted     #9aa0b8   /* secondary text, labels */
--os-faint     #5a5f73   /* placeholders, disabled, metadata */
--os-border    #22232a   /* solid border: 1.2–1.4:1 against canvas */
--os-rule      #1a1a20   /* faint divider: structural only */
--os-accent    #00d1b2   /* computing cyan — the one accent */
--os-accent-ink #001f23  /* text on accent surfaces */
--os-positive  #26e391   /* active / completed / connected */
--os-negative  #ff6b82   /* error / failed / disconnected */
--os-warning   #ffb84d   /* warning / attention */
```

> **Why cyan?** The `design-taste-frontend` house style bans violet/indigo and
> cream+terracotta (the “AI-assistant palette”, §9 `tells.md`). VoltAgent and
> OpenCode establish dark-terminal + green/emerald as the dev-tool convention.
> Cyan reads as *computing* and is distinct from green (reserved for
> positive/active status), so the accent stays unambiguous on data.

### 2.2 Application

```css
:root {
  /* ground model — flat, no shadows at rest (taste rule §2.4) */
  background: var(--os-canvas);
  color: var(--os-ink);
}
.surface { background: var(--os-surface); border: 1px solid var(--os-border); }
.raised  { background: var(--os-raised); }
/* accent — on the action and at most one recurring data element (colour.md §3) */
a { color: var(--os-accent); }
/* status hues — on data only, never on chrome (app-screens.md §5) */
.status-done   { color: var(--os-positive); }
.status-error  { color: var(--os-negative); }
```

- Surfaces are told apart by **a single tone step**, never by a shadow at rest.
- Borders are `1px solid var(--os-border)`, one tone step; no hairlines.
- `--os-accent` must occupy **under 5%** of any screen.

### 2.3 Light mode

A light ground is offered as a **preference only**, never the default for an
agent UI. Tokens flip the step direction, not the palette:

```css
@media (prefers-color-scheme: light) {
  :root {
    --os-canvas:   #f7f8fc;
    --os-surface:  #ffffff;
    --os-raised:   #f1f1fa;
    --os-ink:      #14141a;
    --os-muted:    #5a5f73;
    --os-faint:    #9aa0b8;
    --os-border:   #d9dae2;
    --os-rule:     #e8e9f0;
    --os-positive: #1daa62;
    --os-negative: #d9425c;
  }
}
```

## 3. Typography

```
font-ui:   "Geist", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
font-mono: "Geist Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
```

Geist ships via Google Fonts (CDN substitute for Vercel/Figmas Geist; see
`references/popular-web-designs-bridge.md` §2.3). Request only weights used:
400, 500, 600, 700.

### 3.1 Scale (few sizes, far apart)

```css
:root {
  --step-micro:   clamp(0.70rem, 0.60rem + 0.3vw, 0.80rem);  /* 11–13px  captions */
  --step-small:   clamp(0.85rem, 0.78rem + 0.3vw, 0.95rem);  /* 14–15px  labels, meta */
  --step-body:    clamp(1.00rem, 0.92rem + 0.4vw, 1.12rem);  /* 16–18px  paragraphs */
  --step-large:   clamp(1.25rem, 1.10rem + 0.6vw, 1.50rem);  /* 20–24px  subheadings */
  --step-heading: clamp(1.75rem, 1.40rem + 1.2vw, 2.50rem);  /* 28–40px  section titles */
  --step-display: clamp(2.50rem, 1.80rem + 2.4vw, 4.00rem);  /* 40–64px  page titles */
}
```

### 3.2 Rules

- **Two faces, two jobs.** Geist for all UI and readable text; Geist Mono for
  commands, code, IDs, and numeric values (`font-variant-numeric: tabular-nums`).
  Never three.
- **Display type:** tracking `-0.02em`, leading `1.0–1.05`, `text-wrap: balance`,
  optical left-align (`margin-left: -0.05em`) on big round letters.
- **Body:** `16–18px`, leading `1.5–1.6`, measure `45–75ch`, `text-wrap: pretty`.
- **App text ≤ 32px.** In an agent UI no figure reaches `48px` (app-screens.md §9.1).

## 4. Space, radius, elevation

```css
:root {
  /* 4px base, 8px rhythm — a single grid everywhere (layout.md §2) */
  --space-3xs:  2px;
  --space-2xs:  4px;
  --space-xs:   8px;
  --space-sm:   12px;
  --space-md:   16px;
  --space-lg:   24px;
  --space-xl:   32px;
  --space-2xl:  48px;
  --space-3xl:  64px;

  /* radius: bimodal — tight or soft, chosen once (app-screens.md §9.4) */
  --radius-tight:   4px;   /* code, chips, inline */
  --radius-control: 6px;   /* buttons, inputs */
  --radius-panel:   8px;   /* cards, popovers */
  --radius-soft:    12px;  /* modals, feature panels */
  --radius-pill:    9999px;

  /* elevation — tone step only; shadow on floats (colour.md §6) */
  --shadow-float:  0 4px 12px rgba(0,0,0,0.25);
}
```

- One gutter horizontally and vertically.
- Equal outer margins at every width; content never huddled on one side.
- No shadows on resting cards; only on menus, popovers, tooltips, dialogs.

## 5. Motion

Motion is **discipline, not theatre**. Four parts, one signature move at most.

```css
:root {
  --ease-out:     cubic-bezier(0.22, 1, 0.36, 1);   /* <600ms UI */
  --ease-expo:    cubic-bezier(0.16, 1, 0.3, 1);     /* reveals ≥800ms */
  --ease-in-out:  cubic-bezier(0.76, 0, 0.24, 1);    /* wipes, travel */
  --ease-exit:    cubic-bezier(0.55, 0.055, 0.675, 0.19); /* leaving for good */

  --t-press:    140ms;   /* button press */
  --t-hover:    180ms;   /* colour hover */
  --t-ui:       240ms;   /* menus, popovers */
  --t-reveal:   700ms;   /* section entrances, once */
  --t-open:    1000ms;   /* image/panel openings */
  --stagger:    70ms;    /* list items */
}
```

- Press feedback on every control; hover answers the pointer.
- Entrances fire once, below the fold, 85% down the viewport.
- Reduced motion keeps the finished state; colour/opacity only.
- No preloader, no counter, no custom cursor, no wheel capture.

## 6. Components

All components are built from the tokens above. No raw hex in a component.

### 6.1 Command palette (the agent’s `/`)

- **Surface:** Command/Inspect.
- Full-screen drawer on desktop (720px wide), full-height on phone.
- Input in `--os-surface`, `--os-ink`, `Geist Mono`, `tabular-nums`.
- Results list: one accent dot (status) + monospace command on the left, short
  grey hint on the right. No icons in tinted squares.
- `Enter` runs; `Esc` closes. Focus trap. `aria-live` on the prompt line.

### 6.2 Chat / conversation view

- **Surface:** Monitor.
- Message stack: assistant, user, tool each a flat panel (`--os-raised`) with a
  single `1px` border, `--radius-panel`, no shadow.
- Tool calls appear inline as **tool cards** (§6.3), not as grey bars.
- Status line under the composer: `Ready` (positive), `Thinking…` (accent pulse,
  2s cycle), `Error` (negative). Never a decorative status dot.

### 6.3 Tool card

- **Surface:** Monitor (inside a message).
- Header: tool name in `--os-accent`, `Geist 14px/600`, left; icon-only action
  (`--os-positive`) to expand, right.
- Body: `Geist Mono 13px`, `font-variant-numeric: tabular-nums`, one tone step
  background, collapsible.
- Accent appears **only on the tool name** — data, not chrome (app-screens §5).

### 6.4 Status badge

- Pill, `9999px` radius, `--os-positive`/`--os-negative`/`--os-warning` fill with
  inverted ink (`--os-accent-ink` for positive). No tinted circles on KPIs.

### 6.5 Data table (agent outputs)

- 12px `sentence-case` grey header, no fill band, no zebra.
- Numbers right-aligned, tabular figures, fixed decimals, sign on every change.
- Row rules `1px` one step darker than surface. Status as hue on the row's mark,
  not a coloured chip on every row.

### 6.6 Form controls

- Label above field, 8–12px gap. Inputs 36–40px, `--radius-control`.
- Single filled primary per screen, near-black (`--os-raised` fill, `--os-ink`).
- Toggles: 28×16, semantic green for on, not the brand accent.

## 7. Anti-slop checklist (agent UI)

| Tell | Instead |
|---|---|
| Accent sidebar + coloured active pill | Ground match; brightness marks the active |
| “Welcome back” greeting | Noun title (the project / scope name) |
| KPI cards with icon in tinted circle | Bare type on the panel, no icon |
| Sparkline on every KPI | Trend in its own chart, or nowhere |
| Gradient area chart | 2px straight line, flat 14% fill at most |
| Rainbow chart | One hue per chart; accent on the main data |
| Vanity numbers (“10,000+ runs”) | Real counts, summed from the rows |
| Monospace labels everywhere | Text face; mono only for IDs/code |
| Violet/indigo accent | Cyan accent; violet is generation-default |
| Glass panels / backdro-filter | Opaque surface one tone step from ground |

## 8. Import paths

- Token spec: this file, imported as `@/design/tokens`.
- Component styles: `demo/index.html` (self-contained; classes mirror §6).
- Bridge references: `references/agent-skills-patterns.md`,
  `references/popular-web-designs-bridge.md`.
