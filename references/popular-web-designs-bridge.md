# Bridge: popular-web-designs → Hermes Agent OS

How the 54 `popular-web-designs` vocabularies map onto agent UIs, and which
tokens to import from each. Importing a vocabulary = taking its *palette and
type roles*, not cloning its layout. The OS keeps its own surfaces and anti-slop
rules; a vocabulary only supplies the colour and type decisions.

## Choosing a vocabulary for an agent surface

| Agent surface | Best vocabularies (in popularity-web-designs) | Why |
|---|---|---|
| Monitor (activity stream, run log) | `voltagent`, `linear.app`, `opencode.ai` | Dark canvas, single accent on data, quiet chrome |
| Operate (command palette, tool panel) | `vercel`, `linear.app`, `warp` | High contrast input, keyboard-first, monospace |
| Command/Inspect (detail pane) | `linear.app`, `superhuman`, `cursor` | Focus ring, tight spacing, strong hierarchy |
| Configure (settings) | `notion`, `cal`, `linear.app` | Clear save states, progressive disclosure, low chrome |
| Decide/Learn (landing/docs) | `stripe`, `framer`, `apple` | One idea per section, hero where correct |
| Compare (run history, model selector) | `stripe`, `linear.app`, `kraken` | Aligned columns, parity of structure |
| Explore (file explorer, knowledge) | `notion`, `framer`, `miro` | Result grids, zoom/peek |

## Importing tokens: the recipe

Every `popular-web-designs/templates/<site>.md` ships a §2 Color Palette and
§3 Typography with concrete values. To import:

1. **Map roles, don't copy values.** Take the vocabulary's *ground*, *surface*,
   *ink*, *accent* and re-map to `--os-canvas`, `--os-surface`, `--os-ink`,
   `--os-accent`. Reject any value that violates a house rule (violet/indigo
   default accent, gradient backgrounds, cream+terracotta).
2. **Keep one accent.** The vocabulary's brand color becomes `--os-accent` only
   if it is used on data/the action and under 5% of the screen.
3. **Type:** take the vocabulary's families and weights. If the original is a
   proprietary font, use the template's CDN substitute (see table below). Pin
   only the weights you use (usually 400 / 500 / 600).
4. **Surfaces:** replace the vocabulary's card shadows with the OS rule (tone
   step + `1px` border, no resting shadow).
5. **Document the import** in your report: "Palette imported from
   `popular-web-designs/templates/<site>.md` §2; violet accent rejected per
   `agent-skills-patterns.md` §3; mapped to the OS ground model."

## Concrete imports (the ones the OS ships with)

The demo (`demo/index.html`) includes three ready import sets so you can swap
vocabularies with one class on `<html>`:

### Set A — VoltAgent (dark dev-tool)
From `templates/voltagent.md`:
- `--os-canvas: #050507` (Abyss Black)
- `--os-surface: #101010` (Carbon Surface)
- `--os-ink: #f2f2f2` (Snow White)
- `--os-muted: #b8b3b0` (Warm Parchment)
- `--os-faint: #8b949e` (Steel Slate)
- `--os-border: #3d3a39` (Warm Charcoal)
- `--os-accent: #00d992` (Emerald Signal Green)
- Font: `system-ui` (headings) + `Inter` (body) → OS narrows to `Geist` for both.
- **House-rule override:** no green glow effects; the accent is flat.

### Set B — Linear (dark precision)
From `templates/linear.app.md`:
- `--os-canvas: #08090a`
- `--os-surface: #0f1011`
- `--os-ink: #f7f8f8`
- `--os-muted: #d0d6e0`
- `--os-faint: #8a8f98`
- `--os-border: rgba(255,255,255,0.08)`
- `--os-accent: #7170ff` ← **rejected** (violet); OS remaps to `--os-accent: #00d1b2`.
- Font: Inter Variable → OS uses `Geist`.
- **House-rule override:** remove the semi-transparent white glow; depth is a
  lighter surface step only.

### Set C — Light (Vercel-ish)
From `templates/vercel.md` + `templates/stripe.md` blend:
- `--os-canvas: #f7f8fc`
- `--os-surface: #ffffff`
- `--os-ink: #14141a`
- `--os-muted: #5a5f73`
- `--os-faint: #9aa0b8`
- `--os-border: #d9dae2`
- `--os-accent: #2563eb` (a blue, not violet — accepted)
- Font: Geist (both faces).

## Font substitution reference (vocabulary → CDN)

| Vocabulary proprietary font | CDN substitute (Google Fonts) | OS mapping |
|---|---|---|
| sohne-var / Circular / figmaSans | Inter | → Geist |
| Geist (already on CDN) | Geist | exact |
| Berkeley Mono | JetBrains Mono | → Geist Mono |
| SF Pro | SF Pro (system) | → system-ui |
| SF Mono | SF Mono (system) | → ui-monospace |
| Airbnb Cereal / Pin Sans | DM Sans | → Geist |

```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500;600;700&family=Geist+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

## What not to import

- Gradients on grounds, buttons, text, KPI tiles (tells.md §1).
- Glass panels / `backdrop-filter` (tells.md §1).
- Icon-topped feature cards, rows of identical cards (tells.md §1, app-screens §14).
- Fake dashboards of grey bars and donut charts (tells.md §1).
- Glowing borders / neon accents (tells.md §2).
- Violet/indigo as a default accent (tells.md §4, colour.md §9).

Import colour and type. Leave decoration behind.
