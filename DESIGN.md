# The Notebook, Design System

Visual north star for blogs.dhruvchopra.info. Pair this with two prototypes:
`design/reference.html` (the hub, the subjects, the notebook, the reading view) and
`design/travel.html` (the immersive Travel page). This file states intent and exact
tokens so the look does not drift. Where this file and a prototype disagree, the
prototype wins on behaviour and this file wins on intent.

## Concept

The blog is a portal, not a list of categories. The hub is a calm, fluid space where
the subjects drift as words rather than sit in boxes. The notebook is the spine: a single
chronological stream that holds every post. The subjects are filtered views into it.
Anything that fits no subject still lives in the notebook under Misc. Entering a subject,
or a post, is a full takeover with its own palette, type, motion, and a signature effect.

Hard rule learned in design: do not change theme or background on hover. Hover only lifts
the word and reveals its teaser. The world changes on click only.

## Architecture

- One shell, three layers back to front: a `ground` div (CSS gradient per world), a single
  full screen `canvas#fx` (the shared effect engine), then the content.
- Views: hub, world (a subject), notebook (everything), reader (a post), and travel (a
  dedicated page, see its own section). One view visible at a time, cross-faded.
- Theming is driven by a `data-theme` attribute plus CSS custom properties set on `#stage`,
  so every view inherits the active world's tokens. Switching sets the attribute and the
  canvas mode. Nothing else changes.
- The canvas is one engine with named modes. Worlds select a mode, they do not ship their
  own animation. Overlays (vignette, scanlines, grain, letterbox) are fixed elements
  toggled by opacity.

## Global tokens

- Easing: `cubic-bezier(.16,1,.3,1)` everywhere.
- Base shell background: `#07070c`.
- Type scale uses `clamp()` so it holds from mobile to desktop.
- Utility face is Space Grotesk, except Tech where it is JetBrains Mono.

## The blogging mechanism

The notebook is the front door for writing on a random basis. A post is one record:

    slug, title, cat, date, mins, draft?, body

- `cat` is one of the seven subjects, or `"misc"` for anything that fits none.
- `body` is plain Markdown: `##` heading, `>` quote, `***` divider, blank line between
  paragraphs, `*italic*` and `**bold**`. A tiny renderer turns it into the reading view.
- The notebook lists all posts, newest first, with filter chips: all, the seven subjects,
  and misc. A subject page lists only its own posts. A misc post still appears in the
  notebook, it just has no subject world.
- In production each record becomes a Markdown file. The model is identical.
- Drafts are marked "in the notebook" and never presented as published.

## The seven subjects

History was removed. The seven are Astrophysics, Travel, Cinema, Coffee, Sports, Tech,
Perfumes. Each defines display / body / utility fonts, fg / dim / accent / hairline
colours, a ground gradient, a canvas FX mode, an optional entry skin, overlays, and
whether the ground is light or dark.

- Astrophysics: Space Grotesk / Spectral. fg #eaf0ff, dim #8b94b8, accent #8fb4ff,
  line rgba(143,180,255,.16). Title weight 300, tracking -.03em.
  Ground radial #11193a to #04030a (dark). FX stars. Overlay vignette.
- Travel: see the dedicated Travel page section. Base tokens DM Serif Display / Space
  Grotesk, fg #fff3e6, accent #ff9d5c. Ground dawn gradient #1a2440 to #f0a35e. FX horizon.
- Cinema: Oswald / Spectral. fg #f2ede4, dim #9a8f7e, accent #e8c87a,
  line rgba(232,200,122,.18). Title weight 200, tracking .01em. Ground #050505 (dark).
  FX grain. Overlays letterbox, grain, vignette. Skin film (thin condensed uppercase).
- Coffee: Fraunces / Spectral. fg #3a2417, dim #8a6f54, accent #a9603a,
  line rgba(110,74,46,.22). Ground warm cream radial (light). FX steam.
- Sports: Anton / Space Grotesk. fg #f4fff4, dim #86c69a, accent #22d46f,
  line rgba(63,245,138,.18). Ground pitch stripes (dark). FX sweep.
- Tech: JetBrains Mono throughout. fg #d7ffe6, dim #5f8f73, accent #36e27b,
  line rgba(54,226,123,.14). Title weight 500. Ground #040806 (dark). FX matrix.
  Overlay scanlines. Skin terminal (entries render as `$ cat name.md`).
- Perfumes: Cormorant Garamond / Cormorant Garamond. fg #3a2230, dim #9a7488,
  accent #c27aa0, line rgba(180,120,150,.22). Title weight 500. Ground pale champagne
  radial (light). FX bloom.

Notebook and hub share a neutral theme: Fraunces / Spectral, fg #efeadf, accent #b9a6ff,
dark ink ground, FX hubstars. Misc posts read in this neutral theme.

## The reading view

The one screen people spend minutes inside. Themed to the post's subject but calm.

- Background canvas drops to about 20 percent opacity in reader; overlays are suppressed.
- Column max-width 660px, centered. Body 17 to 19.5px, line height 1.75.
- Eyebrow (subject), title (display font), byline (author, month, read time), body.
- A thin accent progress bar tracks scroll depth. Pull quotes use the accent on a left rule.
- Misc posts use the neutral notebook theme.

## The Travel page (design/travel.html)

Travel is special. It is not the generic subject list. Clicking Travel from the hub opens
a dedicated immersive page that showcases trips. In the prototype this loads as `travel.html`
in a full-screen view with a floating back control. In production it is the real
travel route.

Trips are data driven. One record per trip:

    key, country, city, flag, code, origin, year, type, domestic?, reel, ink, theme, note, photos[3]

- `type` is solo, family, or friends. `reel` is an Instagram URL, or null to show
  "before reels existed" (Singapore, 2013). `photos` are three slots; drop in URLs.
- `theme` carries the country's bg, surface, fg, accent, drawn from its flag.

Signature artifacts on the page:

- Passport: a navy and gold cover that flips open to a two-page spread holding every
  stamp. Tap a stamp to jump to that trip. The centerpiece.
- Stamps: generated per trip, inked in the country colour, country, city, year, code,
  and type, rotated slightly like real entry stamps.
- Flags: real national flags in SVG, generated (Singapore crescent and stars, Turkey
  crescent, Vietnam star, Thailand bands, UAE bands, India Ashoka chakra for Goa).
- Boarding passes: origin to destination code, dashed flight arc with a plane, flight
  number, date, class, seat, and a barcode stub with punched corners.
- Timeline: chronological, evenly spaced so multiple trips in one year never overlap.
  Solid dots for solo and friends, hollow for family. Every node jumps to its trip.

Per-country scroll theming: as each trip section enters view, the whole page recolours to
that country's flag palette via an IntersectionObserver. Passport and timeline sit in the
neutral travel theme.

Current trips: Singapore 2013 family (no reel), UAE 2022 solo, Türkiye 2023 solo,
Thailand 2024 family, Vietnam 2024 solo, Goa 2024 friends (domestic), Thailand 2025 solo.

## Motion and accessibility, the quality floor

- Respect `prefers-reduced-motion`: stop the loop, render one static frame, disable float
  and reveal transitions.
- Visible keyboard focus on every control. Esc returns to the hub or steps back.
- Responsive to mobile: subjects flow into a column, teasers show by default, the timeline
  scrolls horizontally, trip grids stack.
- Hover never triggers a theme or background change.

## What remains for implementation

1. Integrate into the real multi-page structure: home (hub), writing index (notebook),
   the seven subject pages, individual post reading pages, and the Travel page.
2. Bring across real post content; mark unwritten ones as drafts.
3. Wire the Travel page to its real photos and reel URLs, and confirm cities, airport
   codes, and origins.
4. Cross-browser and mobile QA, a performance pass on the canvas effects, and a
   reduced-motion pass on every page.
