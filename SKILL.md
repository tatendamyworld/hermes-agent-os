---
name: hermes-agent-os
description: >
  Use when building a user interface for a Hermes agent or CLI agent — a command palette, chat/conversation
  view, tool cards, activity stream, settings panel, or any web UI an agent renders for a human. Embodies a
  terminal-native, dark-first, single-accent design system with anti-slop house rules borrowed from
  design-taste-frontend and dev-tool vocabularies from popular-web-designs. Load DESIGN.md for tokens;
  follow the seven-stage run in this skill to produce, verify, and report a designed artifact rather than a
  generic template.
tags: [design, html, agent-ui, anti-slop, dev-tool, component, prototype]
platforms: [linux, macos, windows]
---

# Hermes Agent OS

A terminal-native design system for agent UIs, and the run an agent follows to
apply it. This is **process and taste**, not ornaments. Read `DESIGN.md` for the
tokens; follow this run to turn a brief into a decided UI.

## 0. Read me first

This skill composes two upstream skills:

- `design-taste-frontend` (from `tatendamyworld/agent-skills`) — the anti-slop
  rules, the seven surface archetypes, the inspector gate. The house rules in
  `references/agent-skills-patterns.md` are **non-negotiable** here.
- `popular-web-designs` — 54 dev-tool design vocabularies as CSS tokens.
  `references/popular-web-designs-bridge.md` maps each vocabulary to an agent
  surface so you can import a concrete look without cloning a brand.

**Do not skip the surface step.** Most agent UI slop is compositional: a centred
chat input + a column of feature cards for every surface. Naming the right
surface collapses the entropy before a single color is chosen.

## 1. Commit to a surface

An agent UI is rarely one surface, but it has a **primary** one. Name it, out
loud:

> This is a **Monitor** surface — state changes as they happen. Density and
> glanceability, not a hero.

The seven surfaces (use the archetype name, not "dashboard"):

1. **Monitor** — watching state change (activity stream, run log, status page).
2. **Operate** — taking action (command palette, file builder, tool panel).
3. **Compare** — weighing options (run history, model selector, plan comparison).
4. **Configure** — setting things up (settings, env vars, skill loader).
5. **Decide / Learn** — being convinced or taught (landing, docs, onboarding).
6. **Explore** — browsing (file explorer, knowledge search, gallery).
7. **Command / Inspect** — keyboard drill-in (detail pane, property editor).

**Rules:** a Monitor surface gets no centred hero and three feature cards. If a
screen spans two surfaces, name the primary and treat the other as secondary.
The `hero-plus-three-cards` composition is correct for **Decide/Learn only**.

## 2. Gather context

Read what exists before inventing:

- **Repo?** Inspect the actual styling method (Tailwind v4, CSS modules,
  styled-components, plain CSS), the component files, and the design tokens
  already committed. Match spacing, radii, and interaction patterns first;
  restyle only what the surface demands.
- **Brand?** If the product owns colours, pass `--keep palette` mentally: map the
  brand's values onto the OS roles (`--os-accent`, `--os-ground`, `--os-ink`)
  and take only composition. The OS still bans violet/indigo default accents and
  cream+terracotta.
- **Copy?** Use the user's words for labels. Never invent proof (quotes, logos,
  "10,000+ users"). Where the brief is short, write sample values from the
  product's real domain so the design is seen at full density and you list them
  to replace later.

## 3. Name the screen's one question

Every screen answers one question first (app-screens.md §2.2). For an agent UI:

> "What just happened in this run?"  (Monitor — activity stream)
> "What can I make the agent do next?" (Operate — command palette)
> "Where did this output come from?"  (Inspect — tool detail)

The largest block must answer it, and the largest figure must answer the title's
sentence. Delete any block with no question.

## 4. The run (agent UI)

### 4.1 Shell & canvas

Build the frame first, empty. For a desktop app, a 220–240px labelled sidebar
on the canvas ground; for an in-agent web widget, a single `--os-surface` card.
**The product's mark and name** sit at the top — a monogram in the accent, never
a black square with a letter. The canvas uses one gutter horizontally and
vertically; content lands on both right edges.

### 4.2 Tokens

Read `DESIGN.md` and set the CSS custom properties on `:root`. Everything
downstream references tokens only — a raw hex in a component is a second palette
starting. The light mode is a preference (`prefers-color-scheme: light`), never
the default for an agent UI.

### 4.3 Components

Built from the tokens (DESIGN.md §6). The agent UI palette is small; build the
ones the surface asks for and stop:

- **Command palette** — Operate surface. The `/` entry point. Flat input, monospace,
  results with a single accent status dot and one hint line.
- **Message / tool card** — Monitor surface. Flat `--os-raised` panel, `1px` border,
  `--radius-panel`, no shadow. Tool cards carry the tool name in `--os-accent`.
- **Status badge** — pill, one hue, inverted ink. No tinted circles.
- **Data table** — right-aligned tabular figures, `4.5:1` minimum, one status mark
  per row.
- **Form control** — label above field, one filled primary, near-black.

### 4.4 Density

Three settings moved together: row pitch, padding, type size.

- **Sparse** (6–25 data points): homes, settings, single object. Card pad 20–24px,
  body 14–16px.
- **Medium** (25–50): agent dashboards, lists. Body 14px, rows 44–56px.
- **Dense** (≈120): activity streams and logs read in long sessions. Body 12px,
  rows 32–40px, but framed with generous air (gutters 48px, band gaps 48px).

Do not shrink type and keep the padding.

### 4.5 States & semantics

For every component build: default, hover/focus, loading, empty, error, success.
The **empty state** keeps its frame and adds one sentence; the **zero** prints in
the normal style (`0`, `$0.00`, `0% (0)`); the **error** lives *inside* the block
that failed, never a full-screen banner.

### 4.6 Real content

- **Data that tells the truth.** A count shown twice is one variable. Totals sum
  from the rows, recomputed client-side; deltas name their baseline
  ("3h 40m before close"). New accounts show 1, 6, or 9 of something, drawn
  honestly at its real size.
- **One element only this product has** — a power-flow for an energy home, a
  sell-out strip for a marketplace, a deploy's commit hash as the hero. For an
  agent: the real run ID, the real tool call signature, the real file path.

### 4.7 Motion

Motion is the last pass, never a rescue.

- Press and hover feedback on every control.
- One signature move at most (an agent's: a tool card expanding on invocation).
- Reduced motion keeps the finished state; colour/opacity only.
- No preloader, no counter, no custom cursor.

### 4.8 The agent-specific live thing

One live, thumb-sized thing per first screen, built from the agent's real
context: a typed command echoing as it runs, a status dot that flips from
accent → green on completion, a file list that grows as the agent writes it. It
uses the subject's real content, has a plain (no-script) fallback, and never
blocks the work.

## 5. Gate (verify)

Before declaring done, confirm:

- [ ] File exists and opens; no console errors.
- [ ] One `h1`. A heading on every section.
- [ ] Equal outer margins; no void taller than half a screen, each closed by
      content on its far side.
- [ ] Colour: accent under 5% of the screen, on data/the action only.
- [ ] Type: no headline over two lines at any width; one family for UI + mono
      for code.
- [ ] Components: flat surfaces (ground + 1 tone step), no resting shadows,
      `4.5:1` text contrast.
- [ ] Run the anti-slop self-audit (DESIGN.md §7): score out of 10, list which
      tells fired, fix the compositional ones (3, 8, 10) by re-layout, not recolor.

## 6. Report (briefly)

Tell the user, concisely:

- the surface named and why,
- the token source (`DESIGN.md` + which popular-web-designs vocabulary was imported),
- the one live thing and the screen's question,
- the anti-slop score and any tells left (be honest),
- sample data that must be swapped for real data, listed so it can be found.

Do not claim a check you did not run.

## References

- `DESIGN.md` — token spec (colors, type, motion, components).
- `references/agent-skills-patterns.md` — anti-slop rules, seven surfaces, inspector gate.
- `references/popular-web-designs-bridge.md` — 54 vocabularies mapped to agent surfaces.
- `demo/index.html` — living component lab.
