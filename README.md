# Hermes Agent OS

A terminal-native, dark-first design system for agent UIs — built from
`design-taste-frontend` anti-slop rules and `popular-web-designs` vocabularies.
Single-accent (cyan), flat surfaces, and a Monitor surface throughout.

- **`DESIGN.md`** — token spec (color, type, space, radius, motion) + anti-slop rules.
- **`SKILL.md`** — a skill you can drop into Hermes to consume this system.
- **`references/`** — bridge docs explaining how `design-taste-frontend` and `popular-web-designs`
  vocabularies map onto seven surface archetypes.
- **`demo/index.html`** — a living, self-contained component lab (open directly in any browser):
  command palette, conversation view, status badges, activity stream, data table, settings form.

## How to use the tokens

```css
/* Copy the :root block from DESIGN.md into your stylesheet. */
:root {
  --os-canvas: #09090d;
  --os-accent: #00d1b2; /* computing cyan — the one accent */
  /* …full token set in DESIGN.md */
}
```

## Run the demo

```bash
npx serve demo/index.html
# or
python3 -m http.server 8000 --directory demo
```

## Notes

- Accent is computing cyan (`#00d1b2`) on dark; Linear's violet is remapped to cyan per `references/popular-web-designs-bridge.md`.
- Type: **Geist** (UI) + **Geist Mono** (code/numbers).
- Honors `prefers-reduced-motion`.

## License

MIT
