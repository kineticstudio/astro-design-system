# Production component library

`Library.astro` is a reusable catalog shell for Kinetic Astro sites. The
existing `DesignSystem.astro` and `Stage.astro` exports remain compatible.

Host pages supply a brand name and `Entry[]`. Each entry includes an id,
name, group, summary, source file, prop contract, runnable Astro example,
production usage, content guidance and named preview URLs.
Pass `logoSrc` to show the site's actual logo; omit it for a text-only brand.

Organize by scale: Foundations, Elements, Graphics, Composed components,
Sections, Navigation & footer. Never group or filter by page. Production
page names belong in each entry's `usedBy` metadata, visible on the base view.

The library is one interactive inventory with its own searchable navigation.
Every base-view specimen is a focusable, interactive iframe using the real
component source. There is no thumbnail overlay or separate inspector. The
iframe uses its actual available width, with scrolling for tall sections;
CSS, IDs, scripts and modal ownership remain isolated from the catalog.
Optional `overview` dimensions provide the initial frame height (260–800px).
Existing overview URLs can remain for compatibility but are not the base view.

The library shell always uses the dark Creative Studio palette, including
navigation, card headers, controls and the details sheet. Preview light/dark
state is separate from shell styling; never apply it to the library chrome.
Theme changes reveal loaded previews with a short left-to-right swipe and
a bounded stagger for visible cards. Rapid changes cancel earlier swipes;
reduced motion skips them. Initial preference restoration does not animate.
Use explicit `index.html` preview URLs for Studio's mounted file server,
including URLs stored in variant JSON and light/dark data attributes.

Each variant can supply `surfaces: { light, dark }` preview URLs. The global
toggle switches every specimen to that surface, retaining its selected
example. Generate those URLs using real tone/theme props. Set `theme` to
`adaptive` for supported variants and `fixed` for authored brand treatments;
never invert a production palette to fake support. Fixed sections retain
their native treatment and are labeled Brand surface. `variantControl: false`
is useful for a button family that displays its treatments together.

Details opens a native modal side sheet containing usage, source, content
guidance, props/slots and a copyable Astro example. Escape, dismissal and
focus return follow native dialog behavior. Search and surface controls are
keyboard accessible; reduced motion is respected.

Foundations must derive their values from the site's real tokens and use its
existing type primitives/assets, not a parallel palette or type stylesheet.
In Elio, the homepage's `Hero.astro` is the heading/lede/action lockup, and
`OrbitBand.astro` is its independent image-orbit section. Keep them separate.

Creative Studio mounts the host's `/system/index.html` through
`site.production.system`. Set its name and `site.stageLabels.system` to
`Components`. Set `site.shell` to `neutral` for a black review environment.
Set `site.production.system.navigation` to `internal`: Components is a
single destination in Studio, while the catalog owns its complete inner
rail. The document also declares `data-library-navigation="internal"` so
legacy heading discovery does not duplicate the component list. Keep the
existing `system` identifier so old links and comments remain intact.

For future Sanity work, normalize fetched content into the same prop shapes
at build time. The catalog should distinguish props-ready components from
authored sections that need a content extraction before CMS integration.
The library itself neither needs nor stores CMS credentials.
