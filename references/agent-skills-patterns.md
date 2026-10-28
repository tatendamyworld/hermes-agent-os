# Bridge: agent-skills patterns → Hermes Agent OS

How the `design-taste-frontend` anti-slop rules apply to agent UIs. This is the
reasoning behind every "Instead" in `DESIGN.md` §7.

## The seven surfaces, mapped to agent UI

A generated agent UI reaches for the same shape everywhere: a centred input + a
column of three feature cards. Name the surface first; the composition follows.

| Surface | Agent UI example | Anti-slop import |
|---|---|---|
| **Monitor** | Activity stream, run log, status page | Density, glanceable hierarchy. No hero. |
| **Operate** | Command palette, file builder, tool panel | Action affordances + selection state dominate. |
| **Compare** | Run history, model selector, plan diff | Aligned columns; parity of structure; one differentiator. |
| **Configure** | Settings, env vars, skill loader | Progressive disclosure; clear save/validation. Low decoration. |
| **Decide/Learn** | Landing, docs, onboarding | One idea per section. Only here is a hero usually correct. |
| **Explore** | File explorer, knowledge search | Filters, result grids, zoom/peek. |
| **Command/Inspect** | `/inspect`, detail pane, property editor | Speed and focus over breadth. |

## The house rules (the non-negotiables)

From `SKILL.md` §"House rules". Applied to agent UI:

1. **Headlines two lines or fewer** — an agent UI's labels must fit. Long strings get a supporting line.
2. **A clear title on the first screen** — the project / scope name in the `h1`. Not "Welcome back, Alex".
3. **Equal margins; no section huddled on one side** — a chat input floating center-stage with nothing
   beside it is a hole. Close it: the conversation, a preview pane, the status rail.
4. **Statements are paragraphs** — `36px` or less on desktop. No `48px` agent name as a sentence.
5. **No eyebrows** — no tiny "Beta" pill above a heading. Fold it in or delete.
6. **No monospace outside real code** — labels and nav in the UI face; mono only for commands/IDs/code.
7. **No index numbering** — no `01 / 02 / 03`. Order by position, size, or the words.
8. **No thin rules** — `1px` lines only where `app-screens.md §13` allows them (cards, tables, controls).
9. **No gradients** — flat color or a real interface fragment for the hero.
10. **No shadows on things at rest** — surfaces separated by a tone step; shadow on a menu only.
11. **No assistant palette** — no cream+terracotta, no violet/indigo default accent. (See §3 below.)
12. **No tiny uppercase letter-spaced labels** — sentence case at readable size.
13. **No icon-topped feature cards / no rows of identical cards** — the dealt blueprint. In an agent UI,
    this is the "three tool cards" trap; each tool card should carry its own real result.
14. **No glass** — no `backdrop-filter: blur`. Opaque surface, one tone step.
15. **No emoji, no gradient blobs, no decorative shapes, no fake dashboards** — a real interface fragment
    (`data-ui`) or nothing.

## What bans violet/indigo

`tells.md` §1, §4: violet/indigo gradients and the default violet accent are the
first-wave signature of generated pages (22% gradients on weak sites vs 52%).
`colour.md` §9: violet and indigo are banned "as house style" because they read
as the model's favorite, not a brand choice.

**Exception:** if the product's *own* brand is violet/indigo, keep it
(`--keep palette`), document `--allow palette-purple`, and note it. Otherwise
the OS accent is computing cyan (`#00d1b2`) — a deliberate, un-banned choice.

## App-screen tells (dashboards, admin, agent panels)

`app-screens.md` §14 lists 22 generated-dashboard habits. The worst offenders
for agent UIs, and what replaces them:

| Tell | Instead |
|---|---|
| Dark saturated sidebar + coloured active pill | Sidebar on the page ground; active by brightness only |
| "Welcome back, Alex" + waving hand | Noun title at 24px: the project name |
| Four KPI cards, each with icon in tinted circle | One panel split by rules, or one figure per card; no icons |
| Green "+12.5%" pill on every KPI | Delta in words with its window; grey for zero; "+4 (new)" |
| Sparkline on every KPI | Trend in its own chart, or nowhere |
| Gradient area chart with smoothed spline | 2px straight line, ≤14% flat fill |
| Rainbow palette / hue per bar | One hue per chart; accent on the main data only |
| Zebra table under uppercase tracked headers | 12px grey sentence-case headers; rules or space |
| Left-aligned numbers in proportional digits | Right-aligned tabular figures |
| Saturated status chips with white text | Pale tint with dark text of the same hue |
| Accent on every button, tab, link | No filled primary on 12/23 studied screens; near-black primary |
| Hero header over 32px | Noun title; figures 16–40px, nothing at 48 |
| Vanity totals ("10,000+ teams") | Real counts, summed from the rows, listed to replace |

## The inspector gate, for agents

`SKILL.md` §7 of the source maps cleanly:

- **h1 + section headings** — every agent screen names its project.
- **equal margins** — the chat rail and the preview pane share the page's edges.
- **colour share** — accent under 3% on light, under 5% on dark.
- **no placeholders** — `Acme`, `John Doe`, lorem ipsum fail. Use the product's
  real values and list them to swap.
- **figures add up** — a token count, a cost, a step number: compute from rows.

## The slop audit (10 tells), adapted

Use `DESIGN.md` §7. Score out of 10; fix compositional tells (3, 8, 10) by
re-layout, cosmetic ones (1, 2, 9) by recolor/retype, decorative ones (4–7) by
removal.

1. Tech gradient — cyan is flat, not gradient.
2. Generic tech hue — violet/indigo banned (see above).
3. Feature-tile grid — the "three tool cards" trap.
4. Accent rail — no left-strip decoration.
5. Unearned blur — no glass.
6. Monument stat — no oversized vanity number; the answer figure is ≤32px.
7. Icon topper — no rounded-square icon above every heading.
8. Center stack — one anchored edge for nav text.
9. Default type — Geist chosen, not generic.
10. Wrong surface — hero on a Monitor surface, never.
