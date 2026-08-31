# @kineticstudio/astro-design-system

The design system document every Kinetic Astro site renders.

**The brand comes from the site; the document comes from here.** Redesign this
package once and every site that depends on it moves on its next build. That
is the whole reason it is a package rather than a file each site copies:
copies drift the moment anyone redesigns.

## Use

```bash
npm i github:kineticstudio/astro-design-system
```

```astro
---
import DesignSystem from "@kineticstudio/astro-design-system";
import Stage from "@kineticstudio/astro-design-system/Stage.astro";
import Hero from "../components/Hero.astro";

const PALETTE = [
  { name: "Ground", hex: "#000", token: "--bg", on: "#fff", span: 7, tall: true,
    use: "The ground the brand leads with." },
];
const TYPE_SCALE = [
  { label: "Display", size: "clamp(40px,6vw,78px)", face: "var(--display)",
    weight: 300, lh: 0.96, sample: "A headline" },
];
---
<Base title="Brand — Design system">
  <DesignSystem brand="Brand" palette={PALETTE} typeScale={TYPE_SCALE}>
    <Fragment slot="buttons"><Button label="Primary" /></Fragment>
    <Fragment slot="components">
      <Stage name="Hero" file="src/components/Hero.astro"><Hero /></Stage>
    </Fragment>
  </DesignSystem>
</Base>
```

## What the host site must provide

`tokens.css` declaring `--bg --ink --muted --surface`, `--contrast-bg
--contrast-ink --contrast-muted --contrast-surface`, `--accent`, `--display
--body`, `--gutter`. The document styles itself entirely from those, which is
why one stylesheet looks like each brand without knowing any of them.

Brand SVGs in `public/brand/` (`mark.svg` at minimum, for the cover).

## Two rules it enforces

1. **The catalog shows the REAL components**, passed through slots. One that
   redraws its examples by hand is a second implementation, and it starts
   lying the first time somebody changes the real one.
2. **The `data-sys` headings are the structure.** Kinetic Creative Studio
   frames this page and builds its left rail by scanning them: an h2 opens a
   folder, the h3s under it are its contents. Component-internal headings
   carry no marker, so a component's own copy never becomes a rail folder.

There is deliberately **no navigation in the page**. The studio rail is the
navigation; a page with its own sidebar shows two rails for one document.

## Four things that cost real time

Each is commented where it lives. This is the index.

- **The reveal-killer script.** Components carry the host site's entrance
  choreography, so in a catalog everything below the fold renders as an empty
  band until a scroll sweep fires. `DesignSystem.astro` reveals on load.
- **`transform` on the component stage.** A component that positions itself
  fixed is fixed to the *viewport*, so it escapes its frame and floats over
  the page. The transform makes the stage its containing block.
- **`!important` on a reduced-motion draw reset.** Per-graphic rules have the
  same specificity as the shared override, so without it the bundler's emit
  order decides, and outlines never draw.
- **`html { scroll-behavior: smooth }` in a host site** makes even a plain
  `scrollTo` animate, so the studio's anchor ride suspends it for the duration.

## Slots

| slot | what goes in it |
| --- | --- |
| `buttons` | real buttons, rendered on the dark ground |
| `buttons-light` | the same buttons in their light-ground tones |
| `illustration` | `<figure>` per drawn figure, each its own component |
| `components` | one `<Stage>` per real component |
