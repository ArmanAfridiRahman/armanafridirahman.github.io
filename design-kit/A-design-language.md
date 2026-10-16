# Folio · Design language

> A complete specification for a one-page editorial portfolio: warm paper, ink, one accent, hand-set
> type, moulded controls, working figures and hairline 3D drawings. It contains structure, values,
> components, motion and engineering; every word on a Folio page comes from the person it is built for.
> Use it together with **`B-build-rules.md`**, and read that file first.

## Contents

- [0 · Read me first](#0--read-me-first)
- [1 · Concept](#1--concept)
- [2 · Architecture & file anatomy](#2--architecture--file-anatomy)
- [3 · Content model & voice](#3--content-model--voice)
- [4 · Colour, surface & light](#4--colour-surface--light)
- [5 · Typography & marks](#5--typography--marks)
- [6 · Grid, spacing & breakpoints](#6--grid-spacing--breakpoints)
- [7 · Bar, theme switch, cover & opening line](#7--bar-theme-switch-cover--opening-line)
- [8 · Orbit — the pinned stage and the contents](#8--orbit--the-pinned-stage-and-the-contents)
- [9 · At a glance & the letter](#9--at-a-glance--the-letter)
- [10 · Feature articles & photo slots](#10--feature-articles--photo-slots)
- [11 · Next issue, contact & footer](#11--next-issue-contact--footer)
- [12 · Interactive figure kit](#12--interactive-figure-kit)
- [13 · Browser-tab mock & compact figures (≤900px)](#13--browser-tab-mock--compact-figures-900px)
- [14 · 3D stages & scene technique](#14--3d-stages--scene-technique)
- [15 · The signature object](#15--the-signature-object)
- [16 · Motion system](#16--motion-system)
- [17 · Accessibility, performance, dark mode & print](#17--accessibility-performance-dark-mode--print)
- [18 · Responsive matrix, quality bar & acceptance tests](#18--responsive-matrix-quality-bar--acceptance-tests)
- [Appendix A · Token sheet](#appendix-a--token-sheet)
- [Appendix B · Page skeleton](#appendix-b--page-skeleton)
- [Appendix C · Constants registry](#appendix-c--constants-registry)

---

## 0 · Read me first

### 0.1 What Folio is

Folio is a design language for a one-page personal portfolio that reads like an issue of a small,
independent print magazine: warm uncoated paper, near-black ink, one muted accent sampled from the
person's own photograph, five typefaces with one job each, hand-set display type, a few pencil marks
and taped prints, controls that look pressed out of the paper itself, and hairline 3D drawings you can
turn in your hands. Every case study is a feature article, and the strongest ones carry a working
figure, a small interactive model of the idea behind the work, captioned "Fig. N — try it".

This document is the complete specification of that language. It describes structure, values,
components, motion, interaction and engineering. It contains no one's content: every name, headline,
project and sentence on a Folio page comes from the person it is built for, through the content
model in §3. It is paired with a rulebook, **`B-build-rules.md`**, which governs the process: what to ask,
what order to build in, what may be adapted and how the result is proven.

### 0.2 How to read it

| When | Read |
|---|---|
| Before anything else | `B-build-rules.md`, completely. |
| Before writing any code | §0 to §6 and Appendix A, completely. Skim the headings of §7 to §18. |
| Before building each part | That part's chapter, completely, and Appendix B's skeleton for it. |
| Before delivering | §17 and §18. |

Each component section follows the same order: **Purpose → Anatomy → Spec → States → Responsive →
Motion → Accessibility → Pitfalls → Adapting**. "Pitfalls" records failures that are easy to cause and
hard to see; "Adapting" says how to re-tune the part for different content.

### 0.3 Notation

| Notation | Meaning |
|---|---|
| **MUST**, **MUST NOT** | Required. A page that breaks it is not Folio. |
| **SHOULD**, **SHOULD NOT** | Expected. Deviate only for a reason you can state. |
| **MAY** | Permitted option. |
| `{{key}}` | A field of the content model (§3), e.g. `{{fullName}}`, `{{features[i].headline}}`. |
| Values in code blocks | Exact. Copy them as written: no rounding, no unit changes, no "equivalent" rewrites. |
| `Why:` | The reason behind a rule. Keep the rule even if you would have chosen differently; the reason is usually a browser behaviour or a visual effect that is not obvious. |
| `≥ 1100` / `≤ 760` | Viewport width in CSS px, as in `@media (min-width: 1100px)` / `(max-width: 760px)`. |
| `col 3 / bleed-end` | A `grid-column` value on the `.g` grid (§6). |
| `L1`–`L7` | Feature layout templates (§10). |
| `A0`–`A5` | Figure archetypes (§12). |

### 0.4 Platform

| Item | Requirement |
|---|---|
| Delivery | One `index.html` plus `assets/`. No framework, no build step, no CSS framework, no runtime request to any other origin. |
| Browsers | Current evergreen browsers. `color-mix()` and CSS custom properties are required. View Transitions, the Popover API and `CloseWatcher` are used where present and degrade cleanly where absent. |
| Libraries | three.js r149 (`assets/vendor/three.min.js`), loaded with the page when the page has a 3D object. A PDF library only if a figure produces a PDF, loaded on demand (§2). |
| Fonts | Five families, nine self-hosted WOFF2 files (§5). |
| Hosting | Any static host. GitHub Pages works as-is. |

### 0.5 Document map

| § | Chapter | Use it for |
|---|---|---|
| 1 | Concept | The magazine metaphor, the principles, what Folio is not. |
| 2 | Architecture & file anatomy | File layers, banners, the head, scripts, contracts between layers. |
| 3 | Content model & voice | Every field the page needs, length limits, missing-content variants, how the words sound. |
| 4 | Colour, surface & light | Tokens, the accent method, grain, the soft-UI light model, surfaces, radii, shadows. |
| 5 | Typography & marks | Fonts, the type scale, hand-set headlines, emphasis, brush, pencil and loop marks. |
| 6 | Grid, spacing & breakpoints | Document base, the 12-column grid with bleeds, section rhythm, every breakpoint. |
| 7 | Bar, theme switch, cover & opening line | The first screen. |
| 8 | Orbit | The pinned stage, the side lanes and the contents. |
| 9 | At a glance & the letter | The summary card and the editor's note. |
| 10 | Feature articles & photo slots | The shared anatomy, layout templates L1–L7, pull quotes, taped prints. |
| 11 | Next issue, contact & footer | The inverse card, the e-mail line, the colophon. |
| 12 | Interactive figure kit | Trays, controls, drawers, readouts, figure archetypes A0–A5. |
| 13 | Browser-tab mock & compact figures | The tab mock, launchers, sheets and ticket rows at ≤ 900px. |
| 14 | 3D stages & scene technique | The Stage engine, labels, materials, glow, flights and paths. |
| 15 | The signature object | The hand-turnable object in the orbit and its archetypes. |
| 16 | Motion system | Arrivals, every animation, easing palette, reduced motion. |
| 17 | Accessibility, performance, dark mode & print | Cross-cutting rules. |
| 18 | Responsive matrix, quality bar & acceptance tests | What every part looks like at every width; tells; anti-patterns; tests. |
| A | Token sheet | Copy-paste CSS for every token. |
| B | Page skeleton | The complete markup skeleton. |
| C | Constants registry | Every number used in motion and interaction. |

### 0.6 Glossary

| Term | Meaning in Folio |
|---|---|
| **Paper** | The page surface, `--paper`, always with its grain. |
| **Ink** | The text and line colour, `--ink`. Also the fill of anything that is "on". |
| **Plate** | A flat surface one shade off the page (`--paper2`), no shadow: trays, the glance card. |
| **Well** | A surface pressed into a plate (`--paper` with an inner shadow): 3D stages, launcher icons. |
| **Moulded** | A control drawn as the surface itself pushed up (`--raise`) or pressed in (`--sink`). |
| **Print** | A taped, tilted photo slot on near-white stock. |
| **Mark** | A hand-made SVG stroke: sand brush, green pencil underline, green loop. |
| **Hinge word** | The one italic word or phrase in a headline. |
| **Ink word** | A word thickened with `.ink` (an ink-spread text shadow), Folio's only "bold". |
| **Frame** | The content width: twelve columns, capped by `--max`. |
| **Bleed** | A part that runs past the frame to the screen edge. |
| **Lane** | One of the two side columns of the orbit where words travel past the stage. |
| **Stop** | One entry of the contents list, set in a lane. |
| **Arrival** | The scroll-driven spring by which an element travels into place (`data-fly`). |
| **Signature object** | The hand-turnable hairline 3D object pinned in the orbit, "Fig. 0". |
| **Figure** | An interactive model inside a feature, in a tray, "Fig. N". |
| **Archetype** | One of the figure patterns A0–A5. |
| **Template** | One of the feature layouts L1–L7. |
| **Drawer** | A `<details class="dock">` that folds a figure's settings into one line. |
| **Launcher / sheet** | At ≤ 900px, a browser-tab mock folds into a launcher row and opens as a full-screen sheet. |

---

## 1 · Concept

### 1.1 The idea

A Folio page is **one issue of an independent print magazine**, set on warm uncoated paper; the person is both its
subject and its editor. Between the spreads sit **engineering plates you can operate** (hairline technical drawings
and working figures), a few **pencil marks and taped prints** left on the desk, and controls that look
**blind-embossed into the same sheet** rather than placed on top of it.

It reads top to bottom as a document, never as a widget gallery or an app shell.

### 1.2 Metaphor map

| Issue element | Page section | Id / class | Job it does |
|---|---|---|---|
| Running head | top bar | `header.bar` | `{{fullName}}`, five section links, the theme switch; opaque paper, one hairline (§7) |
| Cover | name across a photo card | `section.cover` | Identity in one image: `{{portrait.src}}` in a rounded card, `{{role}}` and `{{location}} · {{coordinates}}` in its top corners, `{{fullName}}` on one line, cut by the card edge and carried on by an ink copy (`.mast-echo`) (§7) |
| Opening line | one display statement | `section.opening` | The thesis, `{{opening.line}}`: one stamped word, one italic word on a brush swipe (§7) |
| Contents ("Inside") | the orbit | `section#orbit` › `nav#inside` | The signature object (fig. 0) holds the middle while contents stops `01 … N` pass in the side lanes (§8, §15) |
| At a glance | one bleeding card | `section#glance` | The whole issue for the thirty-second reader: portrait band, name, role, pitch, three actions, six facts, stack (§9) |
| Editor's letter | the note before the work | `section#letter` | Voice and stance: a ringed word, a drop cap, two newspaper columns, a taped print (§9) |
| Features No. 01–N | case-study spreads | `article#f1 … #fN` | One idea each: the same seven parts, a different composition per spread (§10) |
| Figure plates | tray inside a feature | `.insert` | "Fig. N — try it": a working model of the feature's idea, with an honest note on what is simulated (§12, §13, §14) |
| Pull quotes | display italic | `.quote` | The feature's principle in one sentence; 3–4 per issue (§10) |
| Tipped-in prints | taped photo slots | `.snap` › `.snap-in` | Off-white tilted prints, hatched until a photo exists; at most three (§10) |
| Next issue | the one inverse card | `section#next` | What is in progress; the only large ink mass on the page (§11) |
| Contact | the e-mail as display type | `section#contact` | `{{email}}` as one line of display serif, plus `{{links}}` (§11) |
| Colophon | footer | `footer` | Copyright, one honesty line about the figures, typefaces named in mono (§11) |

Figure numbers count figures in order of appearance, independently of feature numbers; a feature without a figure
uses none. The signature object is fig. 0.

### 1.3 Mood and influences

**Temperature:** quiet, slow, confident, slightly bookish. The page is mostly empty paper; type does the work;
colour is a rumour. It looks *made by hand by someone technical*: every start sits on a grid line, but each spread
is composed by eye.

| Influence | What Folio borrows |
|---|---|
| Independent quarterly | Issue vocabulary (No. 01, Fig. 1, Next issue, colophon), top lines on a rule, drop cap, pull quotes, wide margins, asymmetric spreads |
| Uncoated stock and offset ink | Greige paper with visible tooth, warm near-black ink, ink spread as the only bold |
| Engineering drawing, patent plate | 1-device-pixel line work in three opacities, near-orthographic lens, serif name over tiny caps, no shading |
| Editor's desk | Taped prints, one pencilled note with an arrow, a pen ring round one word, a dry-brush swipe |
| Letterpress blind emboss | Controls are the sheet itself pushed up or pressed in, lit from the top-left |
| Museum wall label | A mono "fig. 0" line over a small sans caption beside the object |

### 1.4 Principles

1. **Ink on paper, never white or black.** Light paper `#e9e6de` / ink `#151412`; dark `#1c1b18` / `#eeebe3`; no
   `#fff` or `#000` anywhere. Why: pure values read as a screen, not as printed stock.
2. **One accent, in small doses.** A single green sampled from the portrait, below 0.1% of the page area, used only
   as marks (§4). "On" fills are ink, not green: `--gface: var(--ink)`, `--on-gface: var(--paper)`. Why: a colour
   seen rarely carries meaning; a green filled button turns the issue into an app.
3. **Only controls are moulded.** Anything pressable or editable is raised out of, or sunk into, the surface it sits
   on (face = `--surf`); trays, cards, photos, text and drawings stay flat. Why: moulding marks what can be touched;
   on everything it turns paper into plastic.
4. **Emphasis by italics, colour or ink spread, never bold.** The serif loads 400 and italic only; the stamp (`.ink`)
   is a sideways text-shadow in the text colour. At most one italic hinge word and one stamped word per headline,
   never the same word. Why: weight contrast is a UI habit; print emphasises by posture.
5. **Big meets small, nothing between.** Display type pairs with an 11–13px mono or sans companion on the same rule;
   only the lede sits between. Why: the 8 : 1 to 10 : 1 jump is what reads as print.
6. **Hand marks are rare.** About ten on the whole page, never more than two in one 1440 × 900 view. Why: rarity is
   what makes a mark read as the editor's hand instead of decoration.
7. **Spacing by eye, in long/short pairs.** Each section owns its top margin as a `clamp()`; gaps alternate long then
   short, longest before the work and before the end. No spacing scale. Why: pairs read as spreads and pace the
   reading.
8. **No two consecutive spreads alike.** One feature anatomy, a different placement per feature at ≥ 1100px; even
   the top rule varies. Why: a repeated template reads as a list, not an issue.
9. **Bleeds alternate.** Glance card off the right, one figure tray off the left, Next card off the right; the
   bleeding side squares its corners and adds the gutter to its padding. Why: the swing moves the eye; content
   stays on the grid.
10. **Drafting, not rendering, for 3D.** Hairlines in three ink opacities, tray-coloured fills, a 30° lens; no
    lights, shading, shadows or textures; glow points alpha-blended, never bloomed (§14). Why: a drawing belongs on
    paper; a render is another medium.
11. **Every figure is honest.** Each tray header pairs "Fig. N — try it" with a plain note on what is simulated and
    what is real; generated documents are stamped as sample data; the colophon repeats it once. Why: a working
    model earns trust only if it never passes invented numbers off as real.
12. **Motion explains, never decorates.** Arrivals come from the centre and settle into their columns, so the layout
    visibly assembles; state changes animate; every infinite or decorative animation stops under
    `prefers-reduced-motion` (§16). Why: motion that carries no information competes with reading.
13. **Everything readable without JS or motion.** Content is in the HTML; arrivals hide only under `html.js` and CSS
    reveals them after 2.5 s if the script never runs; reduced motion never hides anything; print flattens the page
    (§17). Why: the issue is a document first.
14. **Re-inked, not inverted.** Dark mode redefines every token: charcoal paper, cream ink, a lighter sage accent;
    photos unchanged; the inverse card stays opposite to the page (§4). Why: inversion turns paper into a screen.

### 1.5 Folio is not

- A SaaS landing page: no centred hero, no feature-card rows, no coloured section bands, no CTA strips.
- Neumorphism: only controls are moulded.
- Glass, blur or gradient: nothing is translucent over content; the only fades are the photo's own.
- A dark terminal portfolio: dark mode is warm charcoal paper; mono is for notes, never the main voice.
- A project-card grid with thumbnails and tag pills: each case study is a full spread.
- A scrapbook: the base is strict editorial order; marks are rare.
- A minimal white template: without the warm stock and grain it reads clinical.

The full anti-pattern list with reasons is in §18.

### 1.6 Measured composition (1440 wide, light)

| Surface | Token | Share of page area |
|---|---|---|
| Page paper with grain | `--paper #e9e6de` | ≈ 62% |
| Trays, glance card, wells | `--paper2 #e0ddd3` | ≈ 26% |
| Photography (cover, glance band) | — | ≈ 4% |
| Solid ink (Next card, primary buttons) | `--ink #151412` | ≈ 2% |
| Text and line pixels | `--ink`, `--muted #54514a`, hairlines | ≈ 5–6% |
| Accent outside the photo | `--accent #546634` / `--accent-ink #435425` | < 0.1% |
| Sand | `--sand #9a7650` / `--sand-ink #7a5b3b` | < 0.1% |

Plates sample at exactly `#e0ddd3` (no noise); the grained paper averages `#e5e2da` (per-pixel deviation ≈ 20):
strong tooth that averages back to the paper colour.

Pause above each section, in page order (px; `/` joins a long/short pair):

```text
opening 101 · orbit ≈100 · glance 43 · letter 173 · F1 202 / F2 137 · F3 216 / F4 101
· F5 173 / F6 86 · F7 187 · next 158 · contact 230 · footer 130
```

| Fact | Value |
|---|---|
| Page length | ≈ 18,900px ≈ 21 screens |
| One body line (17px × 1.68) | 28.6px; pauses read as 3–8 empty lines |
| Empty paper inside the orbit | ≈ 70% |
| Section background changes | none; sections separate by air and 1px rules only |
| Hand marks / stamped words / pull quotes / prints | ≈ 10 / ≈ 5 / 3–4 / ≤ 3 per page |
| Green marks in one screen | about a dozen at most; more is wrong |

---

## 2 · Architecture & file anatomy

### 2.1 Delivery model

- One `index.html` **MUST** hold all markup, exactly one `<style>` and every script the site writes. No framework, build step, CSS framework, icon font, CDN or runtime cross-origin request.
- Evergreen browsers: `color-mix()` without fallback; View Transitions, popovers and `CloseWatcher` where available (2.6).
- The file ships unminified. Why: gzip takes it to a quarter, and it stays editable in place.

```text
index.html  site.webmanifest  robots.txt  sitemap.xml
favicon.svg  icon-192.png  icon-512.png  apple-touch-icon.png (180)
og-card.jpg (1200 × 630)      portrait.jpg (440 × 550, JSON-LD only)
assets/portrait.webp          cover photo behind --me (§4), 1500 × 900
assets/fonts/*.woff2          the nine faces of §5
assets/vendor/three.min.js    three.js r149 (three@0.149.0), loaded with the page
assets/vendor/{{pdfLib}}.js   only if a figure makes a file (e.g. jsPDF 2.5.1 UMD), loaded on intent
```

Root files serve browsers, crawlers and previews, not the page. Manifest: `standalone`, `background_color` and `theme_color` `#e9e6de`. Vendor files **MUST** be unmodified official builds (an update is a file swap); site code uses r149 APIs only (§14).

### 2.2 The four layers

| Layer | Codes | Where |
|---|---|---|
| **A · HEAD** | `A.01`–`A.09` | `<head>`, before `<style>` |
| **B · STYLES** | `B.01`–`B.23` | one `<style>`, the last element in `<head>` |
| **C · PAGE** | `C.01`–`C.(9+N)` | bar, `<main>`, footer |
| **D · SCRIPTS** | `D.01`–`D.10` | after `</footer>` |

C order: bar · cover · opening line · orbit · at a glance · letter · `C.07`…`C.(6+N)` features (`N` = count) · next issue · contact · footer. Codes are search anchors and are never renumbered; a new concern takes the next free code in its layer.

**Banner grammar (verbatim).** A layer rule, a head part, a style part, a group:

```text
<!-- ━━━━━━━━  B  ·  STYLES  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ -->
<!-- ── A.01 · identity ──────────────────────────────────────────────────── -->
/* ═════════════════════════════════════════════════════════════════════════════
   B.01  TYPEFACES  ·  WOFF2 files in assets/fonts
   ══════════════════════════════════════════════════════════════════════════ */
/* ── portrait ────────────────────────────────────────────────────────────── */
```

- Every rule line **MUST** be exactly 80 characters. Part lines: `CODE  TITLE  ·  subtitle` (two spaces around items, title in capitals, subtitle optional). Head parts and groups: single spaces, lower case.
- C and D parts: B's three-line form opened by `<!-- ` and closed by ` -->`, code line indented 5; inside `<main>` indent 2 more and drop 2 rule characters. Script groups indent 2.
- Layer rules: A after the masthead, B before `<style>`, C after `<body>`, D after `</footer>`.

**Masthead**, after the charset and viewport metas; `{{fullName}}` in capitals, padded so the closing `│` stays in column 76:

```text
<!--
  ┌────────────────────────────────────────────────────────────────────────┐
  │                                                                        │
  │   {{fullName}}  ·  portfolio                                           │
  │   one page, no framework, no build step                                │
  │                                                                        │
  │   A   head ......... identity, crawling, social cards, schema          │
  │   B   styles ....... tokens, grid, figures, soft UI, screens           │
  │   C   page ......... bar, cover, orbit, glance, letter, F1–F{{N}}      │
  │   D   scripts ...... arrivals, three.js, figures, 3D, sheets           │
  │                                                                        │
  │   the full map of every layer is in README.md                          │
  │                                                                        │
  └────────────────────────────────────────────────────────────────────────┘
-->
```

A `README.md` **SHOULD** map each code to its line and groups; without one, drop that masthead line.

### 2.3 Part lists

**B** (subtitles free, e.g. `B.09  ORBIT  ·  the signature object holds the middle`):

```text
B.01–B.23: TYPEFACES · TOKENS · DOCUMENT · TYPE & MARKS · LAYOUT · TOP BAR · COVER · OPENING LINE
ORBIT · AT A GLANCE · LETTER · FEATURES · PHOTO SLOTS · FIGURE TRAYS · CONTROLS · 3D STAGES
BROWSER TAB · COMPACT FIGURES · NEXT ISSUE · CONTACT · FOOTER · MOTION & PRINT · SHARED FINISHING
```

**D**, in a load-bearing order:

| Part | Why here |
|---|---|
| D.01 ARRIVALS | `html.fly-ready` lands before the 608 KB vendor parse (§16) |
| D.02 three.js r149 | plain `<script src="assets/vendor/three.min.js"></script>`; D.04, D.06 need `THREE` |
| D.03 THEME & FOOTER | toggle, theme-color sync, year (§7) |
| D.04 SIGNATURE OBJECT | §15 |
| D.05 FIGURE KIT | custom dropdown, then the 2D figures using it (§12) |
| D.06 3D FIGURES | shared stage (§14) |
| D.07 PHOTO GUARD | cancels `contextmenu`, `dragstart` on cover and profile card |
| D.08 CONTROL DRAWERS | summaries read outputs the figures already set |
| D.09 PINNED DRAWER | needs the `.dock` from D.08 (§8) |
| D.10 COMPACT FIGURES | moves finished figure DOM into sheets (§13) |

### 2.4 CSS organisation

- CSS **MUST** be organised by component: a part holds its base rules, states, soft look, dark variants and responsive changes. Each `@media` block **MUST** directly follow the rule it adjusts; a part **MAY** end with breakpoint groups (`/* ── ≤ 640px ── */`).
- Every dark rule is written twice, side by side: `:root[data-theme="dark"]` and `@media (prefers-color-scheme: dark)` + `:root:not([data-theme="light"])` (§4).
- One selector per context, repeated only where a later rule must win (`.btn:hover` after `.btn:disabled`). One declaration per line, two-space indent, double quotes.
- **B.22 and B.23 come last.** B.23 (groups: rounded small labels · sand underlines · accent ink) gives the label voice (`--ui` 700, `0.06em`) and `--accent-ink` to selectors from many parts. Why: at equal or lower specificity it wins by order alone, and the lists live in one place. Print (B.22) overrides every component; in it `.fly-ready [data-fly]` **MUST** follow `.js [data-fly]` (§16).

### 2.5 Head (A.01–A.09)

Banners shortened; a `+` comment stands for tags written at that spot.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- masthead · A rule · A.01 identity -->
<title>{{fullName}} — {{role}}</title>
<meta name="description" content="{{summary}}">
<!-- + author, creator, publisher {{fullName}}; keywords {{keywords}} -->
<meta name="referrer" content="strict-origin-when-cross-origin">
<meta name="format-detection" content="telephone=no, email=no, address=no">
<link rel="canonical" href="{{siteUrl}}/">
<!-- + alternate hreflang en and x-default → {{siteUrl}}/; author → mailto:{{email}};
     image_src → {{siteUrl}}/og-card.jpg -->
<!-- A.02 crawling -->
<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1">
<!-- A.03 colour and display -->
<meta name="color-scheme" content="light dark">
<meta id="tcLight" name="theme-color" content="#e9e6de" media="(prefers-color-scheme: light)">
<meta id="tcDark" name="theme-color" content="#1c1b18" media="(prefers-color-scheme: dark)">
<!-- A.04 icons and installability -->
<link rel="icon" href="favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="apple-touch-icon.png" sizes="180x180">
<link rel="manifest" href="site.webmanifest">
<!-- + icon icon-192.png (192x192); mask-icon favicon.svg {{accent.light}}; (apple-)mobile-web-app-capable
     yes; apple-mobile-web-app-title, application-name {{shortName}} -->
<!-- A.05 Open Graph -->
<meta property="og:type" content="profile">
<meta property="og:title" content="{{fullName}} — {{role}}">
<meta property="og:description" content="{{pitch}}">
<meta property="og:image" content="{{siteUrl}}/og-card.jpg">
<!-- + og:site_name "{{fullName}} — Portfolio", og:url {{siteUrl}}/, og:locale en_US; og:image:secure_url,
     :type image/jpeg, :width 1200, :height 630, :alt {{ogAlt}}; profile:first_name, :last_name, :username -->
<!-- A.06 Twitter / X -->
<meta name="twitter:card" content="summary_large_image">
<!-- + twitter:title, :description, :image, :image:alt = the og values; :site, :creator @{{xHandle}} -->
<!-- A.07 structured data -->
<script type="application/ld+json">
{"@context": "https://schema.org", "@graph": [
 {"@type": "WebSite", "@id": "{{siteUrl}}/#website", "url": "{{siteUrl}}/", "name": "{{fullName}} — Portfolio",
  "description": "{{summary}}", "inLanguage": "en", "publisher": {"@id": "{{siteUrl}}/#person"}},
 {"@type": "ProfilePage", "@id": "{{siteUrl}}/#profilepage", "url": "{{siteUrl}}/", "name": "{{fullName}} — {{role}}",
  "isPartOf": {"@id": "{{siteUrl}}/#website"}, "about": {"@id": "{{siteUrl}}/#person"},
  "mainEntity": {"@id": "{{siteUrl}}/#person"}, "inLanguage": "en",
  "primaryImageOfPage": {"@type": "ImageObject", "url": "{{siteUrl}}/og-card.jpg", "width": 1200, "height": 630}},
 {"@type": "Person", "@id": "{{siteUrl}}/#person", "name": "{{fullName}}", "givenName": "{{givenName}}",
  "familyName": "{{familyName}}", "jobTitle": "{{role}}", "description": "{{summary}}", "url": "{{siteUrl}}/",
  "image": {"@type": "ImageObject", "@id": "{{siteUrl}}/#portrait", "url": "{{siteUrl}}/portrait.jpg",
   "contentUrl": "{{siteUrl}}/portrait.jpg", "width": 440, "height": 550, "creditText": "{{fullName}}"},
  "email": "mailto:{{email}}",
  "address": {"@type": "PostalAddress", "addressLocality": "{{city}}", "addressCountry": "{{countryCode}}"},
  "knowsLanguage": ["{{languages[i]}}"], "knowsAbout": ["{{topics[i]}}"], "sameAs": ["{{links[i].url}}"]}
]}
</script>
<!-- A.08 early fetches -->
<link rel="preload" href="assets/portrait.webp" as="image" type="image/webp" fetchpriority="high">
<link rel="preload" href="assets/fonts/{{face}}.woff2" as="font" type="font/woff2" crossorigin>
<!-- ↑ once per face: instrument-serif-400, instrument-serif-400-italic, jost-300, jost-400,
     jost-500, nunito-700, dm-mono-400 -->
<!-- A.09 script flag -->
<script>
(function (r) {
  r.classList.add("js");
  try {
    var t = localStorage.getItem("theme");
    if (t) r.setAttribute("data-theme", t);
  } catch (e) {}
})(document.documentElement);
</script>
<!-- B layer rule -->
<style>/* B.01 … B.23 */</style>
</head>
```

Head-only keys extend §3: `siteUrl` (no trailing slash), `summary` (one paragraph pointing to the figures, used verbatim three times), `shortName` (≤ 15 characters, else iOS truncates), `countryCode` (ISO alpha-2), `languages[]` (BCP 47); the rest are self-describing.

- **One card** (1200 × 630) for every social image; only `Person.image` uses the 440 × 550 portrait. The graph links WebSite → ProfilePage → Person by `@id` only.
- **A.03**: the `theme-color` pair is light and dark `--paper`. After a manual choice D.03 writes one colour into both metas and removes `media`, so the browser chrome follows the choice.
- **A.08**: the cover photo is a CSS background, invisible to the preload scanner; `fetchpriority="high"` starts this LCP image with the document. Only the seven first-screen faces are preloaded. `crossorigin` is **REQUIRED** on font preloads, even same-origin. Why: fonts are fetched in CORS mode; without it the preload is discarded and the font downloads twice.
- **A.09** is the last element before `<style>`, so `html.js` and the saved theme exist before the first style pass: no frame in the wrong theme, no arrival visible before it is hidden.

### 2.6 Script conventions

- **One concern, one IIFE**, each in its own classic `<script>`. Why: a failure in one never stops the others.
- ES5 style (`var`, `function`, two-space indent, double quotes, 100 columns), or a strict modern equivalent file-wide, never mixed. `type="module"` and `defer` **MUST NOT** be used. Why: both postpone execution and break the D order.
- Each opens with `var $ = function (id) { return document.getElementById(id); };` and **MUST** return silently if its host id is missing.
- Nothing global but the vendors' (`THREE`, the PDF global); scripts couple only through 2.8. Expandos (`el._fly`, `select._csel`) are allowed.
- `localStorage` and `setPointerCapture` sit in `try/catch`. Scroll/touch listeners are passive, except Ctrl/⌘-wheel zoom (§14). User strings pass `esc()` (`& < > "`) before `innerHTML`.
- Feature checks, never user-agent checks:

| Feature | Check | Fallback |
|---|---|---|
| WebGL | `typeof THREE`, then `new THREE.WebGLRenderer` in `try` | muted sentence in place of the figure; its buttons disabled |
| View Transitions | `document.startViewTransition` | instant swap |
| Popover | `typeof el.showPopover === "function"` | a plain fixed layer |
| `CloseWatcher` | `window.CloseWatcher` | Escape only |

### 2.7 Lazy library loader

A heavy library that only one action needs **MUST NOT** load with the page.

```js
var libs = {};
function lazy(src) {
  if (!libs[src])
    libs[src] = new Promise(function (ok, fail) {
      var s = document.createElement("script");
      s.src = src;
      s.onload = ok;
      s.onerror = function () { libs[src] = null; s.remove(); fail(); };  // retry later
      document.head.appendChild(s);
    });
  return libs[src];                       // one request for warm-up and click
}
["pointerenter", "focus"].forEach(function (t) {
  btn.addEventListener(t, function () { lazy(LIB).catch(function () {}); }, { once: true });
});
btn.addEventListener("click", function () {
  lazy(LIB).then(make, function () { window.print(); });
});
```

A warm-up on `pointerenter` (touch fires it too) or `focus` usually finishes before the click; `.catch` silences its failure. The fallback **MUST** keep the action meaningful: print (§17).

### 2.8 Contracts between layers

Layers couple only through these names. A new coupling **MUST** be added to this table.

| Name | Kind | Set by | Read by | Meaning |
|---|---|---|---|---|
| `html.js` | class | A.09 | B.22 | `[data-fly]` may start hidden |
| `html[data-theme]` | attribute | A.09, D.03 | dark rules; D.04, D.06 probes | `light`/`dark`; absent = OS |
| `localStorage.theme` | storage | D.03 | A.09, D.03 | saved choice |
| `#tcLight` `#tcDark` `#themeBtn` `#yr` | ids | A.03, C | D.03 | chrome colour; toggle (label follows OS too); year |
| `[data-fly]` `[data-fly-delay]` | attributes | C | D.01, B.22 | arrivals, never nested (§16) |
| `html.fly-ready` | class | D.01, always | B.22 | releases the hide |
| `html.sheet-on` | class | D.10 | B.17, B.18 | page scroll locked, pulses paused |
| `sheetchange` | `CustomEvent` on `document` | D.10 | D.06 | `detail.open`: simulate, do not render |
| `want` | `Event` on a `.tab` | D.06 | D.10 | open that `.tab` as a sheet, ≤ 900px |
| `--surf` `--hiMix` `--loMix` | CSS variables | B.02, containers | soft controls | surface to mould from (§4) |
| `.stage.tiny` `.zoomed` `canvas.grab` `.hot` | classes | D.06 | B.16 | host < 300px; zoom > 1.03; dragging; over a pickable |
| `.csel…` | markup | D.05 | B.15 | native `<select>` stays authoritative, gets `input`/`change` |
| `details.dock` | markup | D.08 (wraps `.ctrls` `.fields` `.sig-ctrls`) | B.15, D.09 | open ≥ 701px, closed ≤ 700px |
| `#sig…`, `#figN…` | ids | C | D.04–D.06 | signature object and figure hooks |

### 2.9 Progressive enhancement and network budget

| Condition | Result |
|---|---|
| No JS | all visible: native selects, inline controls, tabs, empty 3D wells; OS theme |
| D.01 fails | `.js [data-fly]` reveals itself at 2.5 s (CSS); other IIFEs run |
| No WebGL | a muted sentence in each well |
| Reduced motion | nothing hidden or moving; theme swaps instantly (§17) |

| Resource | When | Raw | gzip |
|---|---|---|---|
| `index.html` | navigation | ≈ 345 KB | ≈ 78 KB |
| `portrait.webp`; seven faces | preload | ≈ 201 KB; ≈ 102 KB | — |
| Nunito 600, Caveat 500 | first use | ≈ 32 KB | — |
| `three.min.js` r149 | end of body | 608 KB | ≈ 152 KB |
| PDF library | first intent | ≈ 364 KB | ≈ 115 KB |
| `og-card.jpg`, `portrait.jpg` | crawlers only | ≈ 202 KB | — |

First load ≈ 1.26 MB raw (0.53 MB gzipped), no third-party request; grain, marks and glyphs are inline SVG. A build **SHOULD** stay within about 10% of each line.

**Pitfalls.** The paper hex pair lives in the A.03 metas, D.03's `syncTheme` and the manifest; change all three together. Renumbering a code breaks every reference to it; append instead.

---

## 3 · Content model & voice

Every word, photo and number on a Folio page comes from one content object. This chapter defines each field, its limits, which part reads it, what to do when it is missing, and how the words sound. The limits are the lengths the layout is tuned for: if a text does not fit, cut words; **MUST NOT** shrink type or widen a column. Why: every display size in §5–§11 is a `clamp()` tuned to these lengths.

### 3.1 The object

```json
{
  "fullName": "", "nameParts": [{"text": "", "italic": false}],
  "role": "", "location": "", "coordinates": null, "workMode": null,
  "email": "", "links": [{"label": "", "url": ""}],
  "portrait": {"src": "assets/portrait.webp", "focal": "50% 40%", "alt": ""},
  "accent": {"source": "portrait", "light": "#……", "dark": "#……"},
  "pitch": "",
  "opening": {"line": "", "inkWord": "", "brushWord": ""},
  "orbit": {"lede": "", "keyPhrase": "", "aside": "", "figNote": "", "signatureObject": ""},
  "glance": {"kicker": "", "aside": "", "actions": {"mail": "", "link": "", "work": ""},
             "facts": [{"label": "", "value": "", "numeral": null}],
             "stack": [{"label": "", "items": [""], "now": false}]},
  "letter": {"kicker": "", "headline": "", "stampWord": "", "loopWord": "", "paragraphs": [""],
             "strongPhrase": "", "photoSlot": {"caption": "", "src": null, "alt": null}},
  "features": [{"no": "01", "aside": null, "field": "",
                "headline": [{"text": "", "italic": false, "ink": false, "brush": false}],
                "lede": "", "body": [""], "details": [{"label": "", "value": ""}],
                "figure": {"archetype": "A1", "title": "", "caption": "", "note": ""},
                "quote": {"text": "", "turn": ""}, "layout": "L1"}],
  "next": {"mode": "project", "kicker": "", "headline": "", "body": "",
           "built": {"note": "", "items": ""}, "button": "", "photoSlot": {"caption": ""}},
  "contact": {"lead": ""},
  "colophon": {"copyright": "", "note": ""}
}
```

Notation: `w` words, `c` characters with spaces, `?` optional (`null` selects the §3.7 variant).

### 3.2 Fields, limits and consumers

**Identity**

| Key | Limits and rules | Read by |
|---|---|---|
| `fullName` | 6–30 c, as printed | glance `h2`, footer, `<title>`, JSON-LD (§2, §9, §11) |
| `nameParts` | one entry per word, 2–4 words; 3+ words: exactly one `italic` (default the middle); 2 words: none or one. The serif sets ≈ 0.38 em per character, so ≤ 21 c keeps the name ≥ 28px on a 300px screen; at 22–30 c drop a middle name here (keep it in `fullName`) | cover `h1` + echo, bar wordmark (§7) |
| `role` | 2–4 w, ≤ 24 c, sentence case | cover `.lab`, glance role line |
| `location` | `City, Country`, ≤ 28 c; the cover uses the part before the comma | cover `.note`, glance role line |
| `coordinates?` | one decimal with hemisphere letters, `DD.D°N DD.D°W` | cover `.note`, after ` · ` |
| `workMode?` | 1–4 w, ≤ 24 c | glance role line |
| `email` | ≤ 35 c stays on one line; longer breaks at the `<wbr>` before `@` | glance action, contact `.mail` |
| `links` | 0–3; `label` = the site's name, 1–2 w; `url` absolute `https://` | `links[0]` glance action; contact `.links` |
| `portrait?` | landscape WebP ≥ 1500 × 900; subject off-centre, best in the right third; calm natural surroundings; `focal` = the face's `x% y%`; `alt` ≤ 100 c | `--me` (§7, §9); `alt` → glance band label |
| `accent` | `source` ∈ `portrait` · `chosen` · `default`; hex pair from §3.5 / §4 | tokens, marks, icons (§2, §4) |
| `pitch` | one sentence, 10–14 w, 60–85 c | glance `.gl-line`, `og:description` |

`focal` drives three positions: the cover ≤ 620px uses `x y`; the wide cover and the glance band keep `50%` for x and use y, set by eye (a face at the right third works at `72% 42%`, `50% 42%` and `50% 40%`).

**Opening and orbit**

| Key | Limits and rules | Read by |
|---|---|---|
| `opening.line` | 5–9 w, ≤ 40 c, one sentence; two near-equal lines | `.hl` (§7) |
| `opening.inkWord` | one word, the claim (usually the verb); not first, not last | `.ink` |
| `opening.brushWord` | the last word with its full stop: the hinge | `em.brush` |
| `orbit.lede` | 2 sentences, 30–45 w | `.stand` (§8) |
| `orbit.keyPhrase` | exact substring of `lede`, 2–4 w | `.stand b` |
| `orbit.aside` | ≤ 40 c, invites the reader to touch the object; two lines split after an em dash; lowercase, no full stop | `.o-hand .hand` |
| `orbit.figNote` | 2–4 sentences, 35–55 w: what the object shows and what its moving parts mean | `.sig-note` under `fig. 0 — {object name}` |
| `orbit.signatureObject` | an archetype from §15 | the stage (§15) |

The contents stops are derived: one per feature, titled with the plain text of its headline (20–35 c; add up to 3 words only if the headline means nothing out of context). The contents label is 2–4 w, e.g. "In this issue".

**Glance** (§9)

| Key | Limits and rules |
|---|---|
| `kicker` | 2–3 w, ≤ 14 c, names the card |
| `aside` | 25–32 c, to a reader in a hurry; hidden ≤ 380px, so it carries no information |
| `actions` | each ≤ 2 w, ≤ 13 c (three equal cells on phones): `mail` a verb, `link` = `links[0].label`, `work` a verb phrase; CSS appends " ↓", never type it |
| `facts` | exactly 6 (4 or 2 only by §3.7); `label` 1–2 w, ≤ 14 c, sentence case; `value` ≤ 32 c |
| `facts[i].numeral?` | ≤ 4 c, the number the value starts with (`value` holds the rest); one fact at most, normally the first |
| `stack` | 3 groups (1–3 allowed); `label` ≤ 17 c; 3–9 `items` per group, 1–4 w and ≤ 26 c each, ≤ 24 in all; `now: true` on the last group only, for what is in use or being learned now |

**Letter** (§9; the section is optional)

| Key | Limits and rules |
|---|---|
| `kicker` | 3–5 w, ≤ 24 c |
| `headline` | 6–10 w, ≤ 56 c, one statement, no italic; `&nbsp;` joins the last two words |
| `stampWord`, `loopWord` | two different words of `headline`, neither of the last two: `.ink.wide` and `.loop` |
| `paragraphs` | 2–5 (4 balances two columns), 30–60 w each. The first starts with a letter. Why: `::first-letter` pulls a leading quote mark or digit into the drop cap |
| `strongPhrase` | 2–8 w, exact substring of one paragraph; sand underline |
| `photoSlot?` | `caption` lowercase, ≤ 28 c; `src`, `alt` once a photo exists |

**Features** (`features[i]`, 3–7 entries, §10)

| Key | Limits and rules |
|---|---|
| `no` | two digits = position. Shown `No. 0N`; bare `0N` beside an aside; L5 shows the giant numeral instead |
| `aside?` | 3–8 w, lowercase, no full stop; on the biggest figure, the feature without one, the last one |
| `field` | 1–2 w, ≤ 18 c |
| `headline` | 2–6 w, ≤ 32 c, segments in reading order: ≤ 1 `italic` (the hinge, ≤ 3 w), ≤ 1 `ink`, never the same segment; `brush` always `false`. L3 sets its italic segment, which must be last, as the second line |
| `lede` | 1–2 sentences, 12–30 w |
| `body` | 2–3 paragraphs of 2–4 sentences, 18–65 w each; 50–160 w in all (L5: 40–70) |
| `details` | 3–6 rows; `label` 1–2 w, ≤ 11 c (it must fit the 84px column ≤ 460px); `value` 2–7 w, ≤ 48 c, compounds in `.nw`. L5 values ≤ 4 w; L7 values are claims ≤ 45 c |
| `figure?` | `null` = A0. `archetype` A1–A5 (§12); `title` 3–5 w, ≤ 24 c; `caption` one paragraph of 30–60 w (75 max): the two sides, what to do, what is simulated; `note` ≤ 6 w |
| `quote?` | `text` one sentence, 4–12 w, a principle the person stands behind; `turn` 1–3 w, exact substring; shown only by templates with a quote row |
| `layout` | L1–L7 (§10, `B-build-rules.md` §9.1) |

DOM order is `f-top` (`no`, `aside`, `field`) → `h2` → `.lede` → `.f-text` → `.side` → `.insert` → `.quote`. The headline's plain text also names the section (`aria-labelledby`).

**Next, contact, colophon** (§11)

| Key | Limits and rules |
|---|---|
| `next.mode` | `project` (default) or `open` (§3.7) |
| `next.kicker` | 2–4 w, ≤ 14 c |
| `next.headline` | 2 words, ≤ 14 c; the second italic, holding the full stop |
| `next.body` | 2–3 sentences, ≤ 45 w |
| `next.built` | `note` lowercase ≤ 4 w; `items` one sentence ≤ 150 c |
| `next.button` | 1–3 w: the state on a disabled button (`project`), a verb on a mailto link (`open`) |
| `contact.lead` | one sentence, 10–18 w, ≤ 100 c: a greeting and what you are open to |
| `colophon.copyright` | `© {year} {{fullName}}.`; the year is in the HTML, refreshed by script |
| `colophon.note` | one honesty line, ≤ 120 c, on the figures' data |

The colophon's second line is fixed: "Type in Instrument Serif and Jost, notes in DM Mono, controls in Nunito, handwriting in Caveat. Drawings in three.js."

### 3.3 Headline grammar

In examples, *italic* is the hinge and **bold** the ink word (rendered as the `.ink` stamp, never as bold).

| Line | Words | Italic | Ink | Other mark |
|---|---|---|---|---|
| Cover name | 2–4 | the middle word, or none | — | — |
| Opening line | 5–9 | the last word | one | brush under the italic |
| Feature headline | 2–6 | ≤ 1 hinge | on ≤ 2 headlines | — |
| Letter headline | 6–10 | none | one (`.wide`) | pen loop |
| Glance name | as printed | none | — | brush under the family name |
| Next headline | 2 | the second word | — | — |
| Pull quote | 4–12 | the quote; `turn` upright (L4, L5 invert this) | `turn`, once per page | sand hanging mark |

The hinge is the word that turns the sentence: a preposition, a qualifier, the second half. Never the noun a reader expects to be stressed, never the first word unless it is the hinge. Why: posture on the obvious word reads as decoration; posture on the turn reads as a voice. The page carries about five stamped words.

### 3.4 Counts

| Count | Rule |
|---|---|
| Features | 3–7. N sets the template sequence (`B-build-rules.md` §9.1), the contents stops (§8 lengthens gaps for fewer), codes `C.07…C.(6+N)` (§2) and the pause pairs (§10) |
| Figures | ≥ 1 per page, ≤ 1 per feature, ≤ 2 in 3D; numbered in order of appearance; the signature object is fig. 0 |
| Pull quotes | 0–4; never two neighbouring features both without figure and quote |
| Spoken asides | 2–3 (1–2 when N ≤ 4) |
| Photo slots | 0–3: letter, the L6 feature, next card |
| Bar links | 5 (4 without a letter) |

### 3.5 The accent from the portrait

Method only; §4 turns the sample into tokens and checks them.

1. Mask out the subject (skin, hair, clothes, glasses) and blown-out sky.
2. In what remains, find the one natural colour the eye notices after the face: foliage, moss, water, painted wood, brick, tinted stone.
3. Average five 20 × 20 px patches across it, mid-tones only (OKLCH L 0.3–0.7); take the median hue.
4. Hand hue and sample to §4, which sets lightness and chroma for `--accent`, `--accent-ink` and dark `--accent` and runs the contrast checks. Store the pair in `accent`, `source: "portrait"`.
5. A colour the person names replaces steps 1–3 (`"chosen"`) and still goes through §4.
6. Use §4's default pair (`"default"`) with no portrait, when the only colour is the subject, when the surroundings are grey, white or sky, or when the sample fails §4.

Sand and every other token stay as Appendix A gives them. Why: the one chroma in the interface reads as native to the photo; a second sampled colour would compete with it.

### 3.6 Voice

- **First person, plain, short.** Present tense for what the person does, past for what they did. Sentences average 12–18 words; none passes 28.
- **Concrete.** Nouns and numbers the person gave. A sentence that needs a missing number is rewritten without it, never estimated.
- **No buzzwords**: passionate, innovative, cutting-edge, seamless, robust, leverage, synergy, world-class, empower, journey. No exclamation marks, emoji or rhetorical questions.
- **Spoken asides.** The feature top line may carry a half-sentence the person would say out loud: the editor talking in the margin.
- **Frank notes.** Mono notes say what is simulated and what is real; generated files say they hold sample data made in the browser.
- **Verbs on buttons**, plus an object when needed. Never "Click here", "Learn more" or "Submit".
- **Case.** HTML text is sentence case; uppercase comes only from CSS (`.lab`, interface labels). Why: typed capitals are spelled out by some screen readers. Mono notes, captions, asides and group labels are lowercase.
- **Punctuation.** Curly quotes; ` — ` in notes; ` · ` as the separator in role lines, summaries and hints; `&nbsp;` between the last two words of a display line. One spelling variant throughout.

### 3.7 When content is missing

Never lorem ipsum, a stock photo, an avatar, initials in a circle, invented work, filler facts or an empty "under construction" section.

| Missing | Variant |
|---|---|
| Portrait | cover without photo (§7), glance without band (§9), default accent |
| Coordinates / work mode | the city alone / the role line drops that segment and its dot |
| Links | e-mail only; the glance keeps `mail` and `work` |
| Result numbers | state the result without one |
| Work to touch | a figure models the *idea* of one piece of work with invented, labelled data (A1 or A3 fits almost anything); the rest use A0 (L5, L7) |
| Fewer than three pieces | `B-build-rules.md` §3.3 |
| Facts | 4 or 2, never padded |
| Quotes | fewer; the template drops the row (§10) |
| Letter | omit it and its bar link |
| Next project | `next.mode: "open"`, or omit the card |
| A photo for a slot | an honest placeholder: hatched print captioned `{what it will show} — photo to come`; drop the slot if nothing will fill it |

**`open` next card.** Same anatomy. `kicker` states availability ("Taking on work"); `headline` two words ("Your *move.*"); `body` the kind of work, from when, in what arrangement; `built.note` "good fit", `built.items` the kinds of work; `button` a verb ("Write to me") on `<a class="btn" href="mailto:{{email}}">`, using the card's enabled-button rule (§11).

### 3.8 Patterns

Examples are invented: a bike-share network, tide tables for small boats, a museum audio guide.

| Text | Pattern | Example |
|---|---|---|
| Opening line | `{Subject} that {**verb**} {object} *{last.}*` | Maps that **keep** pace with the *tide.* |
| Feature headline | `{Noun phrase} *{hinge}* {rest}` | Bikes *where* people wake up · Tide tables *for* small boats |
| Spoken aside | `the one {that / where / without} {…}` | the one that ran on a ferry |
| Lede | one claim, one number | A plan for 300 docks that moves bikes before the morning rush, not after it. |
| Pull quote | italic, `turn` upright | A good guide knows when to *stop* talking. |
| Glance aside | to a hurried reader | in case you are in a hurry |

### 3.9 Figure microcopy

| Element | Shape | Example |
|---|---|---|
| Header, left | `Fig. {n} — try it` on the first figure, `Fig. {n}` after | Fig. 2 |
| Header note | ≤ 6 lowercase words: what is invented, what is real | made-up docks, real routing rule |
| Title | `From {input} to {output}`, `{A} against {B}`, or three verbs | Rush hour, two ways |
| Readout label | 1–2 w; values raw integers, time `{n}s` | Moved · Empty docks · Time |
| Drawer summary | current values joined by ` · ` | 300 bikes · 4 crews · 5% no-shows |
| Object status | `{Measure}: starting.`, then `{Measure}: {n} of {m} {units}. {State}.`, ≤ 60 c; state words by threshold (§15) | Bikes docked: 7 of 12. Filling up. |
| Status line | one sentence per action: the consequence, not the click | Dock 4 is closed. Its bikes go to the next two. |
| Verdict | claim, counter-claim, moral (≤ 8 w); computed figures in `<b>` | Greedy took 40 min but left **18%** of docks empty. Planned took 55 min and left **2%**. The slow plan is the one riders feel. |
| Empty state | italic serif: `{nothing yet}. {do this} to see {that}.` | No trips yet. Pick a dock to start one. |

The verdict's claim **MUST** hold over the whole input range, or the template branches. Why: a fixed moral beside numbers that contradict it undoes the figure's honesty.

---

## 4 · Colour, surface & light

One paper, one ink, one accent, one sand, re-declared for dark; light on controls is computed from the surface under them (4.6). Selection, focus ring and scrollbars are in §6.

### 4.1 Colour tokens

| Token | Light | Dark | Role |
|---|---|---|---|
| `--paper` | `#e9e6de` | `#1c1b18` | page under the grain; `theme-color`; text on ink |
| `--paper2` | `#e0ddd3` | `#25231f` | plates: trays, cards, sheets |
| `--ink` | `#151412` | `#eeebe3` | text, ink rules, "on" fills, inverse card |
| `--muted` | `#54514a` | `#a29e95` | secondary text, labels, captions |
| `--hair` | `rgba(21, 20, 18, 0.14)` | `rgba(238, 235, 227, 0.15)` | hairlines, dividers, bar bottom |
| `--rule` | `rgba(21, 20, 18, 0.32)` | `rgba(238, 235, 227, 0.34)` | field rules, link underlines |
| `--brand` | `#546634` | not redeclared | the light accent, constant in both themes |
| `--accent` | `var(--brand)` | `#a9b77c` | marks, small fills, focus, big numerals |
| `--accent-ink` | `#435425` | `#a9b77c` | accent-coloured text under ≈ 20px |
| `--on-accent` | `#f6f4ec` | `#161a0e` | text on an accent fill |
| `--sand` | `#9a7650` | `#c9a77c` | hand marks (4.4.4) |
| `--sand-ink` | `#7a5b3b` | `#c9a77c` | sand-coloured small text |
| `--grain` | light recipe | dark recipe | paper tooth (4.5) |
| `--brush` `--pencil` `--loop` | `#9a7650` / `#546634` baked in | `#c9a77c` / `#a9b77c` baked in | hand-mark SVGs (§5) |
| `--surf` | `var(--paper)` | follows `--paper` | light input: surface under a control |
| `--hiMix` | `#fff 55%` | `#fff 6%` | light input: highlight |
| `--loMix` | `#5a4630 16%` | `#000 45%` | light input: shade (warm in light) |

No surface or text is `#fff` or `#000`; those literals live only in the mixes and the fixed colours of 4.7.

### 4.2 Three blocks, one switch

The theme is `data-theme` on `<html>`: absent (follow the OS), `"light"` or `"dark"`, set in the head script (§7). B.02 writes the tokens in exactly these blocks, in this order (every line in Appendix A):

```css
html {
  color-scheme: light;
}
:root {
  /* light tokens (4.1), families (§5), --gut, --cg (§6), --max: 1560px */
}
:root[data-theme="dark"] {
  color-scheme: dark;
  /* each token with a dark value, in :root order */
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* the manual dark block's declarations, verbatim */
  }
}
@media (min-width: 1920px) {
  :root {
    --max: 1680px;
  }
}
```

- Every dark rule **MUST** exist twice, under `:root[data-theme="dark"]` and under `:root:not([data-theme="light"])` in the media query, component rules included. Why: the manual choice must beat the OS both ways.
- `color-scheme: light` on `html` (0,0,1) loses to both dark blocks; native controls follow the chosen theme.
- The `theme-color` metas (`#e9e6de` / `#1c1b18`) and the manifest (`#e9e6de`) are hard copies of `--paper`.
- No colour transitions on a theme change (§7). Why: hundreds of elements tweening colour stutter.

### 4.3 Resolved colours and contrast

Flattened on paper, light / dark: `--hair` ≈ `#cbc9c1` / `#3c3a36`; `--rule` ≈ `#a5a39d` / `#63625d`; selection `color-mix(in srgb, var(--sand) 42%, transparent)` ≈ `#c8b7a2` / `#655642`.

| Pair (WCAG 2.x) | Light | Dark | Allowed for |
|---|---|---|---|
| ink on paper / paper2 | 14.76 / 13.55 | 14.46 / 13.17 | all text |
| muted on paper / paper2 | 6.35 / 5.83 | 6.45 / 5.87 | secondary text |
| accent on paper / paper2 | 5.06 / 4.64 | 8.00 / 7.28 | marks, rings, numerals ≥ 24px |
| accent-ink on paper / paper2 | 6.65 / 6.10 | 8.00 / 7.28 | small accent text |
| sand on paper / paper2 | 3.31 / 3.04 | 7.63 / 6.95 | marks only, never text |
| sand-ink on paper / paper2 | 4.97 / 4.56 | 7.63 / 6.95 | small sand text |
| on-accent on accent | 5.73 | 8.21 | text on accent fills |
| paper on ink ("on" fill) | 14.76 | 14.46 | text on ink fills |

Why two tones per hue: light `--accent` and `--sand` fail as 11–13px labels on grain.

### 4.4 Accent and sand

#### 4.4.1 Jobs

- `--accent`: marks and small fills; it **MUST NOT** colour text under ≈ 20px.
- `--accent-ink`: accent text under ≈ 20px, set only by **one** late rule in shared finishing (B.23). Why: one place to audit contrast.
- `--brand`: the light accent in both themes; the tile-colour and `mask-icon` metas and `favicon.svg`'s light rule use it.
- Sand belongs to the paper stock and **MUST NOT** change with the accent.

#### 4.4.2 Deriving the accent

Only the accent changes per person (`{{accent.light}}`, `{{accent.dark}}`). It is measured from `{{portrait.src}}`, never guessed; with no portrait or no way to read pixels, use the default pair `#546634` / `#a9b77c`.

```text
SAMPLE   once, offline
  downscale the portrait to 64 px wide; convert pixels to OKLCH (L 0..1, C, h°)
  keep 0.30 ≤ L ≤ 0.80, C ≥ 0.03, 105° ≤ h ≤ 285°     coloured mid-tones, olive to blue-violet
  fewer than 2% of pixels left → default pair; stop
  bin by hue (10° bins), weight = C; sample = median L, C, h of the heaviest bin
LIGHT    h = sample.h
  brand      = oklch(0.48, clamp(sample.C, 0.05, 0.09), h)
  accent-ink = oklch(L(brand) − 0.06, 0.95 × C(brand), h)
  on-accent  = #f6f4ec                                fixed
DARK
  accent     = oklch(0.75, C(brand) + 0.004, h − 6)
  accent-ink = accent
  on-accent  = oklch(0.21, 0.023, h)
GAMUT    outside sRGB → lower C by 0.005 until inside
CHECKS   move L by 0.01 (never raise C) until every line passes
  brand on #e9e6de ≥ 4.5            brand on #e0ddd3 ≥ 3.0
  accent-ink on #e0ddd3 ≥ 6.0       #f6f4ec on brand ≥ 4.5
  dark accent on #1c1b18 ≥ 7.0      dark accent on #25231f ≥ 4.5
  dark on-accent on dark accent ≥ 7.0
  dark accent on #151412 ≥ 4.5      accent-ink on #eeebe3 ≥ 4.5     inverse card, 4.6.4
WRITE    lower-case hex
```

Defaults in OKLCH: `#546634` (0.483 0.077 125.4), `#435425` (0.420 0.073 125.9), `#a9b77c` (0.754 0.081 119.2), `#161a0e` (0.209 0.023 123.4). The method's constants are fitted to the default pair. Why: L ≈ 0.48 prints like ink; C ≤ 0.09 keeps colour a rumour; warm hues collide with sand and skin. A preferred colour runs from LIGHT with its own hue; outside 105°–285° the plan offers the default pair.

Write the hex everywhere the accent is baked: `--brand`, `--accent-ink` (`:root`); `--accent`, `--accent-ink`, `--on-accent` (both dark blocks); the `%23` stroke of `--pencil` and `--loop` (all three blocks); the tile-colour and `mask-icon` metas; `favicon.svg` (both rules) and its PNG icons; the inverse-card swap (4.6.4); the 3D colour-probe fallback (§14).

#### 4.4.3 Where green appears (exhaustive)

| Kind | Instances |
|---|---|
| Marks | theme knob (12px); slider dot (8px); drawer glyph; the letter's pen loop; pencil underline on hover/focus (bar links, contents titles, e-mail); contact-link underline, e-mail domain on hover; the aside's arrow |
| Numerals | the interlude's giant numeral; a glance fact's leading numeral; rationale-list numerals |
| Focus | 1.5px outline; 1–2px outlines (drawer summary, combobox, launcher, row keys); 2px field ring; gapped thumb ring |
| Figures | groove bar's second segment; option tick; 1.5px ring on a selected item; 2px bar on a followed row; current tracker step (26px disc, pulse, phone connectors); launcher cursor ring; a row count |
| Hover | primary button face; launcher "Open" pill; sheet close key; enabled button text on the inverse card |
| Other | cover-name selection; 3D live channels, beads, trails, glow cores, halos, "on" labels (§14, §15) |
| `--accent-ink` text | hand aside; field errors; busy toggle; tab-mock small heads; button hover text; selected option; first feature's number; contents number on hover; current step; selected item; last-stage status pills; verdict emphasis; glance link's ↓; zoom keys on hover; row-card header labels |

Green **MUST NOT** be an "on" fill, a resting button, link or toggle, a headline, body text, a fill over 26px, a border, rule, bar, print or card surface, or a glow on a control. It covers < 0.1% of the page; more than about a dozen green marks in a 1440 × 900 view is wrong.

#### 4.4.4 Where sand appears

Brush under the opening's italic word and the glance surname; 1.5px emphasis underline (offset 0.22em) in the letter and rationale lists; 4px crescent on "now" pills; "now" label and glance dot separators (`--sand-ink`); each pull quote's hanging opening mark; print tape, `color-mix(in srgb, var(--sand) 34%, rgba(255, 255, 255, 0.5))` at opacity 0.8; `::selection`. Never a surface or a state.

### 4.5 Paper grain

Two stacked SVG noise tiles; the first listed paints on top.

```css
/* :root (light) */
--grain:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3' seed='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0.24 0 0 0 0 0.2 0 0 0 0 0.14 0.3 0 0 0 -0.105'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E"),
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='173' height='173'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' seed='19' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 1 0 0 0 0 0.99 0 0 0 0 0.96 0 0.75 0 0 -0.22'/%3E%3C/filter%3E%3Crect width='173' height='173' filter='url(%23n)'/%3E%3C/svg%3E");
/* both dark blocks */
--grain:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3' seed='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0.95 0 0 0 0 0.92 0 0 0 0 0.85 0.19 0 0 0 -0.072'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E"),
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='173' height='173'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' seed='19' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.85 0 0 -0.26'/%3E%3C/filter%3E%3Crect width='173' height='173' filter='url(%23n)'/%3E%3C/svg%3E");
```

| Layer | Tile | Octaves / seed | Constant RGB | Alpha | Reads as |
|---|---|---|---|---|---|
| light top | 200 | 3 / 2 | 0.24, 0.20, 0.14 | `0.3·R − 0.105` | sparse warm specks |
| light bottom | 173 | 2 / 19 | 1, 0.99, 0.96 | `0.75·G − 0.22` | denser light flecks |
| dark top | 200 | 3 / 2 | 0.95, 0.92, 0.85 | `0.19·R − 0.072` | faint light specks |
| dark bottom | 173 | 2 / 19 | 0, 0, 0 | `0.85·G − 0.26` | dark specks |

- Constant colour, noise-driven alpha, negative offset: only noise peaks show. One layer darkens, one lightens, so the page averages to `--paper`.
- **Why 200 and 173:** 173 is prime, so the tiles realign only every 34,600px; with different seeds and octaves no repeat shows. `stitchTiles='stitch'` makes tiles seamless; `x='0' y='0'` drops the filter margin.
- Dark specks stay warm with lower slopes (light-on-dark noise reads stronger). Grain goes on `body` (`background-color: var(--paper); background-image: var(--grain);`) and the bar only; print sets `body { background: none; }`.

### 4.6 The soft-UI light model

#### 4.6.1 The kit, verbatim (B.02)

```css
/* ── per-element light for soft controls ─────────────────────────────────── */
* {
  --hi: color-mix(in srgb, var(--surf), var(--hiMix));
  --lo: color-mix(in srgb, var(--surf), var(--loMix));
  --raise: 4px 4px 10px var(--lo), -4px -4px 10px var(--hi);
  --raise-s: 2px 2px 5px var(--lo), -2px -2px 5px var(--hi);
  --sink: inset 3px 3px 6px var(--lo), inset -3px -3px 6px var(--hi);
  --sink-s: inset 1.5px 1.5px 4px var(--lo), inset -1.5px -1.5px 4px var(--hi);
  --face: var(--surf);
  --gface: var(--ink);
  --on-gface: var(--paper);
}
```

#### 4.6.2 How it composes

- Inputs `--surf`, `--hiMix` (colour + percentage in one token), `--loMix` live on `:root` and containers and inherit.
- **Why `*`:** a custom property containing `var()` resolves where declared and inherits finished; on `:root`, `--hi` would freeze to page paper.
- One light, top-left. `--face` = `--surf`: controls are moulded, not placed. "On" is an **ink fill**, never green, never a glow.

| Surface | Inputs | `--hi` | `--lo` |
|---|---|---|---|
| paper `#e9e6de` | `#fff 55%`, `#5a4630 16%` | ≈ `#f5f4f0` | ≈ `#d2ccc2` |
| paper2 `#e0ddd3` | same | ≈ `#f1f0eb` | ≈ `#cbc5b9` |
| dark paper `#1c1b18` | `#fff 6%`, `#000 45%` | ≈ `#2a2926` | ≈ `#0f0f0d` |
| dark paper2 `#25231f` | same | ≈ `#32302c` | ≈ `#141311` |
| inverse card `#151412` | `#fff 9%`, `#000 55%` | ≈ `#2a2927` | ≈ `#090908` |
| inverse card, dark `#eeebe3` | `#fff 70%`, `#000 15%` | ≈ `#faf9f7` | ≈ `#cac8c1` |

#### 4.6.3 Override inputs, never outputs

A container **MUST** change only `--surf`, `--hiMix`, `--loMix`, never `--hi`, `--lo`, `--raise…`, `--sink…`, `--face`, `--gface`, `--on-gface`: children re-declare those from `*`, so the override is silently lost. Set an output only on the control itself. Never hand-write a shadow colour.

#### 4.6.4 Controls on a new surface

A plate sets its background and `--surf` to the same token: `.insert { background: var(--paper2); --surf: var(--paper2); }`. The inverse card (`.next`, §11) is ink in light and cream in dark, so it re-tunes the mixes in both themes:

```css
.next {
  background: var(--ink);
  color: var(--paper);
  --surf: var(--ink);
  --hiMix: #fff 9%;
  --loMix: #000 55%;
  --accent: #a9b77c;
  --accent-ink: #a9b77c;
}
:root[data-theme="dark"] .next {
  --hiMix: #fff 70%;
  --loMix: #000 15%;
  --accent: #435425;
  --accent-ink: #435425;
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) .next {
    --hiMix: #fff 70%;
    --loMix: #000 15%;
    --accent: #435425;
    --accent-ink: #435425;
  }
}
```

- The accent lines swap in the other theme's tone (light card: dark `--accent`; dark card: light `--accent-ink`). Why: the page accent is 2.92:1 on ink and 1.81:1 on cream; the swap gives 8.55:1 and 6.96:1 for focus and hover text.
- New surfaces: dark → small white %, large black %; light → the reverse; dark values twice.
- Card text: `color-mix(in srgb, var(--paper) N%, transparent)`, N = 84 paragraph, 78 mono, 72 eyebrow, 68 caption, 60 disabled, 50 disabled hover; never `--muted`. No `.btn.solid` here (ink on ink).

#### 4.6.5 State grammar

| State | Pills (toggles, segments, theme pill) | Push buttons `.btn` | Fields (input, select, combobox, slider track) |
|---|---|---|---|
| Rest | `--raise-s` on `--face` | `--raise` on `--face` | `--sink-s` on `--surf` |
| Hover | `--raise`, text `--ink` (toggles, theme pill) | shadow kept, text `--accent-ink`; primary face `--accent`, text `--on-accent` | combobox `--sink` |
| Pressed | `--sink-s` | face `--surf`, `--sink`; primary `inset 2px 2px 6px rgba(0, 0, 0, 0.28)` | — |
| On | `--gface` face, `--on-gface` text; switch toggle: ink track, paper knob | primary is always `--gface` / `--on-gface` | option: weight 700, `--accent-ink`, tick |
| Keyboard focus | accent outline (§6) | same | `var(--sink-s), 0 0 0 2px var(--accent)`; thumb ring |
| Mouse focus | none | none | `--sink-s` only |
| Disabled | — | face `--surf`, `--raise-s`, `--muted` text, opacity 0.8 (inverse card: 1, paper 60%) | — |

- Disabled is lower relief, not faded: moulded edge, legible text, never a flat grey ghost.
- Hover changes no background except the primary's. Backgrounds snap; `box-shadow` and `color` transition `0.2s` (§16).
- Drawers, zoom capsule: `--raise-s`; listbox, tab frame, statement card: `--raise`; 3D well, grooves: `--sink-s`.

### 4.7 Surfaces

| Surface | Fill | Grain | Shadow | Note |
|---|---|---|---|---|
| Page | `--paper` | yes | none | |
| Bar | `color-mix(in srgb, var(--paper) 99%, transparent)` + `--grain` | yes | none; `--hair` below | 99%: a fully opaque sticky layer makes Chrome cull the pinned WebGL canvas |
| Plate (tray, card, sheet) | `--paper2`, `--surf: var(--paper2)` | no | none, no border | |
| Well (3D stage) | `--paper` in a plate | no | `--sink-s` | a window back to the page |
| Inverse card | `--ink` | no | none | always the opposite theme |
| Print | `#f7f5ef`; dark `#2e2c27` | no | print shadow | fixed hex: always the inverse card's opposite |
| Photo | `--me` | no | none | |

Fixed colours: print hatch `#ebe7de` / `#25231f` with lines `rgba(40, 34, 26, 0.09)` / `rgba(255, 250, 240, 0.07)`; print caption `#5a554c` / `#b9b3a7`; cover ground `#2b2d29`, shades `rgba(14, 15, 13, …)`, on-photo text `rgba(243, 241, 234, 0.88)` (§7); sheet scrim `rgba(12, 11, 9, 0.58)`, grab bar `rgba(246, 244, 236, 0.7)` (§13). Lines: ink rule `1px solid var(--ink)`, `--rule`, `--hair`; no double rules.

### 4.8 Radius families

| Family | Radius | Used for |
|---|---|---|
| Pill | `999px` | fields, combobox, segments, toggles and switch, theme pill and track, stack pills, zoom capsule, URL bar, "Open" |
| Circle | `50%` | slider thumb, zoom keys, round tab keys, tracker discs, sheet close key |
| Soft rectangle | `16px` (`--r`) | push buttons (never pills), drawers |
| Small | `5`–`15px` | grab bar 5; groove, scrollbar thumb 8; slider track 10; options, readout cards 12; list and tracker rows 14; launcher icon 15 |
| Well | `20px`, `22px` | row cards (20); 3D well and canvas, listbox, launcher row, cover ≤ 620px, left-bleed tray ≤ 760px (22) |
| Plate | `26px`, `28px` | cover, browser-tab frame, statement card (26); trays, glance card (28; 24 ≤ 760px) |
| Big plate | `32px` | inverse card (24 ≤ 760px) |
| None | `0` | prints, tape, rules, bar |

A plate bleeding off an edge squares that side's corners and adds `--gut` to that side's padding (§6).

### 4.9 Shadows (complete, verbatim)

| Where | `box-shadow` |
|---|---|
| `--raise` | `4px 4px 10px var(--lo), -4px -4px 10px var(--hi)` |
| `--raise-s`; written out on round tab keys, sheet close key, current-step disc | `2px 2px 5px var(--lo), -2px -2px 5px var(--hi)` |
| `--sink` | `inset 3px 3px 6px var(--lo), inset -3px -3px 6px var(--hi)` |
| `--sink-s` | `inset 1.5px 1.5px 4px var(--lo), inset -1.5px -1.5px 4px var(--hi)` |
| slider thumb | `4px 4px 9px var(--lo), -4px -4px 9px var(--hi)` |
| zoom keys, completed tracker disc | `3px 3px 7px var(--lo), -3px -3px 7px var(--hi)` |
| focused field, open combobox | `var(--sink-s), 0 0 0 2px var(--accent)` |
| keyboard-focused thumb | `0 0 0 2px var(--surf), 0 0 0 4px var(--accent)` |
| primary pressed | `inset 2px 2px 6px rgba(0, 0, 0, 0.28)` |
| switch-toggle track, on | `inset 1px 1px 3px rgba(0, 0, 0, 0.25)` |
| selected list item | `var(--raise-s, none), inset 0 0 0 1.5px var(--accent)` |
| followed row | `inset 2px 0 0 var(--accent)` |
| current-step pulse (keyframe 50%, `1.4s ease-in-out infinite`, off under reduced motion) | `0 0 0 5px color-mix(in srgb, var(--accent) 25%, transparent)` |
| print, light | `0 1px 1px rgba(30, 25, 18, 0.08), 0 14px 28px -18px rgba(30, 25, 18, 0.45)` |
| print, dark | `0 14px 28px -18px rgba(0, 0, 0, 0.8)` |
| bottom-sheet panel | `0 -8px 40px -14px rgba(0, 0, 0, 0.5)` |

The only text shadow is the stamp, `max(1.1px, 0.022em) 0 0 currentColor` (§5). Never shadowed: bar, plates, cover, inverse card, photos, rules, 3D drawings; focus and hover never glow.

### 4.10 Layering

| Layer | `z-index` | Why |
|---|---|---|
| Bottom sheet `.sheet` | `1000` + `popover="manual"` (top layer) | over everything (§13) |
| Listbox `.csel-list` | `40` | over the bar |
| Bar | `20` | sticky over page content |
| `.orbit-pin` over `.orbit-flow` | `3` over `2` | object in front of the lanes (§8) |
| Drawer popover (signature figure) | `3` | over the stage (§15) |
| Print across the inverse card's edge | `2` | over the card |
| Pull quote over a tray | `1` | overlaps the tray edge (§10) |
| `.cv-hero::before` (photo, shades) | `-1` in `.cv-hero { isolation: isolate; }` | behind the cover text |
| "Now" pill face `::before` | `-1` in `isolation: isolate` | leaves a 4px sand crescent |
| `::view-transition-new(root)` over `-old` | `2` over `1` | reveal grows over the old theme (§7) |

Everything else is `auto`.

### 4.11 Dark mode is re-inked, not inverted

Beyond values: plates step *lighter* than the page (deeper in light); grain swaps recipe, not polarity; controls read by their lower-right shade; "on" fills stay ink, now cream; the inverse card stays inverse with its own mixes; prints turn charcoal; brush drops to .36 / .2, pencil and loop are re-encoded; `--accent-ink` merges into `--accent`; `color-scheme` and `theme-color` follow; the theme knob moves from 10px to 26px. Unchanged: photos, `--brand`, radii, geometry. 3D scenes re-read tokens (§14).

---

## 5 · Typography & marks

Five faces with one job each, a hand-set display scale with no bold, and three drawn marks used about ten times per page. §6 owns the `body` base.

### 5.1 Five voices

| Token | Face, weights | Job | Never |
|---|---|---|---|
| `--serif` | Instrument Serif 400, 400 italic | names, headlines, ledes, quotes, numerals, figure titles, 3D labels, italic asides | buttons, labels |
| `--sans` | Jost 300 · 400 · 500 | 300 running text · 400 kickers, links, captions · 500 `.lab` only | headlines |
| `--mono` | DM Mono 400 | numbers, figure headers, notes, captions, detail labels, tags, colophon | running text |
| `--ui` | Nunito 700 · 600 | 700 everything pressable, interface labels, readout captions · 600 field values, drawer summaries | anything outside a control |
| `--hand` | Caveat 500 | the one pencilled aside (§8) | anything else |

```css
--serif: "Instrument Serif", "Times New Roman", Georgia, serif;
--sans: "Jost", "Helvetica Neue", Arial, sans-serif;
--mono: "DM Mono", ui-monospace, "SFMono-Regular", Menlo, Consolas, monospace;
--hand: "Caveat", "Segoe Print", "Bradley Hand", cursive;
--ui: "Nunito", "Avenir Next", "Segoe UI", system-ui, sans-serif;
```

- Running text is Jost **300** and **MUST NOT** exceed 400. Why: the light weight on warm paper is the delicacy. Display type is serif 400; weight is faked only by `.ink` (§5.6.2).
- Read as part of the article → Jost. Operated or read off an instrument → Nunito 700 at 0.06em (§5.3.4).

### 5.2 Files and rendering

```css
@font-face { font-family: "Instrument Serif"; font-style: normal; font-weight: 400; font-display: swap; src: url(assets/fonts/instrument-serif-400.woff2) format("woff2"); }
@font-face { font-family: "Instrument Serif"; font-style: italic; font-weight: 400; font-display: swap; src: url(assets/fonts/instrument-serif-400-italic.woff2) format("woff2"); }
@font-face { font-family: "Jost"; font-style: normal; font-weight: 300; font-display: swap; src: url(assets/fonts/jost-300.woff2) format("woff2"); }
@font-face { font-family: "Jost"; font-style: normal; font-weight: 400; font-display: swap; src: url(assets/fonts/jost-400.woff2) format("woff2"); }
@font-face { font-family: "Nunito"; font-style: normal; font-weight: 600; font-display: swap; src: url(assets/fonts/nunito-600.woff2) format("woff2"); }
@font-face { font-family: "Nunito"; font-style: normal; font-weight: 700; font-display: swap; src: url(assets/fonts/nunito-700.woff2) format("woff2"); }
@font-face { font-family: "DM Mono"; font-style: normal; font-weight: 400; font-display: swap; src: url(assets/fonts/dm-mono-400.woff2) format("woff2"); }
@font-face { font-family: "Jost"; font-style: normal; font-weight: 500; font-display: swap; src: url(assets/fonts/jost-500.woff2) format("woff2"); }
@font-face { font-family: "Caveat"; font-style: normal; font-weight: 500; font-display: swap; src: url(assets/fonts/caveat-500.woff2) format("woff2"); }
```

Preload after the portrait (§2.5), in this order: `instrument-serif-400`, `instrument-serif-400-italic`, `jost-300`, `jost-400`, `jost-500`, `nunito-700`, `dm-mono-400`, each as:

```html
<link rel="preload" href="assets/fonts/jost-300.woff2" as="font" type="font/woff2" crossorigin>
```

- `nunito-600` and `caveat-500` (below the first screen) are not preloaded. All nine ≈ 137 KB.
- Both serif styles are preloaded because the cover name's fit formula (§7) is measured on them. Why: a short `swap` window means a short reflow under the photo.
- `crossorigin` is required even same-origin. Why: fonts load in CORS mode; without it the file downloads twice.
- `body` sets `font-synthesis: none`, `-webkit-font-smoothing: antialiased`, `text-rendering: optimizeLegibility` (§6). Why: the serif has no bold, the others no italic; nothing may be faked. `strong` and `b` are reset to 400.
- DM Mono's zero is slashed by default (`No. Ø1`); no `font-feature-settings` anywhere. Right-aligned amounts in a column take `font-variant-numeric: tabular-nums; white-space: nowrap;`.

### 5.3 Type scale

Colours are §4 tokens; brackets give the size at 1440. Component-only levels live in their chapters: wordmark, nav (§7); contents (§8); drop cap (§9); top lines, asides (§10); next headline, e-mail (§11); figure titles, captions, verdicts (§12).

#### 5.3.1 Serif · `var(--serif)`

| Level | `font` | Tracking | Colour | Changes |
|---|---|---|---|---|
| Cover name `.mast` | `400 clamp(36px, 11.6vw, 196px)/0.9` inside the fit `min()` (§7) [167] | -0.03em; per word §7 | `#f3f1ea` on photo; ink echo | one line at every width |
| Opening line `.hl` | `400 clamp(40px, 7.2vw, 132px)/0.9` [103.7] | -0.035em | ink | ≤ 620 `clamp(46px, 14.6vw, 66px)` |
| Feature headline `.feature h2` | `400 var(--hs, clamp(40px, 7vw, 120px))/var(--lh, 0.94)` | `var(--tr, -0.03em)`; `em` -0.012em | ink | ≤ 760 `text-wrap: balance`; §5.4 |
| Lede `.lede` | `400 var(--ls, clamp(21px, 2vw, 34px))/1.22` | -0.008em | ink | balance |
| Pull quote `.quote` | `400 italic var(--qs, clamp(30px, 6vw, 110px))/1.02` | -0.02em | ink | balance; §5.6.5 |
| Giant numeral `.f-num` | `italic 400 clamp(72px, 9.2vw, 176px)/0.8` [132] | -0.04em | accent | ≤ 760 `clamp(64px, 22vw, 96px)` |
| Glance name `.gl-name` | `400 clamp(30px, 4.4vw, 58px)/1` [58] | -0.02em | ink | ≤ 640 `clamp(27px, 8vw, 34px)` |
| Letter headline `#letH` | `400 clamp(38px, 5.2vw, 96px)/1.04` [74.9] | -0.022em | ink | balance |
| Readout numeral `.nums b` | `400 clamp(28px, 7vw, 40px)/1.05` | -0.01em | ink | — |

#### 5.3.2 Jost · `var(--sans)`

| Level | `font` | Tracking, case | Colour | Changes |
|---|---|---|---|---|
| Body `body` | `300 17px/1.68` | 0 | ink | ≤ 300 14.5px/1.6 · ≤ 360 15.5px · ≥ 1600 18px · ≥ 2200 19px |
| Kicker `.letter .lab` / `.gl-top .lab` | `400 15px/1.4` / `400 14px/1.3` | 0, `text-transform: none` | muted / ink | — |
| Micro caps `.lab` | `500 10.5px/1.4` | 0.16em, UPPER | muted; on photo `rgba(243, 241, 234, 0.88)` | ≤ 300 0.1em |

Kickers are sentence case; tracked caps live only on the photo and in interface labels. Why: caps over every heading read as a template.

#### 5.3.3 DM Mono · `var(--mono)`

| Level | `font` | Tracking | Colour |
|---|---|---|---|
| Note `.note` | `400 12px/1.5` (on photo 12.5px) | -0.01em (on photo 0.02em) | muted |
| Section number `.f-top .no` | `400 12px/1.4` | 0.02em | ink; first feature accent-ink |
| In-tray label `.insert .lab` | `400 11.5px/1.4`, `text-transform: none` | 0.03em | muted |
| Details label `.side dt` | `400 11.5px/1.75` | 0.02em | muted |

#### 5.3.4 Nunito · `var(--ui)`

Button `.btn` `700 14.5px`, 0.01em (≤ 360 0.12em). Field label `label` 700 11.5px/1.5 uppercase, muted; live value `label output` `700 13px`, ink, floated right. Readout caption `.nums` 700 11px uppercase, muted. Field values `600 16px` (iOS zooms on focus below 16px).

Components declare small caps in Jost; this rule, last in the stylesheet, swaps the face and keeps size, case and colour. Every interface caption in a figure joins its list (§12).

```css
label, .nums, .tag, .tab-body h6 { font-family: var(--ui); font-weight: 700; letter-spacing: 0.06em; }
@media (max-width: 300px) { .lab, label { letter-spacing: 0.1em; } }
```

#### 5.3.5 Caveat · `var(--hand)`

```css
.hand { font: 500 clamp(21px, 1.9vw, 27px)/1.05 var(--hand); letter-spacing: 0.005em; }
```

Colour `--accent-ink` (§4).

### 5.4 Feature headline tokens

Each feature sets its template's tokens on its own id (`#f2 { --hs: …; }` for a feature on L2); bracketed values are undeclared defaults.

| Template | `--hs` [1440] | `--lh` | `--tr` | `--ls` | `--qs` |
|---|---|---|---|---|---|
| L1 | `clamp(48px, 8vw, 150px)` [115.2] | `0.9` | `-0.036em` | `clamp(22px, 2.3vw, 40px)` | — |
| L2 | `clamp(44px, 6.6vw, 124px)` [95] | [0.94] | `-0.03em` | `clamp(21px, 2vw, 34px)` | — |
| L3 | `clamp(44px, 7vw, 132px)` [100.8] | `0.92` | [-0.03em] | `clamp(21px, 2.1vw, 36px)` | `clamp(36px, 6.4vw, 118px)` |
| L4 | `clamp(38px, 4.6vw, 84px)` [66.2] | `0.98` | `-0.026em` | `clamp(20px, 1.85vw, 31px)` | `clamp(32px, 5.2vw, 96px)` |
| L5 | `clamp(36px, 4.8vw, 88px)` [69.1] | `1` | `-0.024em` | `clamp(20px, 1.8vw, 30px)` | `clamp(30px, 4.2vw, 76px)` |
| L6 | `clamp(42px, 6.6vw, 124px)` [95] | [0.94] | `-0.02em` | `clamp(23px, 2.7vw, 46px)` | — |
| L7 | `clamp(40px, 6vw, 112px)` [86.4] | `0.98` | `-0.028em` | `clamp(21px, 2vw, 34px)` | `clamp(38px, 7vw, 132px)` |

Size follows the composition: narrow-track headlines (L4, L5) are ≈ 40% smaller, with their ledes and quotes; bigger type takes tighter tracking and leading. Caps land at ≈ 1830–1890px. Why: type and frame stop growing together, before the 1920 frame step.

### 5.5 Hand-set headlines

#### 5.5.1 Per-word tracking

Values are `em`, so they hold at every size. Scope each class to its headline id (`#fNh .t1`): the value belongs to that word's letters. At most two hand-set words per headline.

| Class | Job | As shipped |
|---|---|---|
| `.t1` | tighten a wide first word | -0.042em (L2), -0.034em (L4) |
| `.t2` | tighten the last word | -0.034em (L6); -0.018em + `margin-right: -0.02em` when right-aligned (L2) |
| `.sq` | crush the stamped last word, with `.ink` | -0.05em (L1) |
| `.wide` | open one word: emphasis by air | +0.014em (L5); letter `.ink.wide` +0.022em, `margin: 0 0.02em` |

The cover name (`.m1`–`.m3`) and opening line (`.tt` -0.055em, `.hl .ink` -0.02em, `.hl em` -0.01em + `padding-right: 0.04em`) use the same system (§7). Join the last two words of a display line with `&nbsp;`.

#### 5.5.2 Optical left margins

A left-aligned headline at the column edge is pulled left so its first stem sits where the top-line rule starts: `#fNh { margin-left: -0.05em; }` (L1), `-0.045em` (L5), `-0.04em` (L7); `.f-num` `-0.06em`. Why: the side bearing grows with size (≈ 5px at 115px).

#### 5.5.3 Desktop-only breaks

```css
br.d { display: none; }
@media (min-width: 1100px) { br.d { display: inline; } }
```

One `<br class="d">` per two-line headline or quote; narrower widths reflow and balance. A plain `<br>` (L4) breaks at every width, for headlines whose two-line shape is the point.

#### 5.5.4 The stepped title (L3)

```html
<h2 id="fNh" data-fly><span class="l1">{line one}</span> <span class="l2"><em>{line two}</em></span></h2>
```

```css
#fNh .l1, #fNh .l2 { display: block; }
#fNh .l2 { letter-spacing: -0.012em; margin-top: -0.04em; }
@media (min-width: 761px) and (max-width: 1099px) { #fNh .l2 { margin-left: 16%; } }
@media (max-width: 760px) { #fNh .l2 { margin-left: 14%; } }
@media (min-width: 1100px) {
  #fNh .l2 { margin-left: calc((100% + var(--cg)) / 4); }
  #fNh::after { content: ""; display: block; height: 1px; background: var(--ink); margin: clamp(16px, 1.8vw, 26px) 0 0 calc((100% + var(--cg)) / 4); }
}
```

The `h2` spans `col 1 / col-end`, so `(100% + cg) / 4 = 3c + 3g`: column 4's left edge, where rule, lede and body also start. Line 2 is wholly italic and **MUST** be shorter than 9 columns.

### 5.6 Emphasis

#### 5.6.1 One italic hinge

A headline has **at most one** italic run, the hinge that turns the sentence ("Headline with *one* italic word"), never the expected noun. Fixed cases: the cover name's middle word, the stepped title's line 2, the next card's second word. Italics track looser than roman beside them and take ≈ 0.04em right padding. Why: italic glyphs are narrower and lean forward.

#### 5.6.2 The ink stamp

```css
.ink { text-shadow: max(1.1px, 0.022em) 0 0 currentColor; }
.ink::selection { text-shadow: none; }
@media (forced-colors: active) { .ink { text-shadow: none; } }
```

- Each glyph smears sideways in its own colour: stems thicken, counters stay open, a stamp that spread (2.3px at 104px, never under 1.1px). `currentColor` follows theme and inverse card. Selection (doubled edge) and forced colours (system repaint) drop it.
- About five per page: the opening's key word, the letter's `.ink.wide` word, the last word of one or two feature headlines (`.ink.sq`), one quote turn (`.up.ink`). At most one per headline, never the italic word; track it looser than its line. Why: the shadow widens each glyph rightward.

#### 5.6.3 Emphasis in running text

```css
.cols strong, .why li b { font-weight: 400; text-decoration: underline 1.5px var(--sand); text-underline-offset: 0.22em; }
```

Long-form text (letter, a figure's rationale list): 400 plus a sand underline, never bold; one phrase per letter. A muted paragraph steps to `--ink` at 400 (`.stand b`); a verdict's figures are `b` at 400 in `--accent-ink`. No `<em>` in Jost, mono or Nunito: with synthesis off it stays upright.

#### 5.6.4 Accent numerals

Numerals ≥ ≈ 24px **MAY** use the lighter `--accent` (≥ 4.64 : 1 on `--paper2`); smaller ones use `--accent-ink`. They are: the L5 giant numeral (`aria-hidden="true"`); a glance fact's lead numeral, `.gl-facts b { font-weight: 400; color: var(--accent); font-size: 1.25em; line-height: 0.9; }`; rationale counters, `.why li::before { content: counter(w); font: 400 italic 26px/1 var(--serif); color: var(--accent); }`; the first feature's number.

#### 5.6.5 Pull quotes

```html
<p class="quote" data-fly=".6"><span class="hang" aria-hidden="true">&ldquo;</span>{{features[i].quote.text}} <span class="up">{{features[i].quote.turn}}</span><span aria-hidden="true">&rdquo;</span></p>
```

```css
.quote .up { font-style: normal; letter-spacing: -0.012em; }
.quote .hang { display: inline-block; width: 0; transform: translateX(-0.46em); color: var(--sand); }
```

- The quote is italic, its turn upright. L4 and L5 invert it: `font-style: normal` on the quote, `font-style: italic; letter-spacing: -0.005em` on `.up`. Why: four identical quotes read as a template.
- The opening mark is sand, zero-width, hung in the margin so the text edge stays on the column line; the closing mark is ink. Both are hidden from assistive tech.
- Only L4 and L7 quotes carry marks (not L3 over its tray, not right-aligned L5). The mark needs ≥ 0.46em of free margin.

### 5.7 Marks

`preserveAspectRatio='none'` stretches each drawing to any word; `vector-effect='non-scaling-stroke'` keeps the line 1.7 or 2px. A `url()` image cannot read custom properties, so colour is baked in and the tokens are declared in `:root` and both dark blocks (§4).

```css
/* :root */
--brush: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 240 24' preserveAspectRatio='none'%3E%3Cpath fill='%239a7650' fill-opacity='.45' d='M4 13C38 8 92 9 140 8s80-2 96 2c3 2 1 5-5 5-40 1-110 2-160 4S14 21 6 20c-4-1-5-5-2-7z'/%3E%3Cpath fill='%239a7650' fill-opacity='.24' d='M18 18c50-2 120-4 200-3-60 3-130 5-200 5z'/%3E%3C/svg%3E");
--pencil: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 10' preserveAspectRatio='none'%3E%3Cpath d='M2 6.5C30 4 61 3.2 97 4.1s69 2.4 101-.6' fill='none' stroke='%23546634' stroke-width='1.7' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E");
--loop: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 80' preserveAspectRatio='none'%3E%3Cpath d='M118 9C70 1 16 11 8 37c-7 25 49 39 103 36 50-3 86-17 82-40C189 12 141 3 95 6 71 8 52 13 38 20' fill='none' stroke='%23546634' stroke-width='2' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E");
/* :root[data-theme="dark"] and the system-dark block */
--brush: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 240 24' preserveAspectRatio='none'%3E%3Cpath fill='%23c9a77c' fill-opacity='.36' d='M4 13C38 8 92 9 140 8s80-2 96 2c3 2 1 5-5 5-40 1-110 2-160 4S14 21 6 20c-4-1-5-5-2-7z'/%3E%3Cpath fill='%23c9a77c' fill-opacity='.2' d='M18 18c50-2 120-4 200-3-60 3-130 5-200 5z'/%3E%3C/svg%3E");
--pencil: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 10' preserveAspectRatio='none'%3E%3Cpath d='M2 6.5C30 4 61 3.2 97 4.1s69 2.4 101-.6' fill='none' stroke='%23a9b77c' stroke-width='1.7' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E");
--loop: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 80' preserveAspectRatio='none'%3E%3Cpath d='M118 9C70 1 16 11 8 37c-7 25 49 39 103 36 50-3 86-17 82-40C189 12 141 3 95 6 71 8 52 13 38 20' fill='none' stroke='%23a9b77c' stroke-width='2' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E");
```

#### 5.7.1 Brush and loop

```html
<em class="brush">{{opening.brushWord}}</em>   <span class="loop">{{letter.loopWord}}</span>
```

```css
.brush { background: var(--brush) no-repeat 0 92%/100% 0.38em; -webkit-box-decoration-break: clone; box-decoration-break: clone; padding: 0 0.05em; }
.loop { position: relative; display: inline-block; margin: 0 0.1em 0 0.16em; }
.loop::after { content: ""; position: absolute; inset: -0.1em -0.3em -0.06em -0.26em; background: var(--loop) no-repeat center/100% 100%; pointer-events: none; transform: rotate(-2.5deg); }
```

- **Brush**: a filled, tapered dry-brush swipe in sand with a fainter streak beneath, 0.38em tall, low across the baseline, a little past the last letter. `clone` gives each wrapped fragment a whole stroke.
- **Loop**: an open pen ellipse overshooting where it closes, more overhang right, tilted -2.5deg (a hand never circles level). `inline-block` limits it to one word; the margins keep the ring off neighbours; `pointer-events: none` keeps the word selectable.

#### 5.7.2 Pencil underline

```css
.bar nav a { background: var(--pencil) no-repeat 0 calc(100% - 7px) / 0 0.3em; transition: background-size 0.4s cubic-bezier(0.3, 0.9, 0.3, 1.15), color 0.2s; }
.bar nav a:hover, .bar nav a:focus-visible { color: var(--ink); background-size: 100% 0.3em; }
```

| Use | Height (width 0 → 100%) | Position | Duration |
|---|---|---|---|
| bar links | `0.3em` | `0 calc(100% - 7px)` | 0.4s |
| contents titles `.inside .t` | `0.32em` | `0 100%`, `padding-bottom: 0.18em` | 0.45s |
| e-mail `.mail` | `0.28em` | `0 100%`, `padding-bottom: 6px` | 0.55s |

The stroke grows from the left by `background-size` (no layout work); the easing overshoots past 100% (clipped at the box edge) and settles: a pencil flick. Bigger text draws longer. Hover and `:focus-visible` share the rule. A multi-line title keeps the default `slice`: one stroke draws through its lines in reading order.

#### 5.7.3 Rarity budget

Brush ×2 (the opening's italic last word, the glance family name); loop ×1 (letter headline); the pencilled aside with its arrow ×1 (wide screens, §8); sand underlines ≤ 6. Pencil underlines (hover, focus) do not count. About ten per page; **MUST NOT** show more than two in a 1440 × 900 viewport, never on an `.ink` word. Why: rarity makes a mark read as the editor's hand. Marks are backgrounds or pseudo-elements, invisible to assistive tech; brush and loop are static.

### 5.8 Pitfalls

1. A `font` shorthand resets `line-height` and every `font-variant-*`; put `font-variant-numeric` after it.
2. A new accent or sand **MUST** be re-encoded (`#` → `%23`) in all three token blocks.

### 5.9 Adapting to new content

Display sizes are tuned to their text: when it changes, measure and derive again; if a limit still breaks, cut words (§3).

**1 · Measure `W`**, the em-width of the longest desktop line with its hand-set spans, after `document.fonts.ready`:

```js
function emWidth(el) {
  var c = el.cloneNode(true), w;
  c.style.cssText += ";position:absolute;visibility:hidden;white-space:nowrap;width:auto;max-width:none;margin:0;font-size:100px";
  el.parentNode.appendChild(c); w = c.getBoundingClientRect().width / 100; c.remove();
  return w;
}
```

Without a browser: ≈ 0.33em per character, spaces included.

**2 · Track width.** Frame `F = min(vw - 2·gut, --max)`; `k` columns span `k·(F + cg)/12 - cg`. At 1880, `F = 1560`, `cg ≈ 32`: ≈ `132.7k - 32` px.

**3 · Derive.**

| Text | Rule | Tuned to |
|---|---|---|
| Cover name | `N = W × 1.03`; slope `86 / N` vw, rounded up to 0.1; cap = slope × 16.9 (engages ≈ 1690), check `cap × W ≤ 1500`; §7 | W 7.25 → 7.45, 11.6vw, 196px |
| Feature headline | max = span at 1880 / W; vw ≈ max / 18.8; min 36–48px, longest word ≤ 355px at 390 | L1: 150px, 8vw |
| Opening line | 5–9 words in two lines: tune `max-width` (ch) until the break falls after the intended word at 1440, 820, 390 | ≈ 7 words, 12.5ch |
| Letter headline | 6–10 words: two lines over `col 1 / col 10` (≈ 980px at 1440), three at 390; lower only the vw term | ≈ 50 characters |

Glance name, next headline and e-mail follow the same steps (§9, §11).

**4 · Hand-set again.** Keep the template's `--tr`; move at most two loose words -0.005 … -0.015em from it, or open one +0.014 … +0.022em; never track an italic as tight as roman. Check the first stem against the rule at 1440, then 390, 1100 and 1920.

---

## 6 · Grid, spacing & breakpoints

One frame, one 12-column grid with a bleed track on each side, pauses set in long/short pairs, and one list of breakpoints. This chapter also owns the document base (B.03) and the grid (B.05).

### 6.1 Document base

#### 6.1.1 Spec (B.03, exact values)

```css
/* ── reset and body ── */
* { box-sizing: border-box; scrollbar-width: thin; scrollbar-color: color-mix(in srgb, var(--ink) 30%, transparent) transparent; }
html { scroll-behavior: smooth; scroll-padding-top: 64px; }
body {
  margin: 0; background-color: var(--paper); background-image: var(--grain); color: var(--ink);
  font: 300 17px/1.68 var(--sans); font-synthesis: none; -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility; overflow-x: clip;
}
a { color: inherit; }
::selection { background: color-mix(in srgb, var(--sand) 42%, transparent); color: inherit; }
.wrap { margin: 0 auto; padding: 0 var(--gut); max-width: calc(var(--max) + 2 * var(--gut)); }
:focus-visible { outline: 1.5px solid var(--accent); outline-offset: 3px; }
@media (max-width: 360px) { body { font-size: 15.5px; } }
@media (max-width: 300px) { body { font-size: 14.5px; line-height: 1.6; } }
@media (max-height: 520px) and (orientation: landscape) { html { scroll-padding-top: 0; } }
@media (min-width: 1600px) { body { font-size: 18px; } }
@media (min-width: 2200px) { body { font-size: 19px; } }
@media (prefers-reduced-motion: reduce) { html { scroll-behavior: auto; } }

/* ── scrollbars ── */
::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track, ::-webkit-scrollbar-corner { background: transparent; }
::-webkit-scrollbar-thumb { background: color-mix(in srgb, var(--ink) 28%, transparent); border-radius: 8px; border: 2px solid transparent; background-clip: padding-box; }
::-webkit-scrollbar-thumb:hover { background-color: color-mix(in srgb, var(--ink) 48%, transparent); }
::-webkit-scrollbar-button { display: none; width: 0; height: 0; }

/* ── taps and focus ── */
html { -webkit-tap-highlight-color: transparent; }
button, a, label, select, input[type="range"], canvas,
.csel-btn, .csel-opt, .mi, .toggles, .seg, .zoom, .runrow, .tab-foot {
  -webkit-touch-callout: none; -webkit-user-select: none; user-select: none;
}
:focus:not(:focus-visible) { outline: none; }
input[type="range"]:focus, input[type="range"]:focus-visible { outline: none; box-shadow: none; }
input[type="range"]::-moz-focus-outer { border: 0; }
input[type="range"]:focus-visible::-webkit-slider-thumb { box-shadow: 0 0 0 2px var(--surf), 0 0 0 4px var(--accent); }
input[type="range"]:focus-visible::-moz-range-thumb { box-shadow: 0 0 0 2px var(--surf), 0 0 0 4px var(--accent); }
select:focus:not(:focus-visible), .csel-btn:focus:not(:focus-visible) { outline: none; box-shadow: var(--sink-s); }

/* ── utilities ── */
.hidden { display: none !important; }
.nw { white-space: nowrap; }
```

Write one declaration per line in the file (§2.4). This part's theme-transition rules are in §7; `.ink::selection` is in §5.

#### 6.1.2 Why

- **`overflow-x: clip`, never `hidden`**, trims bleeds and tilted prints at the screen edge. Why: `clip` never creates a scroll container; `hidden` makes `body` one as soon as `html` gets any overflow value, and the sticky bar and pinned orbit stage then silently stop sticking.
- **`scroll-padding-top: 64px`** = 54px bar + 1px hairline + 9px air; `0` in short landscape, where the bar is static (§7).
- **Keyboard-only rings**, offset 3px to clear moulded shadows. Mouse focus on a `select` or custom select shows `--sink-s` (pressed in). Range: no ring on the track; the thumb gets 2px surface + 2px accent.
- **Scrollbars twice:** the standard pair for Firefox and current Chromium, `::-webkit-scrollbar` for Safari. Result: 8px track, 4px pill at 28% ink (48% hover), no arrows.
- **Pressables** get no tap flash, callout or selection, so dragging a slider or a canvas never selects text; running text stays selectable. `.hidden` beats any component `display` (scripts toggle it); `.nw` keeps a number and unit, or a hyphenated compound, together.

### 6.2 The frame

`.wrap` gives a centred box of `min(100% − 2·--gut, --max)`: the same frame as `.g`, so wrapped and gridded sections align to the pixel. It is used inside `header.bar` and `footer` and on `section.opening` and `section#orbit`; `section.cover` repeats it with its own top padding (§7). Every later section is a `.g`. Tokens: `--gut: clamp(16px, 4.4vw, 84px)`, `--cg: clamp(12px, 1.7vw, 32px)`, `--max: 1560px` (`1680px` under `(min-width: 1920px)`).

### 6.3 The grid

#### 6.3.1 Spec (B.05, exact)

```css
.g {
  display: grid;
  column-gap: var(--cg);
  align-items: start;
  grid-template-columns:
    [bleed-start] minmax(calc(var(--gut) - var(--cg)), 1fr) repeat(
      12,
      [col] minmax(0, calc((var(--max) - 11 * var(--cg)) / 12))
    )
    [col-end] minmax(calc(var(--gut) - var(--cg)), 1fr) [bleed-end];
}
.g > * {
  grid-column: col 1 / col-end;
  min-width: 0;
}
```

`br.d` also lives in B.05 (§5.5.3).

#### 6.3.2 Track math

- 14 tracks, 13 gaps. `col N` is column N's left line; `col-end` is column 12's right edge.
- **Bleed minimum = `gut − cg`**, so bleed + gap = `--gut` and column 1 starts `--gut` from the screen edge at every width below the cap (minimum 4px, never negative).
- **Column cap = `(max − 11·cg) / 12`**, so 12 columns + 11 gaps = `--max`. Columns fill before the `1fr` bleeds: frame = `min(100% − 2·gut, --max)`; above the cap the bleeds split the rest.
- `min-width: 0` stops long words and canvases widening a track; `align-items: start` keeps item heights (sticky children, uneven rows).
- `--gut` is 16px below 364px, 84px from 1909px; `--cg` 12px below 706px, 32px from 1882px; the frame caps at ≈ 1711px and widens to 1680px at 1920px.

#### 6.3.3 Computed tracks (px)

| Viewport | `--gut` | `--cg` | Frame | Column | Bleed track | Edge → col 1 | Body |
|---|---|---|---|---|---|---|---|
| 300 | 16 | 12 | 268 | 11.3 | 4 | 16 | 14.5 |
| 390 | 17.2 | 12 | 355.7 | 18.6 | 5.2 | 17.2 | 17 |
| 820 | 36.1 | 13.9 | 747.8 | 49.5 | 22.1 | 36.1 | 17 |
| 1440 | 63.4 | 24.5 | 1313.3 | 87.0 | 38.9 | 63.4 | 17 |
| 1711 | 75.3 | 29.1 | 1560 | 103.3 | 46.4 | 75.5 | 18 |
| 1919 | 84 | 32 | 1560 | 100.7 | 147.5 | 179.5 | 18 |
| 1920 | 84 | 32 | 1680 | 110.7 | 88 | 120 | 18 |
| 2560 | 84 | 32 | 1680 | 110.7 | 408 | 440 | 19 |

Left edges of `col 1 … col 12`, then `col-end`, in px:

| Width | col 1–12 | col-end |
|---|---|---|
| 1440 | 63, 175, 286, 398, 509, 621, 732, 844, 955, 1067, 1178, 1290 (step 111.5) | 1377 |
| 390 | 17, 48, 78, 109, 140, 170, 201, 232, 262, 293, 324, 354 | 373 |

Acceptance: `col 3 / col-end` spans x = 286 → 1377 at 1440 and 78 → 373 at 390, ±1px.

#### 6.3.4 Placement vocabulary

| Expression | Means |
|---|---|
| `col N` as start / as end | left edge of column N / right edge of column N − 1 (never the gap) |
| `bleed-start` / `bleed-end` | the left / right screen edge |
| `col N / col M` | columns N … M − 1: `col 1 / col 8` = 7, `col 9 / col-end` = 4 |
| `col 1 / col-end` | the frame (default) |

Parts **MUST** be placed with named lines only: no numeric lines, no negative side margins, no `100vw`. `grid-row` is assigned only at ≥ 1100. Below that, DOM order is the layout, so the DOM **MUST** follow reading order (§10).

#### 6.3.5 The axes

At ≥ 1100 almost every start falls on six lines; that is why seven compositions read as one magazine.

| Axis | Line | x at 1440 | Carries |
|---|---|---|---|
| Frame line | `col 1` | 63 | top lines, kickers, headlines (optical margin, §5.5.2), ledes |
| Indent line | `col 2` | 175 | body under a lede (L1, L2), L2/L6 trays, next card, contact e-mail and links, quotes ≤ 1099 |
| Stair line | `col 4` | 398 | L3's second line, the rule under it, its lede and body; L2 headline start |
| Half line | `col 6` | 621 | letter body; L4 lede, body, quote; L5 quote start |
| Margin column | `col 9` → `col-end` | 955 → 1377 | details (L1, L2); run-in details and claims from `col 10` (L5, L7) |
| Right frame line | `col-end` | 1377 | right-aligned headline (L2) and quote (L5), field labels, tray notes |

Text is ragged-right. Leaving half a row as empty paper is normal.

#### 6.3.6 Bleeds

A plate (`--paper2` or `--ink`) that runs off a screen edge **MUST** square its corners on that side. It **MUST** also add `--gut` to its padding on that side, which brings its content back to the frame.

| Plate | Band | `grid-column` | Radius | Bleeding-side padding |
|---|---|---|---|---|
| Glance card → | ≥ 1280 | `col 3 / bleed-end` | `28px 0 0 28px` | `calc(var(--gut) + clamp(16px, 3vw, 44px))` |
| | 761–1279 | `col 1 / bleed-end` | `28px 0 0 28px` | `calc(var(--gut) + clamp(16px, 3vw, 40px))` |
| | ≤ 760 | `col 1 / bleed-end` | `24px 0 0 24px` | `calc(var(--gut) + 16px)` |
| L4 tray ← | ≥ 1100 | `bleed-start / col 12` | `0 28px 28px 0` | `calc(var(--gut) + clamp(16px, 2.4vw, 32px))` |
| | 761–1099 | `bleed-start / col-end` | `0 28px 28px 0` | same |
| | ≤ 760 | `bleed-start / col-end` | `0 22px 22px 0` | `calc(var(--gut) + 12px)` |
| Next card → | ≥ 761 | `col 2 / bleed-end` | `32px 0 0 32px` | `calc(var(--gut) + clamp(24px, 2.6vw, 44px))` |
| | ≤ 760 | `col 1 / bleed-end` | `24px 0 0 24px` | `calc(var(--gut) + 18px)` |

- **Plates alternate down the page: right, left, right.** Exactly one tray bleeds left. Why: the page sways like facing pages.
- **Text may bleed without a plate** to stay on one line: L1 headline and L7 quote (`col 1 / bleed-end`), the e-mail (`col 2 / bleed-end`). No corners, no padding.
- Above ≈ 1711px the bleed exceeds `--gut`, so a bleeding plate's content shifts edgeward by `(100% − --max) / 2 − --gut` (36px at 1920, 356px at 2560). Keep the formula.

#### 6.3.7 Pitfalls

- **Spans end before their last line.** `col 6 / col 12` stops at the right edge of column 11.
- **`.g > *` has specificity (0,1,0).** A one-class placement only beats it because B.05 comes earlier, so B.05 **MUST** precede every component part.
- **No wrapper `div` around grid parts.** Each part must be a direct child of its `.g`.
- **No `overflow: hidden` on a section**: it clips 0.8-leading type, bleeds and prints, and breaks sticky.
- **Auto-placement pairs siblings**: an item starting right of the previous one's start shares its row (§10 uses this on tablets). Set a row when it must not.

#### 6.3.8 Adapting

The grid is the same for every person. Keep `--max`, `--gut`, `--cg` and the line names. To re-compose, move parts between the six axes; never add columns or new line names.

### 6.4 Section spacing

Pauses are set by eye, in pairs, not from a scale. Each section owns its top spacing (`margin-top` unless noted); none has a bottom margin. Only air and each section's own 1px top rule separate sections: no background bands.

| Element | Value | 390 | 820 | 1440 | Pause |
|---|---|---|---|---|---|
| `section.cover` `padding-top` | `clamp(14px, 3vw, 30px)` | 14 | 25 | 30 | below the bar |
| `section.opening` `padding-top` | `clamp(40px, 7vw, 120px)` | 40 | 57 | 101 | medium |
| `section#orbit` | none; text layer `padding: 6svh 0 36svh` (§8) | | | | |
| `section#glance` | `clamp(72px, 9vw, 150px)` | 72 | 74 | 130 | medium |
| `.orbit + .glance` in `(min-width: 1100px) and (min-height: 680px)` | `clamp(24px, 3vw, 48px)` | | | 43 | short: the orbit ends in air |
| `section#letter` | `clamp(88px, 12vw, 200px)` | 88 | 98 | 173 | long |
| `#f1` · P1 | `clamp(104px, 14vw, 240px)` | 104 | 115 | 202 | long: before the work |
| `#f2` · P2 | `clamp(80px, 9.5vw, 156px)` | 80 | 80 | 137 | short |
| `#f3` · P3 | `clamp(112px, 15vw, 260px)` | 112 | 123 | 216 | long |
| `#f4` · P4 | `clamp(64px, 7vw, 116px)` | 64 | 64 | 101 | short |
| `#f5` · P5 | `clamp(96px, 12vw, 200px)` | 96 | 98 | 173 | long |
| `#f6` · P6 | `clamp(64px, 6vw, 100px)` | 64 | 64 | 86 | short |
| last feature · P7 | `clamp(104px, 13vw, 220px)` | 104 | 107 | 187 | long |
| `section#next` | `clamp(84px, 11vw, 184px)` | 84 | 90 | 158 | medium |
| `section#contact` | `clamp(120px, 16vw, 270px)` | 120 | 131 | 230 | longest: before the end |
| `footer` | `clamp(72px, 9vw, 150px)`; `border-top: 1px solid var(--ink); padding: 16px 0 46px` | 72 | 74 | 130 | medium |

- **Pairs.** A short pause binds two features into a spread, a long one opens the next pair (202/137, 216/101, 173/86, 187). The longest come before the work and before the end.
- **Fewer features**. For N features, `article#f1 … #f(N−1)` take P1 … P(N−1) and the last feature takes P7. Margins follow position, while type tokens follow the template (§5.4).
- **Why clamps.** Phones get 64–120px (2–4 body lines), 1440 gets 3–8 empty lines. Every pause caps at ≈ 1640–1735px, beside the frame cap (≈ 1711px): spacing and frame stop growing together.

### 6.5 Breakpoints

#### 6.5.1 Conventions

- **Bands:** ≥ 1100 wide (the orbit pins only if also ≥ 680 tall) · 761–1099 tablet · ≤ 760 phone; ≤ 900 compact figures; ≤ 700 … ≤ 220 panels compact in steps; height ≤ 520 landscape is its own mode.
- **Complements never overlap.** Each `max-width: N` pairs with `min-width: N+1`: 760/761, 1099/1100, 1279/1280, 700/701, 620/621, and heights 679/680.
- **Placement rule.** Each `@media` block **MUST** directly follow the rule it adjusts, inside its component's part (§2.4); there is no responsive section at the end. For one property, wider `max-width` blocks come first and `min-width` blocks run narrow to wide (6.1.1 `body`). Why: a block has its base rule's specificity, so stylesheet order decides, and each component is edited in one place.

```css
.side .row { grid-template-columns: 104px 1fr; }
@media (max-width: 460px) { .side .row { grid-template-columns: 84px 1fr; } }
@media (max-width: 360px) { .side .row { grid-template-columns: 1fr; gap: 2px; } }
```

- **Scripts** call `matchMedia` with the exact strings below and read `.matches` at each use. The pinned-orbit query lives in three places (its CSS complement and two scripts); change them together.

#### 6.5.2 Master table

| Condition (exact) | Changes |
|---|---|
| `(min-width: 2200px)` · `(min-width: 1600px)` | body 19px · 18px |
| `(min-width: 1920px)` | `--max: 1680px` |
| `(min-width: 1280px)` | glance label on cols 1–2, card `col 3 / bleed-end`; L5 details one pair per line (§9, §10) |
| `(max-width: 1279px)` | narrower orbit object column and gap, lede 18px (§8) |
| `(min-width: 761px) and (max-width: 1279px)` | glance card `col 1 / bleed-end` |
| `(min-width: 1100px) and (min-height: 680px)` CSS+JS | orbit pinned, scroll turn, drawer fitting (§8); short glance pause |
| `(min-width: 1100px)` CSS+JS | `br.d`; wide compositions (features, letter, contact); arrivals travel (§16) |
| `(max-width: 1099px), (max-height: 679px)` | orbit one column (§8) |
| `(min-width: 761px) and (max-width: 1099px)` | tablet placements (§9, §10) |
| `(max-width: 999px)` | glance cover 2.5 : 1, actions under pitch, stack groups as rows (§9) |
| `(max-width: 900px)` CSS+JS | tab → launcher + sheet, tickets → spring row (§13); stage and side stack (§14); next card one column, print bottom-right (§11) |
| `(max-width: 900px) and (min-width: 701px)` | 3D stage `4/3` (§14) |
| `(max-width: 800px)` | two-panel figures stack (§12) |
| `(min-width: 761px)` | feature body in 2 columns (§10) |
| `(max-width: 760px)` | bar keeps one link (§7); phone placements; glance and next `col 1 / bleed-end`, radius 24; balanced headlines (§9–§11) |
| `(min-width: 760px)` JS | arrivals drift (≤ ±48px); below, rise only (§16) |
| `(min-width: 701px)` JS | drawers open by default; ≤ 700 closed with a summary (§12) |
| `(max-width: 700px)` | stage `1/1` (`.sq` `4/5`), smaller labels (§14); race lanes stack (§12) |
| `(max-width: 640px)` | glance cover 1.75 : 1, actions a 3-cell grid (§9) |
| `(max-width: 620px)` · `(min-width: 621px)` | cover portrait, radius 22; opening `clamp(46px, 14.6vw, 66px)` (§7); contents 2 columns (§8); readouts → cards, rows → tiles, pill strip scrolls, tracker → stepper with its text line, activity 3 rows (§12, §13); colophon left (§11) |
| `(max-width: 460px)` | details label 84px (§10); tighter control grids, stretched run-row buttons, fields 1 column, tab footer wraps, readouts 2 columns (§12, §13) |
| `(max-width: 380px)` | glance aside hidden, cover 1.6 : 1, actions 2 columns (§9) |
| `(max-width: 360px)` | body 15.5px; details rows stack; control grids 1 column; smaller `.btn`, `.seg` padding |
| `(max-width: 359px)` | bar: name and theme pill only (§7) |
| `(max-width: 300px)` | body 14.5px/1.6; `.lab, label` tracking 0.1em; glance and orbit contents 1 column; tab glyphs and launcher pill hidden |
| `(max-width: 240px)` · `(max-width: 220px)` | statement-table total on one line, then cells wrap (§12) |
| `(max-height: 520px) and (orientation: landscape)` | bar static, `scroll-padding-top: 0`; cover `min-height: 78svh`; glance cover 3.4 : 1; signature view `max-width: min(520px, 78svh)`; stage `max-height: 82svh` |
| `(hover: hover)` | print lift, contents-number tilt, contact-link hover |
| `(pointer: coarse)` | range thumb 20px, `margin-top: -5px`; toggles `9px 12px`; zoom keys 40px (§12, §14) |
| `(any-pointer: coarse)` | signature canvas `touch-action: pan-y` (§8) |

`prefers-color-scheme` → §4, `forced-colors` → §5, `prefers-reduced-motion` → §16, `print` → §17.

---

## 7 · Bar, theme switch, cover & opening line

### 7.1 The paper bar (`header.bar`)

#### 7.1.1 Purpose

Wordmark, up to five section links and the theme switch, printed on the page's own paper and grain. No blur, no shadow, one 1px `--hair` rule.

#### 7.1.2 Anatomy

```html
<header class="bar">
  <div class="wrap">
    <a class="name" href="#top">{{nameParts[0].text}} <i>{{nameParts[1].text}}</i> {{nameParts[2].text}}</a>
    <nav aria-label="Sections">
      <a href="#glance">Glance</a><a href="#inside">Inside</a><a href="#letter">Letter</a><a href="#f1">Work</a><a href="#contact">Contact</a>
      <button class="tbtn" id="themeBtn" type="button" aria-label="Switch between light and dark mode">Dark</button>
    </nav>
  </div>
</header>
<main id="top"> … </main>
```

The wordmark prints `nameParts` in order, the italic one in `<i>`. No letter: drop `#letter` (§3.4). The switch sits in the `nav` row: one `gap` spaces all.

#### 7.1.3 Spec (B.06)

```css
.bar {
  position: sticky; top: 0; z-index: 20;
  background-color: color-mix(in srgb, var(--paper) 99%, transparent);
  background-image: var(--grain); border-bottom: 1px solid var(--hair);
}
.bar .wrap { display: flex; align-items: center; justify-content: space-between; height: 54px; gap: 16px; }
.bar .name {
  font: 400 clamp(17px, 4.6vw, 23px)/1 var(--serif); letter-spacing: -0.012em;
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis; min-width: 0; text-decoration: none;
}
.bar .name i { letter-spacing: -0.02em; }
.bar nav { display: flex; gap: 22px; align-items: center; }
.bar nav a {
  position: relative; padding: 12px 0; font: 400 14px var(--sans); letter-spacing: 0.01em;
  text-decoration: none; color: var(--muted);
  background: var(--pencil) no-repeat 0 calc(100% - 7px) / 0 0.3em;
  transition: background-size 0.4s cubic-bezier(0.3, 0.9, 0.3, 1.15), color 0.2s;
}
.bar nav a:hover, .bar nav a:focus-visible { color: var(--ink); background-size: 100% 0.3em; }
@media (max-width: 760px) { .bar nav a { display: none; } .bar nav a[href="#glance"] { display: inline-block; } }
@media (max-width: 359px) { .bar nav a[href="#glance"] { display: none; } }
@media (max-height: 520px) and (orientation: landscape) { .bar { position: static; } }
```

#### 7.1.4 States

Hover and focus: `--muted` → `--ink`, pencil to full width; focus adds the ring (§6.1). The wordmark has no hover state. Dark uses the dark pencil copy (§4); print hides the bar (§17).

#### 7.1.5 Responsive

≥ 761px: all links; the row spans the card's edges (x 63 → 1377 at 1440). ≤ 760px: only `#glance`, for readers who will not scroll. ≤ 359px: none. Short landscape: static bar, `scroll-padding-top: 0` (§6.1). No menu button.

#### 7.1.6 Motion

The pencil (§5.7.2) draws from the left in 0.4s with a flick of overshoot; colour takes 0.2s. Both are user-triggered and stay under reduced motion.

#### 7.1.7 Accessibility

`scroll-padding-top: 64px` lands each anchor 9px below the bar. The `font` shorthand resets line-height to `normal`, so links are ≈ 44px tall. `--muted` on paper: 6.35:1 light, 6.45:1 dark.

#### 7.1.8 Pitfalls

- Paper **MUST** be 99%, never 100%, on any sticky layer a pinned canvas passes under. Why: Chrome culls the part of a pinned WebGL canvas behind a fully opaque sticky layer, so it vanishes early as its container scrolls off (§8, §14); 99% looks identical but is not opaque.
- **MUST NOT** use `backdrop-filter`, visible translucency or a shadow. Why: the head is printed on the sheet, not glass.
- `.name` needs `min-width: 0`. Why: without it the ellipsis never engages and the row overflows.

#### 7.1.9 Adapting

The 359px cut-off follows the wordmark: the row fits from `B = Wm + Lw + Tw + 90` (`Wm` wordmark at 17px, `Lw` `#glance` label ≈ 46px, `Tw` switch ≈ 96px; 90 = 2 × 16 gutter + gaps 16, 22, 12 + 8 air). Write `(max-width: {B − 1}px)`; `Wm` ≈ 128 gives 359. The link **MUST** go before the ellipsis engages.

### 7.2 The moulded theme switch (`button.tbtn#themeBtn`)

#### 7.2.1 Purpose

One pill, moulded from the bar's paper, holds a grooved track with a flat accent dot. Its label names the theme it switches **to**; the new theme grows out of it as a circle.

#### 7.2.2 Anatomy

Markup: 7.1.2. A raised pill ≈ 96 × 32px; `::before` is the 36 × 20px groove. The dot is a 6px `radial-gradient` at x 10px (light) or 26px (dark), 4px clear of the track's ends, top and bottom; flat, no glow. `6px → 6.5px` anti-aliases its edge.

#### 7.2.3 Spec (B.06, B.03)

```css
.tbtn {
  cursor: pointer; margin-left: 12px; display: inline-flex; align-items: center; gap: 10px;
  padding: 6px 14px 6px 7px; border: 0; border-radius: 999px;
  background: var(--face); color: var(--ink); font: 700 13.5px var(--ui);
  box-shadow: var(--raise-s); transition: box-shadow 0.2s;
}
.tbtn::before {
  content: ""; width: 36px; height: 20px; border-radius: 999px; box-shadow: var(--sink-s);
  background: radial-gradient(circle at 10px 50%, var(--accent) 6px, transparent 6.5px);
}
:root[data-theme="dark"] .tbtn::before { background: radial-gradient(circle at 26px 50%, var(--accent) 6px, transparent 6.5px); }
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) .tbtn::before { background: radial-gradient(circle at 26px 50%, var(--accent) 6px, transparent 6.5px); }
}
.tbtn:hover { background: var(--face); color: var(--ink); box-shadow: var(--raise); }
.tbtn:active { box-shadow: var(--sink-s); }
@media (max-width: 300px) { .tbtn { padding: 5px 8px; letter-spacing: 0.08em; } }
/* B.03 · theme transition */
::view-transition-old(root), ::view-transition-new(root) { animation: none; mix-blend-mode: normal; }
::view-transition-old(root) { z-index: 1; }
::view-transition-new(root) { z-index: 2; }
```

#### 7.2.4 States

Light: dot left, label "Dark", face a shade lighter than the paper, warm shadow. Dark: dot right in pale sage, label "Light", face on the paper, near-black shadow. Hover `--raise`, press `--sink-s`, focus ring; print hides it.

#### 7.2.5 Responsive

Present at every width; at ≤ 300px the padding tightens (7.2.3).

#### 7.2.6 Motion and behaviour (D.03)

The head flag script (§2.5) applies the saved `data-theme` before first paint. D.03 does the rest.

```js
(function () {
  var $ = function (id) { return document.getElementById(id); };
  if ($("yr")) $("yr").textContent = new Date().getFullYear();      /* footer, §11 */
  var root = document.documentElement, tb = $("themeBtn");
  if (!tb) return;
  var mq = window.matchMedia("(prefers-color-scheme: dark)");
  function isDark() { var t = root.getAttribute("data-theme"); return t ? t === "dark" : mq.matches; }
  function label() { tb.textContent = isDark() ? "Light" : "Dark"; }
  function syncTheme() {
    var c = isDark() ? "#1c1b18" : "#e9e6de";                      /* --paper dark / light */
    ["tcLight", "tcDark"].forEach(function (id) {
      var m = $(id); if (m) { m.setAttribute("content", c); m.removeAttribute("media"); }
    });
  }
  label();
  if (root.getAttribute("data-theme")) syncTheme();
  mq.addEventListener("change", function () { if (!root.getAttribute("data-theme")) label(); });
  tb.addEventListener("click", function () {
    var next = isDark() ? "light" : "dark";
    function apply() {
      root.setAttribute("data-theme", next);
      try { localStorage.setItem("theme", next); } catch (e) {}
      label(); syncTheme();
    }
    if (!document.startViewTransition ||
        window.matchMedia("(prefers-reduced-motion: reduce)").matches) { apply(); return; }
    var r = tb.getBoundingClientRect(), x = r.left + r.width / 2, y = r.top + r.height / 2,
      rad = Math.hypot(Math.max(x, innerWidth - x), Math.max(y, innerHeight - y));
    var vt = document.startViewTransition(apply);
    vt.ready.then(function () {
      root.animate(
        { clipPath: ["circle(0px at " + x + "px " + y + "px)", "circle(" + rad + "px at " + x + "px " + y + "px)"] },
        { duration: 700, easing: "cubic-bezier(.4,0,.2,1)", pseudoElement: "::view-transition-new(root)" });
    }).catch(function () {});
  });
})();
```

The circle grows from the button's centre to the farthest corner in 700ms. The dot does not slide (gradients do not interpolate); the reveal is the motion. Nothing transitions colour, so without the API or under reduced motion the swap is instant. Storage is written only on click; until then the OS rules.

#### 7.2.7 Accessibility

A native `<button>`. The `aria-label` contains both visible words, so label-in-name holds. No `aria-pressed`: the label is an action. Dot on paper: 5.06:1 light, 8.0:1 dark.

#### 7.2.8 Pitfalls

- Both snapshots **MUST** have `animation: none`. Why: the default cross-fade hides the circle.
- `vt.ready` **MUST** have a `.catch`. Why: skipped transitions reject. `localStorage` sits in `try/catch`: private modes throw.

#### 7.2.9 Adapting

A translated `aria-label` **MUST** still contain both labels. A new `--paper` changes the `syncTheme` pair, both metas and the manifest together (§2).

### 7.3 The cover (`section.cover`)

#### 7.3.1 Purpose

Identity in one image: a rounded photo card, role and place in its top corners, the name (the only `h1`) in cream on its bottom edge, which cuts the letters just above the baseline; an `aria-hidden` copy in page ink carries them on.

#### 7.3.2 Anatomy

```html
<section class="cover" aria-label="Cover">
  <div class="cv-hero">
    <div class="strip"><span class="lab">{{role}}</span><span class="note">{city} &middot; {{coordinates}}</span></div>
    <h1 class="mast"><span class="m1">{{nameParts[0].text}}</span> <span class="m2">{{nameParts[1].text}}</span> <span class="m3">{{nameParts[2].text}}</span></h1>
  </div>
  <p class="mast mast-echo" aria-hidden="true"><span class="m1">…</span> <span class="m2">…</span> <span class="m3">…</span></p>
</section>
```

- `{city}` = `{{location}}` up to the comma; degrees as `&deg;`; no coordinates → the city alone.
- The italic part takes `.m2`, the word before it `.m1`, the one after `.m3`; two roman words take `.m1`, `.m3`; a fourth word no class.
- The echo is a `<p>` sibling **after** the card, so `overflow: hidden` cannot clip it and the page keeps one `h1`. Its spans and text **MUST** repeat the `h1` character for character.

#### 7.3.3 Spec (B.07)

```css
.cover {
  padding: clamp(14px, 3vw, 30px) var(--gut) 0; margin: 0 auto;
  max-width: calc(var(--max) + 2 * var(--gut));   /* = .wrap: card edges = bar edges */
  --cut: 0.2em; --hp: clamp(14px, 2.6vw, 30px);
}
.cover .strip { display: flex; justify-content: space-between; gap: 8px 16px; flex-wrap: wrap; }
.mast { font: 400 clamp(36px, 11.6vw, 196px)/0.9 var(--serif); letter-spacing: -0.03em; }
.cv-hero {
  position: relative; isolation: isolate; overflow: hidden;
  display: flex; flex-direction: column; justify-content: space-between; gap: 24px;
  min-height: clamp(300px, 42vw, 560px); padding: var(--hp); padding-bottom: 0;
  border-radius: 26px; background: #2b2d29;
  -webkit-user-select: none; user-select: none; -webkit-touch-callout: none;
}
.cv-hero::before {
  content: ""; position: absolute; inset: 0; z-index: -1;
  background:
    linear-gradient(180deg, rgba(14, 15, 13, 0.42), transparent 24%),
    linear-gradient(90deg, rgba(14, 15, 13, 0.6), rgba(14, 15, 13, 0.14) 60%, transparent),
    linear-gradient(0deg, rgba(14, 15, 13, 0.55), transparent 55%),
    var(--me) 50% 42% / cover no-repeat;
  animation: cvIn 0.9s cubic-bezier(0.2, 0.7, 0.2, 1) both;
}
@keyframes cvIn { from { opacity: 0; transform: scale(1.025); } to { opacity: 1; transform: none; } }
.cv-hero .lab, .cv-hero .note { color: rgba(243, 241, 234, 0.88); }
.cv-hero .note { font-size: 12.5px; letter-spacing: 0.02em; }
.cv-hero .mast { color: #f3f1ea; margin: 0; text-shadow: 0 1px 24px rgba(0, 0, 0, 0.18); margin-bottom: calc(-1 * var(--cut)); }
.cover .mast {
  white-space: nowrap;
  font-size: min(clamp(36px, 11.6vw, 196px), calc((100vw - 2 * var(--gut) - 2 * var(--hp)) / 7.45));
}
.mast-echo {
  margin: calc(var(--cut) - 0.9em) var(--hp) 0;
  clip-path: inset(calc(0.9em - var(--cut)) -0.2em -0.2em -0.2em);
  pointer-events: none; -webkit-user-select: none; user-select: none; position: relative;
}
.mast .m1 { letter-spacing: -0.05em; }
.mast .m2 { font-style: italic; letter-spacing: -0.025em; margin: 0 0.015em 0 0.07em; }
.mast .m3 { letter-spacing: -0.018em; }
@media (max-width: 620px) {
  .cv-hero { min-height: min(440px, 118vw); border-radius: 22px; }
  .cv-hero::before { background-position: 0 0, 0 0, 0 0, 72% 42%; }
}
@media (max-height: 520px) and (orientation: landscape) { .cv-hero { min-height: 78svh; } }
@media (prefers-reduced-motion: reduce) { .cv-hero::before { animation: none; } }
```

Each shade serves one text zone: top band (captions), left wash (start of the name), bottom band (the name). All are a green-cast near-black, never black, so cream reads on any photo without a visible vignette. The photo is a CSS background (preloaded, §2.5), never an `<img>`. Photo and cream `#f3f1ea` (12.3:1 on `#2b2d29`) are fixed in both themes.

#### 7.3.4 The cut

```text
 em of the name size, from the top of the line box both copies share
 0.00  top of both boxes
 0.06  cap tops                       cream h1 visible 0 … 0.70
 0.70  ════ card bottom edge ════     = 0.9 − cut (padding-bottom 0; h1 margin-bottom −cut)
 0.79  baseline                       ink echo visible 0.70 … 0.90 (≈ 0.09em of feet)
 0.90  bottom of both boxes           −0.2em clip outsets keep italic tails and descenders
```

The two copies **MUST** stay locked together:

- **Size:** both are `.mast` inside `.cover`.
- **x:** the echo's side margins `var(--hp)` equal the card padding.
- **y:** `margin-top: cut − 0.9em` starts the echo's box where the `h1`'s starts.
- **Glyphs:** text and spans are identical.

`0.9em` appears three times (the `.mast` line-height, the echo margin, the clip); change all three together. Check: section height at 1440 = 30 + 560 + 33.4 = 623.4.

#### 7.3.5 States

Loading: text paints at once on the ground. **No portrait:** `main { --me: none; }`, no photo preload; the ground shows under the shades. Light: cream turns near-black at the edge, a stencil break. Dark: only the echo re-inks; its `#eeebe3` nearly matches the cream, so the letters hang off the card. Reduced motion: no `cvIn`. Print: no photo, shades or echo; text `#111` (§17).

#### 7.3.6 Responsive

| Viewport | 300 | 390 | 621 | 820 | 1440 | ≥ 1920 |
|---|---|---|---|---|---|---|
| Card w × h | 268 × 354 | 356 × 440 | 566 × 300 | 748 × 344 | 1313 × 560 | 1680 × 560 |
| Radius / name px | 22 / 32.2 | 22 / 44.0 | 26 / 71.7 | 26 / 94.7 | 26 / 167 | 26 / 196 |

From 621 to ≈ 1333px the card holds ≈ 2.17 : 1, then widens toward 3 : 1. At ≤ 620px it turns portrait and re-frames the photo (four positions: `background-position` is per layer). In short landscape `78svh` keeps the name in view.

#### 7.3.7 Motion

Only `::before` fades in, scaling 1.025 → 1 over 0.9s, clipped by the card so its edges never move. The name never animates. Why: it is the LCP text; it must not wait for the image.

#### 7.3.8 Accessibility

The name is announced and copied once: the echo is `aria-hidden`, pointer-transparent and unselectable. The photo is decorative; an announced portrait uses `role="img"` on its own element (§9). D.07: one delegated `document` listener each for `contextmenu` and `dragstart` calls `preventDefault()` when `e.target.closest && e.target.closest(".cv-hero, .gl-cover")`. A deterrent only; the menu works elsewhere.

#### 7.3.9 Pitfalls

- **MUST NOT** register `--cut` with `@property`. Why: it would resolve once, on `.cover`, against the wrong size.
- The cap is load-bearing. Why: `100vw` keeps growing past `--max`; uncapped, 2560px gives 313px.
- Keep the 3% slack. Why: `100vw` includes a classic scrollbar (≈ 8px).
- Below ≈ 1000px the rounded corner clips a glyph sliver the straight echo clip does not restore. It is invisible; leave it.

#### 7.3.10 Adapting: one line at every width

```css
.cover .mast { white-space: nowrap;
  font-size: min(clamp(36px, {slope}vw, {cap}px), calc((100vw - 2 * var(--gut) - 2 * var(--hp)) / {K})); }
```

The clamp arm is the designed size; the width arm is a ceiling, content width ÷ name width in em. In the fluid range content = 100 − 2 × 4.4 − 2 × 2.6 = 86vw, so slope `86 / K` keeps the arms level. The width arm wins on phones (clamped gutters), the cap on wide screens.

1. After `document.fonts.ready`, append a clone of the final `h1` with `position:absolute; visibility:hidden; margin:0; font-size:100px; white-space:nowrap`; `W = getBoundingClientRect().width / 100`. Estimate: `W ≈ 0.38 ×` characters incl. spaces.
2. `K = W × 1.03`, rounded up to 0.01. `slope = 86 / K`, rounded up to 0.1.
3. `cap = min(floor(slope × 16.9), floor(1500 / W), 217)` px: engages at ≈ 1690px; fits the content width at `--max`; keeps a short name's line box ≤ 35% of the 560px card. The 36px floor stays.
4. Check `W × used size ≤ content width` at 300, 390, 820, 1440, 1920 and 2560.

The values in 7.3.3 come from W = 7.25: K 7.45, 11.6vw, 196px.

**Worked example.** Placeholder `Xxxxx Yyyyyy Zzzzz` (18 characters, italic middle word, estimate 6.84em) measures **W = 6.86**. K = 7.0658 → **7.07**, slope 12.16 → **12.2vw**, cap = min(206, 218, 217) = **206px**:

```css
font-size: min(clamp(36px, 12.2vw, 206px), calc((100vw - 2 * var(--gut) - 2 * var(--hp)) / 7.07));
```

| Viewport | 300 | 390 | 1440 | 2560 |
|---|---|---|---|---|
| Content width | 240 | 327.7 | 1253.3 | 1620 |
| Clamp / width arm | 36.6 / 33.9 | 47.6 / 46.3 | 175.7 / 177.3 | 206 / 329.8 |
| Used → name width | 33.9 → 233 | 46.3 → 318 | 175.7 → 1205 | 206 → 1413 |

Above 21 characters the name drops below 28px at 300px; drop a middle name from `nameParts` (§3.2).

**Hand-set words.** Keep the 7.3.3 tracking (the 0.07em before `.m2` clears the italic lean). Where letters touch or gap, adjust in 0.005em steps within −0.018 … −0.05em, then re-measure `W`. At most one part is italic (§3.2).

**Photo.** Wide: `50% {y}`; ≤ 620px: `{{portrait.focal}}`. The defaults `50% 42%` / `72% 42%` suit a face in the right third.

### 7.4 The opening line (`section.opening`)

#### 7.4.1 Purpose

The thesis, the largest sentence on the page, on two near-equal lines: a tight first word, one stamped claim word, an italic last word on a sand brush.

#### 7.4.2 Anatomy

```html
<section class="opening wrap" aria-label="Opening line">
  <p class="hl" data-fly><span class="tt">{first word}</span> {words} <span class="ink">{{opening.inkWord}}</span> {words}&nbsp;<em class="brush">{{opening.brushWord}}</em></p>
</section>
```

The `{…}` fragments come from `{{opening.line}}`. The line is a `<p>`. Why: the `h1` is the name; this line is voice, not structure.

#### 7.4.3 Spec (B.08)

```css
.opening { padding-top: clamp(40px, 7vw, 120px); }
.hl { font: 400 clamp(40px, 7.2vw, 132px)/0.9 var(--serif); letter-spacing: -0.035em; margin: 0; max-width: 12.5ch; }
.hl .ink { letter-spacing: -0.02em; }
.hl .tt { letter-spacing: -0.055em; }
.hl em { letter-spacing: -0.01em; padding-right: 0.04em; }
.opening .hl { margin-left: clamp(0px, 4.5vw, 96px); }
@media (max-width: 620px) { .hl { font-size: clamp(46px, 14.6vw, 66px); } }
```

`.ink` and `.brush` are the §5 marks. `.hl em` beats `.brush` on `padding-right`: `0 0.04em 0 0.05em`. The ink word is tracked looser because the stamp widens its glyphs (2.3px at 103.7px).

#### 7.4.4 Responsive

Size 56.9px at 390, 66 at 620, 44.7 at 621, 59 at 820, 103.7 at 1440, 132 from 1833; padding-top 40 → 120px; indent 17.6 → 96px. The drop at 620/621 is deliberate. Why: a phone gets a poster that fills the width (x 35 → 335 at 390); wider screens give the line the left ≈ 40% and leave paper (x 128 → 680 at 1440). The indent breaks the frame line shared by bar and card. Dark: the brush is a darker translucent sand.

#### 7.4.5 Motion

A `data-fly` arrival (§16): hidden under `html.js` until `fly-ready`, with a 2.5s failsafe.

#### 7.4.6 Accessibility

Ink on paper: 14.8:1. The stamp drops in `forced-colors` and in selection (§5.6.2). The brush is a background, unseen by assistive tech.

#### 7.4.7 Pitfalls

**MUST NOT** set `font-weight` on the claim word. Why: the serif has one weight under `font-synthesis: none`; the stamp is the bold. Use exactly one `.ink`, one `.brush` (italic last word) and one `.tt` (first word).

#### 7.4.8 Adapting

`12.5ch` suits a 7-word, 35-character line breaking just before the ink word (lines ≈ 4.5em and 5.2em at 1440). Check 1440, 820, 390: lower in 0.5ch steps if line 1 takes the ink word, raise if a third line appears; if nothing works, put a `<br>` before the ink word.

---

## 8 · Orbit — the pinned stage and the contents

The signature object (fig. 0, §15) stays pinned mid-screen while the lede, two margin notes and the contents stops scroll past in two side lanes. No absolute positioning, no layout script. Object hooks use the `sig` prefix.

### 8.1 Purpose

Hand the reader the object, and pass the contents as stops on the way down rather than as a block to skip. The middle column belongs to the object; no word ever crosses it.

### 8.2 Anatomy

```html
<section class="orbit wrap" id="orbit" aria-label="The {{orbit.signatureObject}}, and the contents">
  <div class="orbit-pin"><!-- stage -->
    <figure class="sig" id="sigFig">
      <div class="sig-view" id="sigHost"><canvas id="sigCanvas" tabindex="0" role="img"
        aria-label="Interactive 3D {{orbit.signatureObject}}. Drag or use the arrow keys to turn it; Q and E roll it."></canvas></div>
      <div class="sig-ui" id="sigUi">
        <div class="sig-ctrls"><!-- slider rows (§15); D.08 wraps them in details.dock "Adjust", closed (§12) --></div>
        <div class="sig-foot">
          <span class="sig-status" id="sigStatus" aria-live="polite"></span>
          <button class="btn" id="sigReset" type="button">Reset</button>
        </div>
      </div>
      <figcaption class="note"><span>{{orbit.aside[0]}}</span><span>{{orbit.aside[1]}}</span></figcaption>
    </figure>
  </div>
  <div class="orbit-flow"><!-- text layer -->
    <p class="stand o-r" data-fly style="--mt:4svh">{{orbit.lede}} … <b>{{orbit.keyPhrase}}</b> …</p>
    <p class="o-hand o-l o-in" data-fly style="--mt:20svh">
      <span class="hand">{{orbit.aside[0]}} &mdash;<br>{{orbit.aside[1]}}</span>
      <svg viewBox="0 0 160 64" aria-hidden="true" focusable="false">
        <path d="M6 5c8 19 30 35 62 41 27 5 55 1 79-10"/><path d="M131 27c6 3 12 6 17 9-5 4-11 8-16 12"/></svg>
    </p>
    <p class="sig-note o-r o-in" data-fly style="--mt:12svh"><span class="note">fig. 0 &mdash; {{orbit.signatureObject}}</span>{{orbit.figNote}}</p>
    <nav class="inside" id="inside" aria-label="Contents">
      <span class="lab o-l" data-fly style="--mt:26svh">In this issue</span>
      <a class="o-l" data-fly href="#f1" style="--mt:8svh"><span class="n">{{features[0].no}}</span><span class="t">{{features[0].headline}}</span></a>
      <a class="o-r o-in" data-fly href="#f2" style="--mt:6svh"><span class="n">{{features[1].no}}</span><span class="t">{{features[1].headline}}</span></a>
      <!-- stops 03 … N: classes and --mt from 8.3.3 -->
    </nav>
  </div>
</section>
```

- `aside[0]`/`[1]` are `orbit.aside` split at its em dash; aside and figcaption carry the same words, one visible per layout. `.t` is the headline as plain text (§3).
- DOM: stage, then text. The stage holds no prose, so reading order is the zig-zag order.
- `.o-l`/`.o-r` pick the lane; `.o-in` hugs the inner edge (beside the object). Only `.o-l.o-in` has CSS; on `.o-r` it marks intent.

### 8.3 Spec

#### 8.3.1 Frame and lanes (B.09; one declaration per line in the file, §2.4)

```css
.orbit { --objW: clamp(360px, 33vw, 620px); --og: clamp(28px, 3.6vw, 72px);
  --laneW: calc((100% - var(--objW)) / 2 - var(--og)); position: relative; display: grid; }
.orbit > .orbit-flow, .orbit > .orbit-pin { grid-area: 1/1; }
.orbit-flow { position: relative; z-index: 2; display: flex; flex-direction: column; align-items: flex-start;
  padding: 6svh 0 36svh; pointer-events: none; }
.orbit-flow > *, .orbit-flow .inside > * { pointer-events: auto; width: var(--laneW); margin-top: var(--mt, 12svh); }
.orbit-flow > .inside { display: flex; flex-direction: column; align-items: flex-start; align-self: stretch;
  width: auto; margin-top: 0; pointer-events: none; }
.o-l { align-self: flex-start; }
.o-r { align-self: flex-end; }
.o-in.o-l { text-align: right; }
.o-in.o-l .n { margin-left: auto; }
.orbit-pin { position: sticky; top: 54px; height: calc(100svh - 54px); align-self: start; z-index: 3;
  display: flex; align-items: center; justify-content: center; pointer-events: none; }
@media (max-width: 1279px) {
  .orbit { --objW: clamp(300px, 29vw, 380px); --og: clamp(24px, 2.6vw, 40px); }
  .orbit .stand { font-size: 18px; }
  .orbit .sig-ctrls output { white-space: nowrap; }
}
```

One cell, two layers: the in-flow text layer sets the height, so the sticky stage travels exactly the section. `--objW` is the object column, `--og` the clear gap beside it, `54px` the bar's inner height (§7). The orbit uses `.wrap`, not `.g` (§6).

| Viewport | 1100 | 1279 | 1280 | 1440 | 1920 |
|---|---|---|---|---|---|
| `--objW` / `--og` | 319 / 28.6 | 370.9 / 33.3 | 422.4 / 46.1 | 475.2 / 51.8 | 620 / 69.1 |
| `--laneW` | 313.5 | 364.5 | 326.4 | 367.2 | 460.9 |

Why the step at 1280: the narrower column below it keeps lanes ≥ ≈ 310px, a readable measure.

#### 8.3.2 Pointer-events and stacking

| Layer | Position · `z-index` | `pointer-events` |
|---|---|---|
| `.bar` | sticky 0 · 20 | auto |
| `.orbit-pin` (full width) › `.sig` › open `.dock-b` | sticky 54px · 3 › static › absolute · 3 | **none** › **auto** › auto |
| `.orbit-flow` › `nav.inside` › each lane item and stop | relative · 2 › static › static | **none** › **none** › **auto** |

Both full-size layers **MUST** be `pointer-events: none`, re-enabled only on the figure and each word block; the stage **MUST** be on top. Why: the canvas drags wherever drawn, lane clicks fall through the empty stage, the open drawer floats over words, and arrivals (§16) emerge from behind the object. `.orbit` has no `z-index`, so the stage stays under the bar.

#### 8.3.3 The zig-zag

One flex column in reading order; each item goes to a lane with its own top margin in `svh`, irregular so it reads hand-placed.

| Item | Lane | `--mt` |
|---|---|---|
| layer `padding-top` | — | 6svh |
| lede · aside · fig. 0 note | R · L-in · R | 4 · 20 · 12 svh |
| label `.lab` | L-out | **26svh** (longest pause) |
| stops 01 – 07 | L-out · R+in · L-in · R · L-out · R+in · L-in | 8 · 6 · 16 · 12 · 18 · 9 · 15 svh |
| layer `padding-bottom`: the runway | — | **36svh** |

The runway lets the last stop pass the object's middle before release, so the next section's top margin is short while pinned (§6). Why `svh`: a similar number of stops in view on any screen, and a collapsing mobile toolbar moves nothing. At 1440 × 900: ≈ 2.5 screens tall, pinned for ≈ 1.5.

#### 8.3.4 Margin notes

```css
.stand { font: 300 clamp(18px, 1.45vw, 22px)/1.55 var(--sans); color: var(--muted); margin: 0; }
.stand b { font-weight: 400; color: var(--ink); }
.orbit .stand, .sig-note { text-wrap: pretty; }
.o-hand { position: relative; margin: 0; left: calc(var(--og) - 12px); }
.o-hand .hand { display: block; transform: rotate(-4deg); }
.o-hand svg { display: block; width: clamp(110px, 10vw, 170px); margin: 6px 0 0 auto; overflow: visible; }
.o-hand path { fill: none; stroke: var(--accent); stroke-width: 1.6; stroke-linecap: round; stroke-linejoin: round; }
.sig-note { margin: 0; font: 400 15px/1.6 var(--sans); color: var(--muted); }
.sig-note .note { display: block; margin-bottom: 8px; color: var(--ink); }
@media (max-width: 620px) { .sig-note { font-size: 14px; } }
```

- **Lede**: ≈ 21px at 1440, five lines, muted; the key phrase in ink at 400. Why: emphasis by one step of value, not bold (§5).
- **Aside**: Caveat in `--accent-ink` (`.hand`, §5), −4°, right-aligned, shifted `--og − 12px` into the gap so the arrow tip stops 12px short of the object. Arrow: a sagging shaft rising right plus a separate curved chevron, one stroke and a flick.
- **fig. 0 note**: a museum label, lowercase mono heading in ink over muted sans. Object 0 leaves Fig. 1 onward to the features.

#### 8.3.5 Contents stops

```css
.inside a { text-decoration: none; text-wrap: balance; }
.inside .n { display: block; width: max-content; font: 400 12px/1 var(--mono); letter-spacing: 0.04em;
  color: var(--muted); margin-bottom: 10px;
  transition: color 0.25s, transform 0.45s cubic-bezier(0.34, 1.56, 0.64, 1); }
.inside .t { font: 400 clamp(24px, 2.3vw, 40px)/1.12 var(--serif); letter-spacing: -0.014em; text-wrap: balance;
  background: var(--pencil) no-repeat 0 100%/0 0.32em; padding-bottom: 0.18em;
  transition: background-size 0.45s cubic-bezier(0.3, 0.9, 0.3, 1.15); }
.inside a:hover .t, .inside a:focus-visible .t { background-size: 100% 0.32em; }
.inside .lab { text-wrap: balance; }
@media (hover: hover) { .inside a:hover .n { transform: translateX(4px) rotate(-4deg); } }
.inside a:hover .n { color: var(--accent-ink); } /* in B.23 with the other accent-ink rules (§4) */
```

Mono number over serif title (33px at 1440, the section's largest type). Left stops alternate outer edge (01, 05) and inner edge (03, 07); right stops always hug the object. Inner-left: title right-aligned, number pushed right by `margin-left: auto`. Why `max-content`: a full-width block ignores the auto margin.

#### 8.3.6 Object column and control bar (base rules in §15)

```css
.orbit .sig { width: var(--objW); display: flex; flex-direction: column; align-items: center; }
.orbit .sig-view { width: min(100%, calc((100svh - 250px) * 10 / 9)); transform-origin: 50% 100%;
  transition: transform 0.55s cubic-bezier(0.3, 1.15, 0.5, 1); }
.orbit .sig-ui { order: -1; display: flex; align-items: center; gap: 10px 14px; flex-wrap: wrap; margin: 0 0 6px; }
.orbit .sig figcaption { display: none; }
.orbit .sig-ui .dock { margin: 0; flex: none; }
.orbit .sig-ui .dock > summary { min-height: 40px; padding: 8px 14px; }
.orbit .sig-ui .dock > summary .dock-s { display: none; }
.orbit .sig-foot { flex: 1 1 200px; gap: 12px; }
.orbit .sig-status { font-size: 13.5px; line-height: 1.35; }
.orbit .sig-foot .btn { padding: 10px 18px; }
.orbit .sig-ui .dock[open] > .dock-b { position: absolute; left: 0; right: 0; top: calc(100% + 6px); z-index: 3;
  padding: 16px; border-radius: var(--r, 16px); background: var(--surf, var(--paper)); box-shadow: var(--raise-s, none); }
@media (prefers-reduced-motion: reduce) { .orbit .sig-view { transition: none; } }
```

The 10/9 view is never taller than `100svh − 250px`, so object and bars fit (475 × 428 at 1440 × 900). Control bar, one row ≤ 440px: raised **Adjust** pill (accent three-line glyph, bold label, chevron; no value summary) · growing status line in light sans, which wraps to two lines first · raised **Reset**. `.sig-ui` is `position: relative` (§15), the drawer's anchor; drawer, sliders, button: §12.

### 8.4 States

- Stop: fine-pointer hover = `--accent-ink` number + tip + stroke; touch = colour + stroke; `:focus-visible` = stroke + global ring (§6).
- Object: untouched, the scroll turns it (8.6); after the first manual input it stays put until **Reset**. No WebGL: §15. Dark: tokens only (§4).
- Drawer: starts closed; opened while pinned, the object makes room.

**The object makes room** (D.09; runs after D.08, whose `.dock` it needs):

```text
PIN = matchMedia("(min-width: 1100px) and (min-height: 680px)")
fit():  // on details toggle, PIN change, ResizeObserver(.orbit-pin)
  if !(dock.open && PIN) → view.style.transform = ""; return
  top = view.offsetTop; h = view.offsetHeight  // relative to the stage
  need = ui.offsetTop + ui.offsetHeight + 6 + panel.offsetHeight + 12 − top  // 6 gap, 12 clearance
  if need ≤ 0 → clear; return
  shift = min(need, max(0, stage.clientHeight − (top + h) − 8))  // move down first, keep 8px
  rest = need − shift
  sc = rest > 0 ? max(0.6, 1 − rest / h) : 1  // then shrink, never < 60%
  view.style.transform = "translateY(" + shift + "px) scale(" + sc.toFixed(3) + ")"
Escape on the details → close, focus the summary
pointerdown outside the stage while open and PIN → close
```

With origin `50% 100%`, scaling lowers the top edge by exactly `h·(1 − sc)`. Why offset values: they ignore transforms, so `fit()` is idempotent and cannot loop with the observer.

### 8.5 Responsive

One column applies at `(max-width: 1099px), (max-height: 679px)`, the exact complement of PIN. Why the height term: under 680px tall, bar, control bar and object no longer fit one screen. Short landscape and print: §15, §17.

```css
@media (max-width: 1099px), (max-height: 679px) {
  .orbit { display: flex; flex-direction: column; }
  .orbit-flow { display: contents; }
  .orbit-flow > *, .orbit-flow .inside > * { margin-top: 0; width: auto; }
  .orbit-pin { position: relative; top: auto; height: auto; order: 2; margin: clamp(26px, 6vw, 44px) 0 0;
    pointer-events: auto; align-self: stretch; }
  .orbit .stand { order: 1; margin-top: clamp(18px, 4vw, 30px); max-width: 40ch; }
  .orbit .sig { width: 100%; max-width: 560px; margin: 0 auto; }
  .orbit .sig-view { width: 100%; }
  .orbit .sig-ui { order: 0; margin: 6px 0 0; }
  .orbit .sig figcaption { display: flex; }
  .orbit .sig-ui .dock[open] { flex: 1 1 100%; }
  .orbit .sig-ui .dock[open] > .dock-b { position: static; padding: 4px 16px 16px; box-shadow: none; background: none; }
  .o-hand { display: none; }
  .o-in.o-l { text-align: left; }
  .orbit .o-l, .orbit .o-r { align-self: auto; }
  .o-in.o-l .n { margin-left: 0; }
  .sig-note { order: 3; margin-top: 14px; max-width: 560px; }
  .orbit-flow .inside { order: 4; display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 22px 18px;
    margin-top: clamp(36px, 8vw, 64px); padding-top: 16px; border-top: 1px solid var(--ink); }
  .orbit-flow .inside > .lab { grid-column: 1/-1; }
  .inside .t { font-size: clamp(21px, 3.4vw, 26px); }
}
@media (max-width: 620px) { .orbit-flow .inside { grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 20px 16px; } }
@media (max-width: 300px) { .orbit-flow .inside { grid-template-columns: minmax(0, 1fr); } }
```

`display: contents` drops the text layer's box, so its children join the section's flex column and `order` interleaves them: lede (1) → stage (2: object ≤ 560px, control bar below, figcaption halves at the ends, centred by `align-items`) → fig. 0 note (3) → contents (4) under a 1px ink rule, 4/2/1 columns, stops left-aligned. An open drawer takes the row; its in-flow panel joins the raised pill and stays open while scrolling.

### 8.6 Motion

Hover, focus and make-room transitions are in the CSS above; reduced motion removes the make-room one. Lane items arrive from the screen centre on a spring (§16). The idle turn (yaw ±12°, maths in §15):

```text
each frame, if !held && !dragging && PIN && !reducedMotion:
  t = clamp((scrollY − sectionTop) / max(1, sectionHeight − innerHeight), 0, 1)  // 1 = stage releases
  yaw += ((t − 0.5) × 24 − yaw) × min(1, dt × 4)  // τ ≈ 0.25 s
first manual input: rot.y = wrap(rot.y + yaw); yaw = 0  // folds the turn in, no jump
Reset: back to REST; the yaw eases back to the scroll target
```

Section metrics come from a `ResizeObserver`; `scrollY` is read in rAF; there is no scroll listener. Why ±12°: the object breathes as words pass; it does not spin.

### 8.7 Accessibility

- A real `<nav aria-label="Contents">` of plain links; the bar's contents link targets `#inside` (`scroll-padding-top`, §6).
- Canvas: focusable `role="img"` labelled with every control; keys match dragging (§15). Drawer: native `<details>`; Escape closes it and refocuses the summary.
- `#sigStatus` is `aria-live="polite"`, written only when the sentence changes. Why: it refreshes 4× a second; identical rewrites would re-announce.
- One drag hint at a time; the arrow is `aria-hidden`, the handwriting is read. Tab order: canvas → Adjust → Reset → stops.

### 8.8 Pitfalls

1. **Sticky in a grid cell needs `align-self: start`**; overlap by `grid-area: 1/1`, never `position: absolute`. Why: stretched, the stage cannot stick; the in-flow text layer must size the section.
2. **`100%` in `--laneW` resolves where it is used**: stops resolve it against `nav.inside`, so the nav **MUST** stretch, and `.orbit-flow > .inside` (0,2,0) **MUST** undo the width, margin and `pointer-events` that `.orbit-flow > *` gives it.
3. **`54px` appears twice** and equals the bar's inner height; the bar stays 99% opaque. Why: under a fully opaque sticky layer Chrome culls the pinned canvas and the object vanishes early (§7).
4. **The PIN query lives in three places** (the CSS complement, the idle turn, D.09): change them together.
5. **`display: contents` drops the runway padding**; in one column the next section keeps its full top margin.

### 8.9 Adapting to new content

- **N stops**: stop k goes left if odd, right if even; left is inner when k mod 4 = 3; right takes `.o-in` when k mod 4 = 2. Each `--mt` 6–18svh, never equal twice in a row; keep the ≥ 20svh pauses before aside and label and the 36svh runway. Pinned travel (section height − viewport) **MUST** be ≥ one screen; with fewer stops, raise the gaps.
- **Text**: ≈ 25 title characters per line at 1440; if most titles exceed ≈ 35, lower the title maximum to ≈ 34px. Lede ≤ 6 lines (≈ 45 words); aside two short lines, arrow aiming right; fig. 0 note 2–4 sentences.
- **Object proportions**: change `10/9` and `* 10 / 9` together; a wider object may take more vw only while `--laneW` stays ≥ ≈ 310px from 1100 to 1280 (8.3.1).

---

## 9 · At a glance & the letter

Parts B.10 / C.05 and B.11 / C.06; both sections are `.g` grids (§6.3), top margins in §6.4.

### 9.1 The glance card (`section#glance`)

#### 9.1.1 Purpose

One flat sheet bleeding off the right edge. On phones the bar keeps only the `#glance` link (§7.1): this is the phone reader's main stop.

#### 9.1.2 Anatomy

```html
<section class="glance g" id="glance" aria-labelledby="glH">
  <div class="gl-top" data-fly="rise"><span class="lab">{{glance.kicker}}</span><span class="gl-time">{{glance.aside}}</span></div>
  <div class="gl-card" data-fly="rise">
    <div class="gl-cover" role="img" aria-label="{{portrait.alt}}"></div>
    <div class="gl-body">
      <div class="gl-id">
        <h2 id="glH" class="gl-name">{given part of {{fullName}}} <span class="brush">{family part}</span></h2>
        <p class="gl-role">{{role}} <i aria-hidden="true">·</i> {{location}} <i aria-hidden="true">·</i> {{workMode}}</p>
        <p class="gl-line">{{pitch}}</p>
        <div class="gl-acts"><a class="btn solid" href="mailto:{{email}}">{{glance.actions.mail}}</a>
          <a class="btn" href="{{links[0].url}}" rel="noopener">{{glance.actions.link}}</a>
          <a class="btn gl-go" href="#f1">{{glance.actions.work}}</a></div>
      </div>
      <dl class="gl-facts"><!-- ×6; <b> only where a numeral leads -->
        <div><dt>{{glance.facts[i].label}}</dt><dd><b>{{glance.facts[i].numeral}}</b> {{glance.facts[i].value}}</dd></div>
      </dl>
      <div class="gl-stack" role="group" aria-label="Stack">
        <div class="gl-grp"><span class="lab" id="glS1">{{glance.stack[0].label}}</span><ul aria-labelledby="glS1"><li>{{glance.stack[0].items[k]}}</li></ul></div>
        <!-- one .gl-grp per group; the last adds .now when its now is true -->
      </div>
    </div>
  </div>
</section>
```

The person's `h2` names the region. DOM order never changes; the actions column is placement only. The `glS…` ids are an.

#### 9.1.3 Spec (B.10)

Order inside B.10: placement → card → ≤ 999 → ≤ 640 → ≤ 380 → ≤ 300 → short landscape → facts on hairlines (with its own ≤ 640, ≤ 300). Why: equal-weight rules resolve by order.

**Placement, kicker, bleed.**

```css
.gl-top { display: flex; justify-content: space-between; align-items: baseline; gap: 12px; border-top: 1px solid var(--ink); padding-top: 14px; }
.gl-top .lab { font: 400 14px/1.3 var(--sans); letter-spacing: 0; text-transform: none; color: var(--ink); }
.gl-time { font: italic 400 19px/1.15 var(--serif); color: var(--muted); transform: rotate(-2.5deg); transform-origin: 100% 100%; }
@media (min-width: 1280px) {
  .gl-top { grid-column: col 1 / col 3; grid-row: 1; flex-direction: column; justify-content: flex-start; gap: 16px; border-top: 0; padding-top: clamp(18px, 2vw, 30px); }
  .gl-time { max-width: 7.5em; transform-origin: 0 100%; }
  .glance .gl-card { grid-column: col 3 / bleed-end; grid-row: 1; margin-top: 0; border-radius: 28px 0 0 28px; }
  .glance .gl-body { padding-right: calc(max(var(--gut), (100vw - var(--max)) / 2) + clamp(16px, 3vw, 44px)); }
}
@media (min-width: 761px) and (max-width: 1279px) {
  .glance .gl-card { grid-column: col 1 / bleed-end; border-radius: 28px 0 0 28px; }
  .glance .gl-body { padding-right: calc(var(--gut) + clamp(16px, 3vw, 40px)); }
}
@media (max-width: 760px) {
  .glance .gl-card { grid-column: col 1 / bleed-end; border-radius: 24px 0 0 24px; }
  .glance .gl-body { padding-right: calc(var(--gut) + 16px); }
}
```

- **≤ 1279**: kicker row on a full-ink rule; the italic aside flush right, tilted −2.5° about its bottom-right corner to stay on the rule's end.
- **≥ 1280, margin note**: no rule; kicker and aside stack in columns 1–2, the aside capped at two lines and pivoting bottom-left. `grid-row: 1` on both keeps them side by side.
- **Bleed**: `col-end` → screen edge = `max(--gut, (100vw − --max) / 2)` (just `--gut` below the ≈ 1711px cap), so content sits `x` inside the frame, mirroring the left padding. The `max()` is an. Why: plain `gut + x` overshoots by 36px at 1920, 356px at 2560; the scrollbar in `100vw` (≈ 8px) is invisible.

**Card and band.**

```css
.gl-card { --surf: var(--paper2); margin-top: clamp(16px, 2.6vw, 24px); background: var(--paper2); overflow: hidden; }
.gl-cover { position: relative; aspect-ratio: 3.4/1; background: var(--me) 50% 40% / cover no-repeat, var(--paper);
  -webkit-user-select: none; user-select: none; -webkit-touch-callout: none; }
.gl-cover::after { content: ""; position: absolute; inset: 0;
  background: linear-gradient(180deg, transparent 62%, color-mix(in srgb, var(--paper2) 70%, transparent) 92%, var(--paper2)); }
.gl-body { display: grid; grid-template-areas: "id" "facts" "stack"; gap: clamp(22px, 2.6vw, 30px);
  padding: clamp(4px, 1vw, 10px) clamp(16px, 3.4vw, 40px) clamp(16px, 3.4vw, 40px); position: relative; margin-top: -28px; }
```

- Flat `--paper2`, no grain, border or shadow: on grainy paper (§4.5) it reads as a pressed sheet. `--surf` gives every moulded part inside the card's light (§4.6).
- The band reuses the cover's `--me` (§7.3, one download) on a `--paper` fallback. Ratios: 3.4 → 2.5 (≤ 999) → 1.75 (≤ 640) → 1.6 (≤ 380) → 3.4 in short landscape.
- The fade clears the top 62%. The body rises 28px onto it and, later in the DOM, paints over it without `z-index`.

**Identity.**

```css
.gl-id { grid-area: id; min-width: 0; display: grid; grid-template-columns: minmax(0, 1fr) auto; column-gap: 32px; align-items: end; }
.gl-id > * { grid-column: 1; }
.gl-id > .gl-acts { grid-column: 2; grid-row: 1 / span 3; justify-content: flex-end; margin: 0; flex-wrap: nowrap; }
.gl-name { font: 400 clamp(30px, 4.4vw, 58px)/1 var(--serif); letter-spacing: -0.02em; margin: 0; }
.gl-role { margin: 12px 0 0; font: 400 15px/1.5 var(--sans); letter-spacing: 0.005em; color: var(--muted); }
.gl-role i { font-style: normal; color: var(--sand-ink); padding: 0 4px; }
.gl-line { margin: 10px 0 0; font: 400 clamp(19px, 2vw, 24px)/1.3 var(--serif); max-width: 34ch; }
.gl-acts { display: flex; flex-wrap: wrap; gap: 12px; }
.gl-acts .btn { padding: 11px 20px; font-size: 14px; letter-spacing: 0.01em; }
.gl-go::after { content: " \2193"; content: " \2193" / ""; }
```

Sand brush under the family name (§5.7.1); dots U+00B7 in `--sand-ink` (4.56 : 1). The actions span the three text rows and `align-items: end` sets them on the pitch's last line. `.btn`: §12. The arrow is `--accent-ink` (B.23); its silent `/ ""` alternative is an.

**Facts on hairlines.**

```css
.gl-facts { grid-area: facts; margin: 0; display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); }
.gl-facts > div { min-width: 0; }
.gl-facts dt { color: var(--muted); }
.gl-facts dd { margin: 5px 0 0; font: 400 clamp(18px, 1.7vw, 22px)/1.2 var(--serif); letter-spacing: -0.005em; text-wrap: balance; }
.gl-facts b { font-weight: 400; color: var(--accent); font-size: 1.25em; line-height: 0.9; }
/* after the breakpoint blocks: */
.gl-facts { gap: 0 clamp(18px, 2.4vw, 34px); }
.gl-facts > div { padding: 12px 0 14px; border-top: 1px solid var(--hair); }
.gl-facts dt { font: 400 11.5px/1.4 var(--mono); letter-spacing: 0.02em; }
@media (max-width: 640px) { .gl-facts { gap: 0 16px; } .gl-facts > div { padding: 10px 0 11px; } }
@media (max-width: 300px) { .gl-facts > div { padding: 9px 0; } }
```

Each cell owns its hairline, so the rules break at the gaps; `row-gap: 0` lets row 2's rules close row 1. The numeral is `--accent` only because it is large (≈ 27px, §5.6.4).

**Stack and the "now" crescent.**

```css
.gl-stack { grid-area: stack; display: grid; grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.9fr) minmax(0, 1.3fr);
  gap: 24px; border-top: 1px solid var(--hair); padding-top: clamp(18px, 2.4vw, 26px); }
.gl-grp ul { list-style: none; margin: 12px 0 0; padding: 4px 2px 6px; display: flex; flex-wrap: wrap; gap: 12px 10px; }
.gl-grp li { font: 400 12.5px/1 var(--mono); letter-spacing: -0.005em; padding: 9px 14px; border-radius: 999px; background: var(--face); box-shadow: var(--raise-s); }
.gl-grp .lab { font: 400 11.5px/1.4 var(--mono); letter-spacing: 0.02em; text-transform: lowercase; }
.gl-grp.now .lab { color: var(--sand-ink); }
.gl-grp.now li {
  position: relative;
  isolation: isolate;
  background: linear-gradient(180deg, color-mix(in srgb, var(--sand), #fff 18%), color-mix(in srgb, var(--sand), #000 12%));
}
.gl-grp.now li::before { content: ""; position: absolute; inset: 0 0 0 4px; z-index: -1; border-radius: inherit; background: var(--face); }
```

- One continuous hairline answers the facts' broken ones. Pills are card-coloured; only `--raise-s` lifts them.
- **Crescent**: `::before` is the same pill 4px to the right, in `--face`, behind the text. Cap minus cap leaves sand 4px wide at mid-height, tapering to 0; the right caps coincide. The gradient lights it from above.

#### 9.1.4 States

- **Buttons** (§4.6.5): primary ink, hover `--accent`; secondaries' text eases to `--accent-ink`; pressed `--sink`. Pills are static.
- **Dark**: tokens only; card `#25231f`, the primary turns a light pill.
- **No portrait** (§3.7): omit `.gl-cover`; `.gl-body { margin-top: 0; padding-top: clamp(16px, 3.4vw, 40px); }`.
- **Print**: actions hidden, card background none (§17).

#### 9.1.5 Responsive

```css
@media (max-width: 999px) {
  .gl-cover { aspect-ratio: 2.5/1; } .gl-id { display: block; }
  .gl-id > .gl-acts { margin-top: 18px; flex-wrap: wrap; justify-content: flex-start; }
  .gl-line { max-width: 46ch; }
  .gl-stack { grid-template-columns: minmax(0, 1fr); gap: 14px; }
  .gl-grp { display: grid; grid-template-columns: 150px minmax(0, 1fr); gap: 12px; align-items: baseline; }
  .gl-grp ul { margin: 0; }
}
@media (max-width: 640px) {
  .gl-cover { aspect-ratio: 1.75/1; }
  .gl-body { padding: 0 16px 16px; margin-top: -18px; gap: 18px; }
  .gl-name { font-size: clamp(27px, 8vw, 34px); }
  .gl-role { margin-top: 6px; font-size: 13.5px; line-height: 1.45; } .gl-role i { padding: 0 2px; }
  .gl-line { font-size: 19px; max-width: none; }
  .gl-acts { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 10px; }
  .gl-acts .btn { padding: 12px 6px; font-size: 13.5px; text-align: center; white-space: nowrap; }
  .gl-facts { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .gl-facts dd { margin-top: 4px; font-size: 17.5px; }
  .gl-stack { padding-top: 16px; } .gl-grp { display: block; }
  .gl-grp ul { margin-top: 8px; gap: 10px 8px; }
  .gl-grp li { font-size: 12px; padding: 8px 11px; }
}
@media (max-width: 380px) {
  .gl-time { display: none; } .gl-cover { aspect-ratio: 1.6/1; } .gl-body { padding: 0 14px 14px; }
  .gl-acts { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .gl-acts .btn.solid { grid-column: 1/-1; }
}
@media (max-width: 300px) {
  .gl-body { padding: 0 12px 12px; margin-top: -10px; }
  .gl-name, .gl-role { text-align: center; } .gl-acts { grid-template-columns: minmax(0, 1fr); }
  .gl-facts { grid-template-columns: minmax(0, 1fr); background: var(--paper); border-radius: 14px; padding: 2px 12px; }
  .gl-facts > div:first-child { border-top: 0; }
}
@media (max-height: 520px) and (orientation: landscape) { .gl-cover { aspect-ratio: 3.4/1; } }
```

Reflow: actions right column → under the pitch (≤ 999) → 3 cells (≤ 640) → primary on its own row (≤ 380) → 1 column (≤ 300); facts 3 → 2 → 1 in a `--paper` well (sunk in both themes); stack 3 columns → label rows → label above pills. Acceptance at 1440 (±1px): card 286.4 → 1440, band 339px, content 326 → 1333.4.

#### 9.1.6 Motion

`data-fly="rise"` on `.gl-top` and `.gl-card`: a short rise, no sideways travel (§16). Buttons ease `box-shadow` and `color` 0.2s.

#### 9.1.7 Accessibility

Band: `role="img"`, `{{portrait.alt}}`, guarded like the cover (§7.3.8). Sand (3.04 : 1) is marks only; small sand text is `--sand-ink`; "now" is also said by its label. The profile link stays in the tab (`rel="noopener"`, no `target`).

#### 9.1.8 Pitfalls

1. **Keep the padding split.** Phone `.gl-body { padding }` is (0,1,0); `.glance .gl-body { padding-right }` (0,2,0) wins. Why: otherwise content hits the screen edge.
2. **No `isolation`, no crescent**: `position: relative` alone makes no stacking context, so the `z-index: -1` face falls behind the card.
3. **No `z-index`, no absolute body**: in flow, the card's height stays right.

#### 9.1.9 Adapting to new content

| Value | Re-tune |
|---|---|
| `.gl-name` | ≈ 0.38em per character; keep `0.38 × chars × size` ≤ body − actions − 32px; 25+ characters → `clamp(30px, 3.6vw, 48px)` |
| `.gl-line` | 60–85 characters at `34ch`; shorter → `28ch` |
| `.gl-time` | `max-width ≈ (chars ÷ 2) × 0.5em` |
| stack | frs ∝ pill characters per group, or `1fr 1fr 1fr`; label column ≥ longest label + 24px |
| band | `50% {y of {{portrait.focal}}}`; face above the bottom 38% at 3.4 : 1 |

### 9.2 The letter (`section#letter`)

#### 9.2.1 Purpose

A note before the work: a serif statement with two hand-set words, two columns under a drop cap, a taped print awaiting a photo. Optional (§3.7).

#### 9.2.2 Anatomy

```html
<section class="letter g" id="letter" aria-labelledby="letH">
  <span class="lab" data-fly>{{letter.kicker}}</span>
  <h2 id="letH" data-fly>{words} <span class="ink wide">{{letter.stampWord}}</span> {words} <span class="loop">{{letter.loopWord}}</span> {words} {penultimate}&nbsp;{last}.</h2>
  <div class="snap snap-desk" data-fly=".6">
    <figure class="snap-in" style="--tilt:-2.4deg"><span class="snap-ph" aria-hidden="true"></span><figcaption>{{letter.photoSlot.caption}} &mdash; photo to come</figcaption></figure>
  </div>
  <div class="cols" data-fly=".55" data-fly-delay=".08">
    <p>{{letter.paragraphs[0]}}</p>
    <p>{… <strong>{{letter.strongPhrase}}</strong> …}</p>
    <!-- paragraphs[2], [3] -->
  </div>
</section>
```

Rows are explicit; no sign-off. A real photo replaces `span.snap-ph` with a lazy `<img>` (§10).

#### 9.2.3 Spec (B.11)

```css
.letter .lab { grid-row: 1; font: 400 15px/1.4 var(--sans); letter-spacing: 0; text-transform: none; }
#letH { grid-row: 2; font: 400 clamp(38px, 5.2vw, 96px)/1.04 var(--serif); letter-spacing: -0.022em; margin: 14px 0 0; text-wrap: balance; }
#letH .wide { letter-spacing: 0.022em; margin: 0 0.02em; }
.cols { grid-row: 3; margin-top: clamp(36px, 4.6vw, 76px); columns: 2; column-gap: calc(var(--cg) * 1.6); }
.cols p { margin: 0 0 1.1em; break-inside: avoid-column; text-wrap: pretty; }
.cols p:first-child::first-letter { font: 400 5.6em/0.78 var(--serif); float: left; padding: 0.06em 0.1em 0 0; color: var(--ink); }
@media (min-width: 1100px) {
  .letter .lab { grid-column: col 1 / col 5; } #letH { grid-column: col 1 / col 10; }
  .letter .cols { grid-column: col 6 / col-end; }
  .snap-desk { grid-column: col 2 / col 5; grid-row: 3; margin-top: clamp(90px, 10vw, 170px); }
}
@media (min-width: 761px) and (max-width: 1099px) {
  .letter .cols { grid-column: col 5 / col-end; columns: 1; }
  .snap-desk { grid-column: col 1 / col 5; grid-row: 3; margin-top: clamp(70px, 9vw, 120px); padding-right: 12px; }
}
@media (max-width: 760px) {
  .cols { columns: 1; } .snap-desk { grid-row: 4; justify-self: end; width: min(62%, 260px); margin: 26px 6px 0 0; }
}
@media (max-width: 300px) { .cols p:first-child::first-letter { font-size: 4.2em; } }
```

- **Statement**: under a muted kicker, ≈ 75px over columns 1–9 at 1440, ragged. One `.ink.wide` word, stamped (§5.6.2) and opened to +0.022em; one `.loop` word ringed (§5.7.1). `&nbsp;` backs up `balance`.
- **Columns**: two of 358px (≈ 55–60 characters) at 1440. `avoid-column` keeps paragraphs whole, so four balance two and two; `pretty` stops one-word last lines.
- **Drop cap**: 5.6em = 95.2px; `0.78 × 95.2` plus 0.06em padding ≈ 80px = 2.8 body lines of 28.56px, so it sits beside **three lines**, its top at cap height.
- **Emphasis**: one `<strong>`, 400, 1.5px sand underline offset 0.22em (§5.6.3).
- **Print** (§10): hatched cream print tilted −2.4°, tape counter-rotated `tilt × −1.6` = +3.84°. At ≥ 1100 it shares row 3 in columns 2–4, starting 144px down against the text's 66px: staggered, columns 1 and 5 empty.

#### 9.2.4 Responsive

Two columns appear only at ≥ 1100 although `columns: 2` is the base. Tablets: one column from `col 5`, print in cols 1–4. Phones: statement 38px (3 lines at 390), print after the text, right-aligned. Short landscape changes nothing; print hides the slot (§17).

#### 9.2.5 Motion

Kicker and statement `data-fly`; print `.6`; columns `.55`, delay `.08` (§16). The print's hover lift is off under reduced motion (§10).

#### 9.2.6 Accessibility

Named by `#letH`; hatch hidden, caption read. Stamp, ring and drop cap are pure CSS: heading and first word read whole.

#### 9.2.7 Pitfalls

1. **Both `columns: 1` overrides are required**: the base is 2.
2. **The first paragraph MUST start with a letter.** Why: `::first-letter` swallows a leading quote mark or digit.
3. **MUST NOT tilt `.snap`**: its arrival transform would erase the tilt; tilt `.snap-in`.
4. **Keep the 6px right margin ≤ 760.** Why: the tilted corner would touch the edge.

#### 9.2.8 Adapting to new content

- **Statement**: 6–10 words, ≈ 50 characters; one `.ink.wide`, one `.loop`, neither in the last two words. Longer: lower the 5.2vw slope; never pass `col 10`.
- **Body**: 3–5 paragraphs, four best; one `<strong>`.
- **Drop cap for N lines** at line-height L: `(N − 0.2) × L / 0.84` em, keeping `/0.78` and the paddings.
- **Tilt**: −1.5° to −3°, unlike the page's other slots.

---

## 10 · Feature articles & photo slots

Every case study is a feature article: seven parts, placed by one of seven templates at ≥ 1100px, stacked in reading order below. Owns B.12 and B.13. Trays: §12; arrivals: §16; headline fitting: §5.5, §5.9.

### 10.1 Purpose

A feature reads as a magazine spread. One anatomy keeps the issue coherent; a different composition per feature keeps neighbours from looking alike.

### 10.2 Anatomy

```html
<article class="feature g" id="fN" aria-labelledby="fNh">
  <span class="f-num" aria-hidden="true">{{features[i].no}}</span> <!-- L5 only, first child -->
  <div class="f-top" data-fly="rise">
    <span class="no">No. {{features[i].no}}</span> <!-- bare beside an aside; none on L5 -->
    <span class="say">{{features[i].aside}}</span> <!-- optional -->
    <span>{{features[i].field}}</span> <!-- always the last span -->
  </div>
  <h2 id="fNh" data-fly>Words <em>hinge</em> <span class="ink sq">noun</span></h2>
  <p class="lede" data-fly=".7">{{features[i].lede}}</p>
  <div class="f-text" data-fly=".5" data-fly-delay=".06"><p>{{features[i].body[0]}}</p><p>…</p></div>
  <aside class="side" data-fly=".5" data-fly-delay=".12">
    <dl><div class="row"><dt>{{features[i].details[j].label}}</dt><dd>{{features[i].details[j].value}}</dd></div></dl>
  </aside>
  <div class="snap snap-sketch" data-fly=".6">…</div> <!-- L6 only, §10.12 -->
  <div class="insert" id="figN" data-fly="rise">…</div> <!-- tray, §12; none for A0 -->
  <p class="quote" data-fly=".6">…</p> <!-- L3, L4, L5, L7 only, §10.5 -->
</article>
```

- **DOM order is the phone and tablet layout**: `f-num? → f-top → h2 → lede → f-text → side → snap? → insert? → quote?`. Rows are assigned only at ≥ 1100px.
- Every part **MUST** be a direct child of the article (a `.g`, §6.3); no wrapper `div`. Why: each part is placed on its own lines.
- `N` is the feature's position; in the blocks below, `#fN` is its id. Copy the template's whole block (tablet and phone too), replacing `N`.
- Segments (§3): `italic` → `<em>`, `ink` → `<span class="ink">`; hand-set classes: §5.5. Breaks: L2, L5, L6 one `<br class="d">` (≥ 1100 only); L4 a plain `<br>` (every width); L3 `.l1`/`.l2` (§5.5.4); L1, L7 none. Compounds go in `<span class="nw">`.

### 10.3 Spec: shared parts

```css
.f-top { display: flex; align-items: baseline; gap: 0.8em; border-top: 1px solid var(--ink); padding-top: 14px;
  font: 400 13px/1.4 var(--sans); letter-spacing: 0.005em; color: var(--muted); }
.f-top > span:last-child { margin-left: auto; }
.f-top .no { font: 400 12px/1.4 var(--mono); letter-spacing: 0.02em; color: var(--ink); }
.f-top .say { font: italic 400 19px/1.1 var(--serif); letter-spacing: 0.003em; color: color-mix(in srgb, var(--ink) 74%, transparent); }
.feature h2 { margin: clamp(12px, 1.4vw, 22px) 0 0; font: 400 var(--hs, clamp(40px, 7vw, 120px))/var(--lh, 0.94) var(--serif);
  letter-spacing: var(--tr, -0.03em); }
.feature h2 em { letter-spacing: -0.012em; }
.lede { font: 400 var(--ls, clamp(21px, 2vw, 34px))/1.22 var(--serif); letter-spacing: -0.008em; margin: clamp(26px, 3vw, 44px) 0 0; text-wrap: balance; }
.f-text { margin-top: clamp(20px, 2.4vw, 30px); column-gap: calc(var(--cg) * 1.6); columns: 1; }
.f-text p { margin: 0 0 1.1em; break-inside: avoid-column; text-wrap: pretty; }
.side { margin-top: clamp(26px, 3vw, 44px); }
.side dl { margin: 0; border-top: 1px solid var(--ink); }
.side .row { display: grid; grid-template-columns: 104px 1fr; gap: 12px; padding: 12px 0;
  border-bottom: 1px solid var(--hair); font-size: 15px; line-height: 1.5; }
.side dt { font: 400 11.5px/1.75 var(--mono); letter-spacing: 0.02em; color: var(--muted); }
.side dd { margin: 0; }
.f-num { font: italic 400 clamp(72px, 9.2vw, 176px)/0.8 var(--serif); letter-spacing: -0.04em; color: var(--accent); margin-left: -0.06em; }
@media (max-width: 760px) { .feature h2 { text-wrap: balance; } .f-top .say { font-size: 17px; } }
@media (min-width: 761px) { .f-text { columns: 2; } }
@media (max-width: 460px) { .side .row { grid-template-columns: 84px 1fr; } }
@media (max-width: 360px) { .side .row { grid-template-columns: 1fr; gap: 2px; } }
@media (min-width: 1100px) { .feature .f-top { grid-row: 1; } .feature h2 { grid-row: 2; } }
```

- **Top line**: a 1px ink rule; below it on one baseline the mono number (slashed zero), the aside in grey italic serif, the field label at the right end. Feature 01's number alone is `--accent-ink` (`#f1 .f-top .no`, §4.4.3). Beside an aside the number drops "No.". Why: the aside reads first.
- **Headline**: serif 400, emphasis `<em>` or `.ink` (§5.6). **Body**: two columns from 761px, paragraphs never split; a single narrow track forces `columns: 1` and a `ch` max-width. **Details**: a fixed 104px grey mono label column (sentence case), hairlines under rows.
- **Tokens.** Each template sets on `#fN`: `--hs` headline size, `--lh` leading, `--tr` tracking, `--ls` lede size, `--qs` quote size (§5.4). `#fN { margin-top }` follows **position**: P1… for features 1…N−1, P7 for the last (§6.4). Why: type belongs to the composition, pauses to the page's rhythm.

### 10.4 Spec: details-table dialects

One `<dl>`, three looks fixed by the template: **ruled** (L1–L4, L6), **mono run-in** (L5: `Label — Value · Label — Value` wrapping, one pair per line ≥ 1280), **serif claims** (L7: grey mono label over a ≈ 24px serif statement, no rules).

```css
/* L5 · run-in */
#fN .side dl { border-top: 0; font: 400 12.5px/1.85 var(--mono); letter-spacing: -0.005em; }
#fN .side .row, #fN .side dt, #fN .side dd { display: inline; padding: 0; border: 0; font: inherit; letter-spacing: inherit; }
#fN .side dt::after { content: "\00a0\2014\00a0"; }
#fN .side .row:not(:last-child)::after { content: "\00a0\00b7\0020"; color: var(--muted); }
@media (min-width: 1280px) {
  #fN .side .row { display: block; }
  #fN .side .row:not(:last-child)::after { content: none; }
}
/* L7 · claims */
#fN .side dl { border-top: 0; }
#fN .side .row { display: block; padding: 0 0 clamp(14px, 1.4vw, 20px); border: 0; }
#fN .side dt { font: 400 11.5px/1.4 var(--mono); }
#fN .side dd { margin: 4px 0 0; font: 400 clamp(20px, 1.7vw, 26px)/1.18 var(--serif); letter-spacing: -0.01em; text-wrap: balance; }
```

No-break spaces glue `Label —` to its value; the space after `·` is the only break. Claims suit statements, not facts.

### 10.5 Spec: pull quotes

```css
.quote { margin: clamp(40px, 6vw, 96px) 0 0; font: 400 italic var(--qs, clamp(30px, 6vw, 110px))/1.02 var(--serif); letter-spacing: -0.02em; text-wrap: balance; }
.quote .up { font-style: normal; letter-spacing: -0.012em; }
.quote .hang { display: inline-block; width: 0; transform: translateX(-0.46em); color: var(--sand); }
```

```html
<!-- L4, L7 -->
<p class="quote" data-fly=".6"><span class="hang" aria-hidden="true">&ldquo;</span>Principle sentence<br class="d"> with one <span class="up">turned</span> word.<span aria-hidden="true">&rdquo;</span></p>
<!-- L3, L5 -->
<p class="quote" data-fly=".6">Principle sentence with one <span class="up">turned</span> word.</p>
```

- **Inverse emphasis**: the quote is italic, its `turn` (§3) upright; L4 and L5 invert it again; L7's turn is `.up.ink`. Why: four identical quotes read as a template.
- **Hanging mark**: sand, zero-width, −0.46em into the margin so the text edge stays on its line; closing mark ink; both `aria-hidden`; only L4 and L7. Why: L3's first line sits on the tray edge, L5 is right-aligned.
- A `<p>`, not a citation `<blockquote>`. At 1440: L3 92px, L4 75, L5 61, L7 101.

### 10.6 Layout templates L1–L7 (≥ 1100px)

Unplaced parts stay at `col 1 / col-end`; `col N / col M` spans N…M−1 (§6.3.4). Tray base (§12): `margin-top: clamp(44px, 5vw, 80px); border-radius: 28px`.

| | Headline (track at 1440) | Lede | Body | Details | Tray | Quote |
|---|---|---|---|---|---|---|
| L1 | one line, col 1 → bleed (1377px) | 1–7 | 2–7, 2 cols | right 9–12 | full frame | – |
| L2 | 4–12, right-aligned, 2 lines (979) | 2–6 | 2–6, 60ch | right 9–12 | 2–12 | – |
| L3 | stepped, line 2 at col 4, rule under | 4–10 | 4–12, 2 cols | left 1–3 | full, deep bottom | 3–12 over tray |
| L4 | 1–4, 2 lines (421) | 6–11, beside | 6–12, 2 cols | left 1–4 | screen edge → 11 | 6–12 upright |
| L5 | 3–8 + numeral 1–2 (644) | 3–7 | 3–7, 58ch | right 10–12 run-in | none | 6–12 right, upright |
| L6 | 2–11, 2 lines (1090) | 8–12, bottom | 8–12, 62ch | left 2–5 + print | 2–11 | – |
| L7 | one line, 1–9 (979) | 1–7 | 3–8, 2 cols | right 10–12 claims | none | col 1 → bleed |

#### 10.6.1 L1 · Opener

```css
#fN { --hs: clamp(48px, 8vw, 150px); --lh: 0.9; --tr: -0.036em; --ls: clamp(22px, 2.3vw, 40px); }
@media (min-width: 1100px) {
  #fN h2 { grid-column: col 1 / bleed-end; }
  #fN .lede { grid-column: col 1 / col 8; grid-row: 3; }
  #fN .side { grid-column: col 9 / col-end; grid-row: 3 / span 2; margin-top: calc(clamp(26px, 3vw, 44px) + 8px); }
  #fN .f-text { grid-column: col 2 / col 8; grid-row: 4; }
  #fN .insert { grid-row: 5; }
}
@media (min-width: 761px) and (max-width: 1099px) { #fN .side { grid-column: col 7 / col-end; } }
```

The headline may end at `bleed-end`. Why: it never wraps early. The body steps one column in; `+8px` drops the table's rule just below the lede's top.

#### 10.6.2 L2 · Mirror

```css
#fN { --hs: clamp(44px, 6.6vw, 124px); --tr: -0.03em; --ls: clamp(21px, 2vw, 34px); }
@media (min-width: 1100px) {
  #fN .f-top { grid-column: col 5 / col-end; }
  #fN h2 { grid-column: col 4 / col-end; text-align: right; }
  #fN .lede { grid-column: col 2 / col 7; grid-row: 3; margin-top: clamp(36px, 4.4vw, 64px); }
  #fN .side { grid-column: col 9 / col-end; grid-row: 3 / span 2; margin-top: calc(clamp(36px, 4.4vw, 64px) + 10px); }
  #fN .f-text { grid-column: col 2 / col 7; grid-row: 4; columns: 1; max-width: 60ch; }
  #fN .insert { grid-column: col 2 / col-end; grid-row: 5; margin-top: clamp(56px, 6.6vw, 96px); }
}
@media (min-width: 761px) and (max-width: 1099px) {
  #fN .side { grid-column: col 7 / col-end; }
  #fN h2 { grid-column: col 3 / col-end; text-align: right; }
}
@media (max-width: 760px) {
  #fN h2 { grid-column: col 2 / col-end; text-align: right; }
  #fN .f-top { grid-column: col 4 / col-end; }
}
```

The top-left quadrant stays empty. Why the 4.4vw lede gap: the eye travels back from a right-aligned headline.

#### 10.6.3 L3 · Stepped title, quote over the tray

```css
#fN { --hs: clamp(44px, 7vw, 132px); --lh: 0.92; --ls: clamp(21px, 2.1vw, 36px); --qs: clamp(36px, 6.4vw, 118px); }
/* .l1/.l2 lines, the col-4 indent and the rule under the headline: §5.5.4 */
@media (min-width: 1100px) {
  #fN .f-top { border-top: 0; padding-top: 0; }
  #fN .side { grid-column: col 1 / col 4; grid-row: 3 / span 2; }
  #fN .lede { grid-column: col 4 / col 11; grid-row: 3; }
  #fN .f-text { grid-column: col 4 / col-end; grid-row: 4; }
  #fN .insert { grid-row: 5; margin-top: clamp(56px, 5.6vw, 80px); padding-bottom: clamp(64px, 7vw, 110px); }
  #fN .quote { grid-column: col 3 / col-end; grid-row: 6; margin-top: -0.56em; position: relative; z-index: 1; }
}
@media (min-width: 761px) and (max-width: 1099px) { #fN .side { grid-column: col 1 / col 8; } }
@media (max-width: 760px) { #fN .f-top { border-top: 0; padding-top: 0; } }
```

The rule moves under the headline from the indent: title and text column form one L. The quote's −0.56em lays its first line's top half over the tray edge; the deep bottom padding keeps tray content clear. Why `z-index: 1`: an arriving tray's inline transform makes it a stacking context, and the quote would slip under it.

#### 10.6.4 L4 · Spread, tray bleeding left

```css
#fN { --hs: clamp(38px, 4.6vw, 84px); --lh: 0.98; --tr: -0.026em; --ls: clamp(20px, 1.85vw, 31px); --qs: clamp(32px, 5.2vw, 96px); }
#fN .quote { font-style: normal; }
#fN .quote .up { font-style: italic; letter-spacing: -0.005em; }
@media (min-width: 1100px) {
  #fN .f-top { border-top-color: var(--hair); }
  #fN h2 { grid-column: col 1 / col 5; margin-top: 22px; }
  #fN .lede { grid-column: col 6 / col 12; grid-row: 2; margin-top: 0; padding-top: clamp(26px, 2.6vw, 40px); }
  #fN .side { grid-column: col 1 / col 5; grid-row: 3; margin-top: clamp(32px, 3.4vw, 52px); }
  #fN .f-text { grid-column: col 6 / col-end; grid-row: 3; margin-top: clamp(32px, 3.4vw, 52px); }
  #fN .insert { grid-column: bleed-start/col 12; grid-row: 4; border-radius: 0 28px 28px 0; padding-left: calc(var(--gut) + clamp(16px, 2.4vw, 32px)); margin-top: clamp(48px, 5vw, 72px); }
  #fN .quote { grid-column: col 6 / col-end; grid-row: 5; margin-top: clamp(48px, 5.2vw, 80px); }
}
@media (min-width: 761px) and (max-width: 1099px) {
  #fN .side { grid-column: col 1 / col 8; }
  #fN .insert { grid-column: bleed-start/col-end; border-radius: 0 28px 28px 0; padding-left: calc(var(--gut) + clamp(16px, 2.4vw, 32px)); }
}
@media (max-width: 760px) { #fN .insert { grid-column: bleed-start/col-end; border-radius: 0 22px 22px 0; padding-left: calc(var(--gut) + 12px); } }
```

Two halves under a hairline. The smallest headline shares its row with the lede, whose `padding-top` aligns its first line with the headline's x-height. Off-screen corners are square; `--gut` in the padding returns content to the frame (§6.3.6).

#### 10.6.5 L5 · Interlude, giant numeral (no figure)

```css
#fN { --hs: clamp(36px, 4.8vw, 88px); --lh: 1; --tr: -0.024em; --ls: clamp(20px, 1.8vw, 30px); --qs: clamp(30px, 4.2vw, 76px); }
#fN .quote { font-style: normal; }
#fN .quote .up { font-style: italic; letter-spacing: -0.005em; }
@media (min-width: 1100px) {
  #fN .f-num { grid-column: col 1 / col 3; grid-row: 1 / span 2; align-self: end; }
  #fN .f-top { grid-column: col 3 / col 8; border-top: 0; padding-top: 0; }
  #fN h2 { grid-column: col 3 / col 9; margin-top: 6px; }
  #fN .lede { grid-column: col 3 / col 8; grid-row: 3; }
  #fN .f-text { grid-column: col 3 / col 8; grid-row: 4; columns: 1; max-width: 58ch; }
  #fN .side { grid-column: col 10 / col-end; grid-row: 3 / span 2; align-self: end; }
  #fN .quote { grid-column: col 6 / col-end; grid-row: 5; text-align: right; }
}
@media (min-width: 761px) and (max-width: 1099px) { #fN .side { grid-column: col 7 / col-end; } }
@media (max-width: 760px) {
  .f-num { grid-row: 1; grid-column: col 1 / col 5; align-self: end; font-size: clamp(64px, 22vw, 96px); margin-bottom: 6px; }
  #fN .f-top { grid-row: 1; grid-column: col 5 / col-end; align-self: end; border-top: 0; padding-top: 0; }
  #fN .quote { text-align: right; }
}
```

The `--accent` numeral replaces the top-line number, its baseline near the headline's last line; its 0.8 leading overflows upward, never clip it. Tablet: the numeral is its own row above a ruled top line. Phone: both share row 1, no rule.

#### 10.6.6 L6 · Inset, with a taped print

```css
#fN { --hs: clamp(42px, 6.6vw, 124px); --tr: -0.02em; --ls: clamp(23px, 2.7vw, 46px); }
@media (min-width: 1100px) {
  #fN .f-top { grid-column: col 1 / col 8; justify-content: flex-end; }
  #fN .f-top .no { order: 2; }
  #fN .f-top > span:last-child { margin-left: 0; }
  #fN h2 { grid-column: col 2 / col 12; margin-top: clamp(20px, 2.4vw, 34px); }
  #fN .side { grid-column: col 2 / col 6; grid-row: 3; }
  #fN .snap-sketch { grid-column: col 2 / col 5; grid-row: 4; margin-top: clamp(40px, 5vw, 80px); }
  #fN .lede { grid-column: col 8 / col-end; grid-row: 3; align-self: end; }
  #fN .f-text { grid-column: col 8 / col-end; grid-row: 4; columns: 1; max-width: 62ch; }
  #fN .insert { grid-column: col 2 / col 12; grid-row: 5; margin-top: clamp(56px, 5.8vw, 84px); }
}
@media (min-width: 761px) and (max-width: 1099px) {
  #fN .side { grid-column: col 1 / col 8; }
  .snap-sketch { grid-column: col 8 / col-end; width: 80%; justify-self: end; }
}
@media (max-width: 760px) { .snap-sketch { justify-self: end; width: min(58%, 240px); margin: 30px 8px 0 0; } }
```

The only top line with labels at the right end of a short rule, number last. The lede bottom-aligns with the table: one floor line. Tablet: the print starts right of the table's start, so auto-placement keeps it on the table's row without a `grid-row`.

#### 10.6.7 L7 · Closer (no figure)

```css
#fN { --hs: clamp(40px, 6vw, 112px); --lh: 0.98; --tr: -0.028em; --ls: clamp(21px, 2vw, 34px); --qs: clamp(38px, 7vw, 132px); }
@media (min-width: 1100px) {
  #fN h2 { grid-column: col 1 / col 10; }
  #fN .side { grid-column: col 10 / col-end; grid-row: 2 / span 2; margin-top: 24px; }
  #fN .lede { grid-column: col 1 / col 8; grid-row: 3; margin-top: clamp(30px, 3.6vw, 52px); }
  #fN .f-text { grid-column: col 3 / col 9; grid-row: 4; }
  #fN .quote { grid-column: col 1 / bleed-end; grid-row: 5; margin-top: clamp(64px, 7.6vw, 110px); }
}
@media (min-width: 761px) and (max-width: 1099px) { #fN .side { grid-column: col 7 / col-end; } }
```

Only here do the details start on the headline row. The body stair-steps two columns in; the largest quote may bleed right, its mark in the gutter.

### 10.7 Responsive: tablet and phone

```css
@media (min-width: 761px) and (max-width: 1099px) { .feature .lede { grid-column: col 1 / col 12; } .quote { grid-column: col 2 / col-end; } }
@media (max-width: 760px) { .quote { grid-column: col 2 / col-end; } }
```

- **761–1099**: DOM order at `col 1 / col-end`, `br.d` off; lede one column short; body two columns everywhere; the table a half block on its wide side (right `col 7` for L1, L2, L5, L7; left to `col 8` for L3, L4, L6); L2 right-aligned, L4 still bleeding, quotes one column in.
- **≤ 760**: one column, headlines at clamp minimums and balanced, table full width (label 104 → 84px ≤ 460 → stacked ≤ 360), quotes at `col 2` so a mark has room, L6's print 8px in. Why: its tilted corner must not touch the edge.

### 10.8 Sequencing N features

1. Feature 01 (the work the person most wants seen) is always L1, trayless if it has no figure.
2. Each template at most once; neighbours never share one.
3. Other features with a figure take L4, L2, L3, L6 in that order; features without take L7 (the closer), L5, then L6, L3, L2 without tray (°). L4 always has a tray. Why: it is the page's one left bleed.
4. Arrange in ascending template order. Features 02…N **MAY** be reordered for this; feature 01 never.
5. **Bleeds:** L1 headline →, L4 tray ←, L7 quote →. L4 is present whenever a feature after 01 has a figure.
6. **Details sides** (L1 R, L2 R, L3 L, L4 L, L5 R, L6 L, L7 R): never three in a row on one side. Hence L3 before L2 where L3·L4·L6 would run, and L6° instead of L5 between L1 and L7.
7. **Pauses** follow position (§10.3, §6.4): short binds a pair, long opens the next.
8. Never two neighbours both without figure and quote; at most four quotes.

Lookup:

| N | k = 0 | k = 1 | k = 2 | k = 3 | k = 4 | k = 5 |
|---|---|---|---|---|---|---|
| 3 | L1·L2·L4 | L1·L4·L7 | L1·L6°·L7 | – | – | – |
| 4 | L1·L2·L3·L4 | L1·L2·L4·L7 | L1·L4·L5·L7 | L1·L5·L6°·L7 | – | – |
| 5 | L1·L3·L2·L4·L6 | L1·L2·L3·L4·L7 | L1·L2·L4·L5·L7 | L1·L4·L5·L6°·L7 | L1·L3°·L5·L6°·L7 | – |
| 6 | – | L1·L3·L2·L4·L6·L7 | L1·L2·L3·L4·L5·L7 | L1·L2·L4·L5·L6°·L7 | L1·L3°·L4·L5·L6°·L7 | L1·L2°·L3°·L5·L6°·L7 |
| 7 | – | – | L1…L7 | L1…L7, L6° | L1…L7, L3° L6° | L1…L7, L2° L3° L6° |

A ° template has no `.insert`; L3° also drops the quote overlap (`margin-top`, `position`, `z-index`). Seven features with one figure do not fit: cut the weakest told one.

### 10.9 States, motion, accessibility

- **States**: none. Dark mode needs no feature rules: every colour is a token.
- **Motion** (§16): pulls `.f-top` rise, `h2` 1, `.lede` .7, `.f-text` .5 (+.06s), `.side` .5 (+.12s), `.snap` .6, `.insert` rise, `.quote` .6; `.f-num` none, an anchor while neighbours travel.
- **Accessibility**: `aria-labelledby` names each article; tray titles are `h3`; `aria-hidden` on `.f-num`, quote marks and the placeholder; details are a real `<dl>`; `.ink` drops its shadow under `forced-colors` (§5.6.2).

### 10.10 Pitfalls

- **Rows exist only ≥ 1100**: wrong DOM order, wrong small-screen layout.
- **The field label must be the last span**; when `order` moves the number (L6), reset its `margin-left`.
- **Overlap needs both** the quote's `z-index` and the tray's deep padding; no quote, no padding.
- **No `overflow: hidden` on a feature**: it clips 0.8-leading numerals, bleeds and arrivals.

### 10.11 Adapting to new content

- Fit each headline to its track (§10.6 table, §5.9, ≈ 0.33em per character): L1, L7 one line at 1100; L2, L4, L5, L6 two; L3's italic line under 9 columns. Too long: cut words or pick a wider template, never a smaller size.
- Hand-set at most two words; re-check at 390, 1100, 1440, 1920 (§5.5).
- Details 3–6 rows, labels ≤ 11 characters (L5 values ≤ 4 words). Asides on 2–3 features (1–2 when N ≤ 4). Single-track bodies 55–65ch.

### 10.12 Photo slots: taped prints

#### 10.12.1 Purpose

A print taped into an empty quadrant: an analogue counterweight to the grid that reads as intentional while empty. At most three: letter (§9), L6, next card (§11).

#### 10.12.2 Anatomy

```html
<div class="snap snap-sketch" data-fly=".6"> <!-- arrival and hover target -->
  <figure class="snap-in" style="--tilt:1.8deg"> <!-- the print, carries the tilt -->
    <span class="snap-ph" aria-hidden="true"></span>
    <figcaption>{{features[i].photoSlot.caption}}</figcaption>
  </figure>
</div>
```

Role classes: `.snap-desk` (letter), `.snap-sketch` (L6), `.snap-process` (next). `features[i].photoSlot` mirrors `letter.photoSlot`. Captions are lowercase: `{what it will show} — photo to come`.

#### 10.12.3 Spec (B.13)

```css
.snap { position: relative; }
.snap-in {
  margin: 0; position: relative; display: block; padding: 10px 10px 0; background: #f7f5ef;
  box-shadow: 0 1px 1px rgba(30, 25, 18, 0.08), 0 14px 28px -18px rgba(30, 25, 18, 0.45);
  transform: rotate(var(--tilt, -2deg)); transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.snap-in::before {
  content: ""; position: absolute; left: 50%; top: -11px; width: 84px; height: 24px; margin-left: -42px;
  background: color-mix(in srgb, var(--sand) 34%, rgba(255, 255, 255, 0.5)); opacity: 0.8;
  transform: rotate(calc(var(--tilt, -2deg) * -1.6));
}
.snap-ph, .snap-in img { display: block; width: 100%; aspect-ratio: 4/5; object-fit: cover; }
.snap-ph { background: repeating-linear-gradient(-45deg, transparent 0 9px, rgba(40, 34, 26, 0.09) 9px 10px), #ebe7de; }
.snap figcaption { padding: 10px 4px 12px; font: 400 11.5px/1.35 var(--mono); letter-spacing: 0.01em; color: #5a554c; text-align: center; }
```

- **Print** `#f7f5ef` (fixed, cooler than `--paper`), 10px border, caption as the deep bottom margin. **Shadow**: contact line plus a drop whose −18px spread shows only below (§4.9).
- **Tape** is a child of the rotated print: world angle −0.6 × tilt (+1.8deg → ≈ −1.1deg). Why: it leans slightly the other way, stuck on by hand.
- **Placeholder**: 1px diagonal every 10px, 4/5. **Tilt** 1.5–3.5deg, sign alternating down the page (letter −2.4, L6 1.8, next 3.2).

#### 10.12.4 States

```css
@media (hover: hover) and (prefers-reduced-motion: no-preference) {
  .snap:hover .snap-in { transform: rotate(calc(var(--tilt, -2deg) * 0.4)) translateY(-3px); }
}
```

Hover on the wrapper straightens the print to 40% of its tilt and lifts it 3px on a back-out spring. Why the wrapper: a still target, so corners never flicker. No focus state; hidden in print (§17).

#### 10.12.5 Dark

```css
:root[data-theme="dark"] .snap-in { background: #2e2c27; box-shadow: 0 14px 28px -18px rgba(0, 0, 0, 0.8); }
:root[data-theme="dark"] .snap-ph { background: repeating-linear-gradient(-45deg, transparent 0 9px, rgba(255, 250, 240, 0.07) 9px 10px), #25231f; }
:root[data-theme="dark"] .snap figcaption { color: #b9b3a7; }
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) .snap-in { background: #2e2c27; box-shadow: 0 14px 28px -18px rgba(0, 0, 0, 0.8); }
  :root:not([data-theme="light"]) .snap-ph { background: repeating-linear-gradient(-45deg, transparent 0 9px, rgba(255, 250, 240, 0.07) 9px 10px), #25231f; }
  :root:not([data-theme="light"]) .snap figcaption { color: #b9b3a7; }
}
```

Charcoal one step above dark `--paper2`, no contact line; the dark `--sand` greys the tape. Literal hex, hence both selectors (§4.2).

#### 10.12.6 Placement and photos

- Placement: L6 §10.6.6, letter §9, next card §11.
- **Swap in a photo**: replace the span with `<img src="{{features[i].photoSlot.src}}" alt="{{features[i].photoSlot.alt}}" width="800" height="1000" loading="lazy">` and rewrite the caption; it inherits 4/5 and `object-fit: cover`. A landscape slot sets its ratio on both children: `.snap-process .snap-ph, .snap-process img { aspect-ratio: 5/4; }`. A slot nothing will fill is dropped.
- **Accessibility**: an empty slot announces only its caption; a photo **MUST** carry `alt`. Captions: `#5a554c` on `#f7f5ef`, `#b9b3a7` on `#2e2c27`.
- **Pitfalls**: the tilt lives on `.snap-in`, never `.snap`. Why: arrivals write an inline `transform` on the `[data-fly]` wrapper, so a tilt there vanishes in flight and pops back. No `overflow: hidden` on the parent: tape and corners stick out.
- **Adapting**: keep every value; choose only the tilt (opposite sign to the previous print), the caption and the photo.

---

## 11 · Next issue, contact & footer

The page ends in three moves: one inverse card, the address as display type, a colophon under a full ink rule. Gaps: §6.4.

### 11.1 The next-issue card (`section#next > .next`)

#### 11.1.1 Purpose

The full stop, the only large ink mass below the cover: it paints `--ink` and writes `--paper`, so it always shows the opposite theme.

#### 11.1.2 Anatomy

```html
<section class="g" id="next" aria-labelledby="nextH">
  <div class="next" data-fly="rise">
    <div><span class="lab">{{next.kicker}}</span>
      <h2 id="nextH">{word one} <em>{word two.}</em></h2></div>
    <div><p>{{next.body}}</p>
      <p class="built"><span class="note">{{next.built.note}}</span><br>{{next.built.items}}</p>
      <button class="btn soon" type="button" disabled aria-disabled="true">{{next.button}}</button></div>
    <div class="snap snap-process">
      <figure class="snap-in" style="--tilt:3.2deg"><span class="snap-ph" aria-hidden="true"></span>
        <figcaption>{{next.photoSlot.caption}}</figcaption></figure></div>
  </div>
</section>
```

`{{next.headline}}`: a roman word, then an italic word holding the full stop (§3.3). In `next.mode: "open"` the button is `<a class="btn" href="mailto:{{email}}">` (§3.7).

#### 11.1.3 Spec (B.19)

```css
#next { margin-top: clamp(84px, 11vw, 184px); }
.next {
  position: relative; background: var(--ink); color: var(--paper);
  display: grid; grid-template-columns: 7fr 5fr; gap: 50px; align-items: end;
  grid-column: col 2 / bleed-end; border-radius: 32px 0 0 32px;
  padding: clamp(48px, 5vw, 84px) calc(var(--gut) + clamp(24px, 2.6vw, 44px)) clamp(44px, 4.6vw, 72px) clamp(32px, 3.6vw, 60px);
  --surf: var(--ink); --hiMix: #fff 9%; --loMix: #000 55%;
  --accent: #a9b77c; --accent-ink: #a9b77c;
}
:root[data-theme="dark"] .next { --hiMix: #fff 70%; --loMix: #000 15%; --accent: #435425; --accent-ink: #435425; }
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) .next { --hiMix: #fff 70%; --loMix: #000 15%; --accent: #435425; --accent-ink: #435425; }
}
.next .lab { font: 400 13px/1.4 var(--sans); letter-spacing: 0.01em; text-transform: none; color: color-mix(in srgb, var(--paper) 72%, transparent); }
.next h2 { font: 400 clamp(52px, 7.6vw, 150px)/0.88 var(--serif); letter-spacing: -0.035em; margin: 14px 0 0; }
.next h2 em { letter-spacing: 0.004em; }
.next p { margin: 0 0 22px; font-size: 18px; color: color-mix(in srgb, var(--paper) 84%, transparent); text-wrap: pretty; }
.next .built { margin-top: 22px; font: 400 13.5px/1.7 var(--mono); letter-spacing: -0.01em; color: color-mix(in srgb, var(--paper) 78%, transparent); }
.next .built .note { color: color-mix(in srgb, var(--paper) 68%, transparent); }
.next .btn:not(:disabled) { border: 0; color: var(--paper); background: var(--face); }
.next .btn:not(:disabled):hover { background: var(--face); color: var(--accent); }
.next .btn:disabled, .next .btn.soon { background: var(--surf); color: color-mix(in srgb, var(--paper) 60%, transparent); opacity: 1; }
.next .btn:disabled:hover { background: transparent; color: color-mix(in srgb, var(--paper) 50%, transparent); }
.snap-process { position: absolute; top: clamp(-76px, -5vw, -44px); left: clamp(150px, 22%, 380px); width: clamp(130px, 12vw, 210px); z-index: 2; }
.snap-process .snap-ph { aspect-ratio: 5/4; }
@media (max-width: 900px) {
  .next { grid-template-columns: 1fr; gap: 24px; }
  .snap-process { position: relative; top: auto; left: auto; justify-self: end; width: clamp(112px, 16vw, 150px); margin: -8px 4px calc(-1 * clamp(56px, 9vw, 90px)) 0; }
}
@media (max-width: 760px) {
  .next { grid-column: col 1 / bleed-end; border-radius: 24px 0 0 24px; padding: clamp(28px, 7vw, 44px) calc(var(--gut) + 18px) clamp(26px, 6vw, 40px) clamp(18px, 5vw, 26px); }
  .next h2 { font-size: clamp(44px, 12.5vw, 64px); }
  .snap-process { width: 112px; }
}
```

- **Re-lighting.** `--surf: var(--ink)` plus a mix pair per theme: outputs re-resolve on `*` (§4.6.1), so every control inside moulds out of the card. Why 9% / 55%: the card is darker than dark paper; 70% / 15%: bone wants a near-white lift, a neutral shade (§4.6.4).
- **Accent swap:** the other theme's tone. Why: the page accent is 2.92:1 on ink, 1.81:1 on bone; swapped, ring and hover text reach 8.55:1 and 6.96:1. A new accent writes `{{accent.dark}}` and the light `--accent-ink` (§4.4.2).
- **Text is translucent paper**, never `--muted` (tuned for paper): 84 paragraph, 78 list, 72 kicker, 68 note, 60 disabled.
- **Bleed right, round left** (§6.3.6). Kicker: sentence-case Jost 13px, not the uppercase `.lab`.
- **≥ 901px:** cells share the bottom line; the upper left is empty but for the print, ≈ 22% across, 44–76px over the edge.

#### 11.1.4 States

| | Disabled (`project`) | Enabled (`open`) |
|---|---|---|
| Rest | ink face, paper 60%, `--raise-s`, `not-allowed` | ink face, `--paper`, `--raise` |
| Hover | paper 50% | `--accent` label |
| Active, focus | none | `--sink`; swapped-accent ring |

Disabled is not faded: `opacity: 1` beats the base 0.8 (`.btn`, §12). Why: a ghost reads as broken; a moulded button with a quiet label reads "not yet".

#### 11.1.5 Responsive

≤ 900: one column; the print becomes row 3, right-aligned, hanging ≈ 30px below the bottom edge. ≤ 760: `col 1`, 24px radius, print 112px. At 390 / 820 / 1440 / 1920: left edge 17 / 100 / 175 / 263px; headline 48.8 / 62.3 / 109.4 / 145.9px; print 112 / 131.2 / 172.8 / 210px.

#### 11.1.6 Motion

`data-fly="rise"` (§16): no sideways travel. Why: a wide plate swinging in reads as a slide. The print rides along (arrivals never nest). Button: `box-shadow 0.2s ease, color 0.2s`; print hover §10.12.4; paper output §17.

#### 11.1.7 Accessibility

The `h2` names the region; the button is skipped by Tab, read as unavailable. Contrast for 84 / 78 / 72 / 68 / 60 / 50: light card 10.6 / 9.3 / 8.0 / 7.3 / 5.9 / 4.5; dark card 9.2 / 7.6 / 6.2 / 5.5 / 4.3 / 3.2. Values under 4.5 sit on an inactive control (exempt, WCAG 1.4.3); live text stays ≥ 68.

#### 11.1.8 Pitfalls

- Set inputs only (§4.6.3); write both dark blocks. Keep the disabled rule after the enabled one (a live `.btn.soon` matches both).
- **MUST NOT** clip `.next` or `#next`. Up ≤ 87px (print + tape) sits in a ≥ 99px margin; down ≤ 40px in contact's ≥ 120px.
- `left: 22%` is of the card's padding box (it grows with the bleed); the clamp holds it.
- The print's literal hex (§10.12.3) follows the page theme, so it always opposes the card. Never re-key it.

#### 11.1.9 Adapting to new content

Headline tuned for ≈ 11 characters; for `c` ≤ 14: `s = min(1, 11 / c)`, `clamp(52px, 7.6·s vw, 150·s px)`. Phone: if `c × 0.38em × size` > 232px (the 300px measure), lower the 44px floor. Tilt stays 3.2deg; photo §10.12.7.

### 11.2 Contact (`section#contact`)

#### 11.2.1 Purpose

The call to action is the address in display serif, no form; lead on `col 1`, the rest one column in.

#### 11.2.2 Anatomy

```html
<section class="g" id="contact" aria-labelledby="conH">
  <h2 class="con-h" id="conH" data-fly>{{contact.lead}}</h2>
  <a class="mail" href="mailto:{{email}}" data-fly=".6"><span class="m1">{local part}</span><wbr><span class="at">@</span><span class="m2">{domain}</span></a>
  <div class="links" data-fly=".5"><a href="{{links[i].url}}" rel="noopener">{{links[i].label}}</a></div>
</section>
```

Split `{{email}}` at its last `@`; **MUST NOT** put whitespace between the spans. The lead is an `h2` for heading navigation; `font` and `margin: 0` cancel its defaults.

#### 11.2.3 Spec (B.20)

```css
#contact { margin-top: clamp(120px, 16vw, 270px); }
.con-h { grid-row: 1; margin: 0; font: 400 clamp(16px, 1.3vw, 19px)/1.45 var(--sans); max-width: 34ch; text-wrap: pretty; }
.mail {
  grid-row: 2; justify-self: start;
  font: 400 clamp(30px, 6.2vw, 116px)/1.05 var(--serif); letter-spacing: -0.028em;
  padding-bottom: 6px; margin-top: clamp(14px, 1.6vw, 24px);
  overflow-wrap: anywhere; text-decoration: none;
  background: var(--pencil) no-repeat 0 100%/0 0.28em;
  transition: background-size 0.55s cubic-bezier(0.3, 0.9, 0.3, 1.15);
}
.mail .m1 { letter-spacing: -0.036em; }
.mail .at { font-style: italic; margin: 0 0.012em 0 -0.01em; position: relative; top: -0.03em; }
.mail .m2 { color: var(--muted); letter-spacing: -0.014em; transition: color 0.25s; }
.mail:hover .m2 { color: var(--accent); }
.mail:hover, .mail:focus-visible { background-size: 100% 0.28em; color: var(--ink); }
.links { grid-row: 3; display: flex; flex-wrap: wrap; margin-top: 28px; gap: 28px; }
.links a { text-decoration: none; font: 400 16px/1 var(--sans); letter-spacing: 0.01em; padding: 10px 0 7px; border: 0; border-bottom: 1px solid var(--rule); transition: border-color 0.2s, color 0.2s; }
.links a::after { content: "\2197"; content: "\2197" / ""; display: inline-block; margin-left: 0.35em; transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); }
@media (min-width: 1100px) {
  .con-h { grid-column: col 1 / col 6; }
  .mail { grid-column: col 2 / bleed-end; }
  .links { grid-column: col 2 / col-end; }
}
@media (hover: hover) {
  .links a:hover { border-bottom-color: var(--accent); }
  .links a:hover::after { transform: translate(2px, -2px); }
}
```

- **Tracking ladder:** local −0.036em (a wordmark), whole −0.028em, domain −0.014em in `--muted` (secondary; it lights up). Italic `@`, lifted, hand-kerned.
- `<wbr>` prefers a break before `@`; `overflow-wrap: anywhere` is the last resort. `justify-self: start`: the pencil equals the address.
- **Pencil** (§5.7.2): drawn left to right, 0.55s, a flick past the end; `padding-bottom: 6px` clears descenders. Links underline with a real `--rule` border.

#### 11.2.4 States

`.mail` hover: pencil draws, domain → `--accent`; focus: pencil plus the global ring; both restate `--ink`. `.links a` hover (hover devices): `--accent` border, arrow nudge; focus: ring.

#### 11.2.5 Responsive

Below 1100px all span `col 1 / col-end`. At ≥ 1100 address and links start on `col 2`, the card's edge: one vertical through the end; the address may enter the bleed (§6.3.6). At 390 / 820 / 1440 / 1920: lead 16 / 16 / 18.7 / 19px; address 30 / 50.8 / 89.3 / 116px (one line at 390).

#### 11.2.6 Motion

Arrivals (§16): lead full pull, address `.6`, links `.5`. The user-triggered pencil, colour and arrow stay under reduced motion (§7.1.6).

#### 11.2.7 Accessibility

The link's name is the whole address. The arrow's empty alternative silences it. Links ≈ 34px tall, 28px apart (WCAG 2.5.8). Domain 6.35 / 6.45:1; hovered 5.06 / 8.00:1.

#### 11.2.8 Pitfalls

Without `overflow-wrap` the page scrolls sideways at 300px; without `justify-self: start` the pencil spans the column. The pencil's colour is baked in: it changes only via the three token blocks (§5.7).

#### 11.2.9 Adapting to new content

Tuned for 27 characters (≈ 0.385em each; 312px at 30px, one line at 360px). For `c`: `s = min(1, 27 / c)`, size `clamp(max(26, 30·s)px, 6.2·s vw, 116·s px)`, rounded. Below 26px wrap at the `<wbr>`. The lead's `34ch` sets ≤ 100 characters in three lines.

### 11.3 Footer colophon (`footer`)

#### 11.3.1 Purpose

A printed page's last line: an edge-to-edge ink rule, copyright and an honesty line left, mono colophon right.

#### 11.3.2 Anatomy

```html
<footer><div class="wrap">
  <p>&copy; <span id="yr">{year}</span> {{fullName}}. {{colophon.note}}</p>
  <p class="colo">{the fixed colophon line, §3.2}</p>
</div></footer>
```

Outside `<main>` (contentinfo). `{year}` is written in the HTML and refreshed by D.03 (§7.2.6). Why: never blank without script.

#### 11.3.3 Spec (B.21)

```css
footer { margin-top: clamp(72px, 9vw, 150px); border-top: 1px solid var(--ink); padding: 16px 0 46px; }
footer .wrap { display: flex; justify-content: space-between; gap: 20px; flex-wrap: wrap; }
footer p { margin: 0; font: 300 13px/1.6 var(--sans); color: var(--muted); max-width: 620px; }
footer .colo { font: 400 12px/1.6 var(--mono); letter-spacing: -0.005em; text-align: right; max-width: 44ch; margin-left: auto; }
@media (max-width: 620px) { footer .colo { text-align: left; margin-left: 0; } }
```

The rule sits on `footer`, not `.wrap`: the only edge-to-edge ink line (dividers elsewhere are `--hair`). `.wrap` holds the text to the frame (§6.2). No `data-fly`.

#### 11.3.4 Responsive

One row while the frame holds 620 + 20px + 44ch (≈ 954px, viewport ≈ 1046px); narrower, the colophon wraps 20px below, still right-aligned; ≤ 620 it aligns left.

#### 11.3.5 Accessibility

Contentinfo; `--muted` 6.35 / 6.45:1; the year is text.

#### 11.3.6 Pitfalls

A rule on `.wrap` stops at the frame; without `margin-left: auto` the wrapped colophon jumps left.

#### 11.3.7 Adapting to new content

`44ch` sets the colophon line in three lines; re-tune it if the line changes. `colophon.note` ≤ 120 characters: two lines.

---

## 12 · Interactive figure kit

A figure proves one claim by touch: caption, a few values, one button, one sentence of result, a note on what is simulated (R22). Owns B.14, B.15, D.05, D.08. States §4.6.5; focus §6.1; microcopy §3.9; tab, tracker, lists §13; 3D §14. `B.23` marks declarations kept in B.23 (§2.4).

### 12.1 The tray `.insert`

```html
<div class="insert" id="figN" data-fly="rise">
  <div class="insert-h"><span>Fig. N &mdash; try it</span><span>{{features[i].figure.note}}</span></div>
  <h3>{{features[i].figure.title}}</h3>
  <p class="cap">{{features[i].figure.caption}}</p>
  <div class="ctrls">…</div>
  <div class="runrow"><button class="btn solid" type="button">{Verb}</button><button class="btn" type="button">Reset</button></div>
  …archetype body…
  <div class="verdict" id="figNVerdict" aria-live="polite"></div>
</div>
```

```css
.insert { margin-top: clamp(44px, 5vw, 80px); background: var(--paper2); --surf: var(--paper2); border-radius: 28px;
  padding: clamp(16px, 3.4vw, 30px) clamp(12px, 3.4vw, 32px) clamp(20px, 3.6vw, 34px); }
.insert > * { min-width: 0; }
.insert-h { display: flex; justify-content: space-between; align-items: baseline; gap: 4px 12px; flex-wrap: wrap; }
.insert-h span { font: 400 12px/1.4 var(--mono); letter-spacing: -0.01em; }
.insert-h span:last-child { color: var(--muted); }
.insert .lab, .tk-h { font: 400 11.5px/1.4 var(--mono); letter-spacing: 0.03em; text-transform: none; }
.insert h3 { font: 400 clamp(26px, 6.4vw, 54px)/1.02 var(--serif); letter-spacing: -0.02em; margin: 10px 0 12px; }
.cap { font: 400 clamp(14.5px, 3.6vw, 16px)/1.6 var(--sans); color: var(--muted); margin: 0 0 24px; max-width: 62ch; }
.ctrls { display: grid; grid-template-columns: repeat(auto-fit, minmax(min(210px, 100%), 1fr)); gap: 18px 34px; }
.runrow { display: flex; flex-wrap: wrap; margin: 0 0 24px; gap: 18px; }
@media (max-width: 460px) { .runrow .btn { flex: 1 1 auto; text-align: center; } }
```

- "&mdash; try it" on the first figure only; ids `figN` + a PascalCase part.
- **A flat well, not a card:** no border or shadow; `--surf` re-lights the controls inside. Why: controls are moulded out of the plate.
- Footprint per template (§10.6); the one left bleed (L4) squares its edge corners and pads by `--gut` more (§6.3.6).
- Dark: `#25231f` on `#1c1b18`. Print hides `.runrow, .seg, .toggles`.

### 12.2 Controls (B.15)

All in `--ui`; every control is a `999px` pill except 16px push buttons.

#### 12.2.1 Labels and live values

```html
<label for="figNCap"><span id="figNCapL">{Label}</span> <output id="figNCapO" for="figNCap">20</output>
  <input id="figNCap" type="range" min="5" max="100" value="20" aria-labelledby="figNCapL"></label>
```

```css
label { display: block; font: 700 11.5px/1.5 var(--ui); letter-spacing: 0.06em; text-transform: uppercase; color: var(--muted); }
@media (max-width: 300px) { .lab, label { letter-spacing: 0.1em; } } /* B.23 */
label output { float: right; color: var(--ink); font: 700 13px var(--ui); letter-spacing: 0; }
```

Every `<label>` **MUST** carry `for` and wrap its text in a `<span id>` the control names (`aria-labelledby`). Why: with `<output>` first, the implicit control is the output. `bindOut` writes `fmt(value)` on `input` and at boot (also a range's `aria-valuetext`). Idle figures reset on `change`, not mid-drag.

#### 12.2.2 Pill fields

```css
input[type="text"], input[type="number"], select { display: block; width: 100%; color: var(--ink); }
input[type="text"], input[type="number"], select, .csel-btn { padding: 12px 18px; border: 0; border-radius: 999px;
  background: var(--surf); box-shadow: var(--sink-s); font: 600 16px var(--ui); margin-top: 10px; }
input:focus, select:focus { outline: none; box-shadow: var(--sink-s), 0 0 0 2px var(--accent); }
select { appearance: none; }
select option { background: var(--paper); color: var(--ink); }
select { background-image: linear-gradient(45deg, transparent 50%, var(--ink) 50%), linear-gradient(135deg, var(--ink) 50%, transparent 50%);
  background-position: calc(100% - 24px) 55%, calc(100% - 19px) 55%; background-size: 5px 5px; background-repeat: no-repeat; }
.fields { display: grid; grid-template-columns: 1fr 1fr; gap: 16px 26px; align-content: start; }
@media (max-width: 460px) { .fields { grid-template-columns: minmax(0, 1fr); } }
```

≈ 44px tall; 16px stops iOS zoom. Chevron size and repeat **MUST** follow the `background` shorthand, which resets them.

#### 12.2.3 Custom dropdown

Every `<select>` becomes a combobox (R23); the native select stays the **source of truth**: read and listen on it, drive it by `selectedIndex` + a fired `change`. DOM: `div.csel` (+ `.open`, `.up`) > `button.csel-btn#csel1-b[role=combobox][aria-haspopup=listbox][aria-expanded][aria-controls=csel1-l][aria-labelledby="{label span} csel1-v"]` (`span#csel1-v` value, `span.csel-caret[aria-hidden]`) + `ul.csel-list#csel1-l[role=listbox]` > `li.csel-opt#csel1-o{i}[role=option][aria-selected][data-i]` + `select.csel-native[tabindex=-1][aria-hidden]`.

```css
.csel-native { position: absolute !important; width: 1px; height: 1px; opacity: 0; pointer-events: none; overflow: hidden; margin: 0 !important; padding: 0 !important; }
.csel { position: relative; margin-top: 10px; }
.csel-btn { width: 100%; display: flex; align-items: center; justify-content: space-between; gap: 12px; color: var(--ink); text-align: left; cursor: pointer; margin-top: 0; }
.csel-btn:focus-visible { outline: 1px solid var(--accent); outline-offset: 3px; }
.csel-btn:hover { box-shadow: var(--sink); }
.csel-btn[aria-expanded="true"] { box-shadow: var(--sink-s), 0 0 0 2px var(--accent); }
.csel-caret { flex: none; width: 8px; height: 8px; border-right: 1px solid currentColor; border-bottom: 1px solid currentColor;
  transform: rotate(45deg) translateY(-2px); transition: transform 0.2s; margin-right: 4px; }
.csel-btn[aria-expanded="true"] .csel-caret { transform: rotate(-135deg) translate(-2px, 0); }
@media (prefers-reduced-motion: reduce) { .csel-caret { transition: none; } }
.csel-list { position: absolute; left: 0; right: 0; top: calc(100% + 14px); z-index: 40; margin: 0; padding: 10px; list-style: none;
  max-height: 264px; overflow: auto; display: none; border: 0; border-radius: 22px; background: var(--surf); box-shadow: var(--raise); }
.csel.up .csel-list { top: auto; bottom: calc(100% + 14px); }
.csel.open .csel-list { display: block; }
.csel-opt { display: flex; justify-content: space-between; align-items: center; gap: 12px; letter-spacing: 0; text-transform: none;
  color: var(--ink); cursor: pointer; border-radius: 12px; padding: 10px 14px; font: 600 15.5px var(--ui); }
.csel-opt.act { background: var(--face); box-shadow: var(--raise-s); }
.csel-opt[aria-selected="true"] { font-weight: 700; color: var(--accent-ink); }
.csel-opt[aria-selected="true"]::after { content: ""; flex: none; width: 6px; height: 11px; border-right: 1.5px solid var(--accent);
  border-bottom: 1.5px solid var(--accent); transform: rotate(45deg) translateY(-2px); }
```

The ring is keyboard-only: `.csel-btn:focus:not(:focus-visible)` (§6.1) outranks hover and open.

| Key | Closed | Open |
|---|---|---|
| ↓ / ↑ | open, highlight selected (or 0) | ±1, clamped, no wrap |
| Home / End | passes through | first / last |
| Enter / Space | open | choose highlighted |
| Escape | passes through | close, `preventDefault` |
| Tab | – | close, no choice |
| printable, no Ctrl/⌘ | type-ahead: open, highlight | highlight |

```js
// customSelect(sel): once per sel (sel._csel); id = "csel" + ++n; label.htmlFor = btn.id;
// btn aria-labelledby = label span + " " + id + "-v"
function open() {
  var r = btn.getBoundingClientRect(), need = Math.min(264, items.length * 44 + 14);
  wrap.classList.toggle("up", innerHeight - r.bottom < need + 16 && r.top > need + 16 + 54);   // 54 = bar
  wrap.classList.add("open"); btn.setAttribute("aria-expanded", "true"); hi(Math.max(0, sel.selectedIndex));
}
function choose(i) {
  sel.selectedIndex = i;
  sel.dispatchEvent(new Event("input", { bubbles: true }));
  sel.dispatchEvent(new Event("change", { bubbles: true })); close(); btn.focus();
}
function typeAhead(k) {                                      // "€ EUR" matches "e"
  buf += k.toLowerCase(); clearTimeout(bufT); bufT = setTimeout(function () { buf = ""; }, 600);
  for (var i = 0; i < items.length; i++)
    if (items[i].textContent.toLowerCase().replace(/^[^a-z0-9]+/, "").indexOf(buf) === 0) { open(); hi(i); break; }
}
// hi(i): clamp, .act, aria-activedescendant, scroll into view (6px air); close() undoes open(); wrap._close = close
// sel change → refresh(); list mousedown → preventDefault (so focusout cannot close first); focusout → close
// one document pointerdown closes every .csel.open not containing the target (covers dropdowns built later)
```

#### 12.2.4 Sliders

```css
input[type="range"] { width: 100%; margin: 4px 0 0; appearance: none; background: transparent; touch-action: pan-y; height: 40px; }
input[type="range"]::-webkit-slider-runnable-track { height: 10px; border-radius: 10px; background: var(--surf); box-shadow: var(--sink-s); }
input[type="range"]::-moz-range-track { height: 10px; border-radius: 10px; background: var(--surf); box-shadow: var(--sink-s); }
input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; cursor: pointer; width: 28px; height: 28px; margin-top: -9px; border-radius: 50%;
  background: radial-gradient(circle, var(--accent) 4px, transparent 4.5px), var(--face); border: 0; box-shadow: 4px 4px 9px var(--lo), -4px -4px 9px var(--hi); }
input[type="range"]::-moz-range-thumb { cursor: pointer; width: 28px; height: 28px; border-radius: 50%;
  background: radial-gradient(circle, var(--accent) 4px, transparent 4.5px), var(--face); border: 0; box-shadow: 4px 4px 9px var(--lo), -4px -4px 9px var(--hi); }
@media (pointer: coarse) {
  input[type="range"]::-webkit-slider-thumb { width: 20px; height: 20px; margin-top: -5px; }
  input[type="range"]::-moz-range-thumb { width: 20px; height: 20px; }
}
```

No fill: the dot is the only colour. WebKit and Gecko rules **MUST** stay separate. Why: an engine drops a list holding a pseudo it does not know. Thumb halo: §6.1.

#### 12.2.5 Push buttons

```css
.btn { cursor: pointer; text-decoration: none; font: 700 14.5px var(--ui); letter-spacing: 0.01em; padding: 14px 26px; border: 0;
  border-radius: var(--r); background: var(--face); color: var(--ink); box-shadow: var(--raise); transition: box-shadow 0.2s ease, color 0.2s; }
.btn:hover { background: var(--face); color: var(--accent-ink); } /* B.23 */
.btn.solid:hover { background: var(--accent); color: var(--on-accent); }
.btn:disabled:hover { background: var(--surf); color: var(--muted); }
.btn:active { background: var(--surf); box-shadow: var(--sink); }
.btn.solid { background: var(--gface); color: var(--on-gface); }
.btn.solid:active { box-shadow: inset 2px 2px 6px rgba(0, 0, 0, 0.28); }
.btn:disabled, .btn.soon { cursor: not-allowed; pointer-events: auto; background: var(--surf); border: 0; color: var(--muted); opacity: 0.8; box-shadow: var(--raise-s); }
```

Never pills. Rule order gives a disabled primary the secondary's disabled look. ≤ 360px: `11px 14px`, tracking 0.12em. Two words ≈ 115 × 48px.

#### 12.2.6 Segmented control

```css
.seg { display: inline-flex; margin-bottom: 22px; gap: 14px; padding: 4px; }
.seg button { border: 0; cursor: pointer; font: 700 14px var(--ui); padding: 11px 24px; border-radius: 999px; color: var(--muted);
  background: var(--face); box-shadow: var(--raise-s); transition: box-shadow 0.2s, color 0.2s; }
.seg button[aria-pressed="true"] { background: var(--gface); color: var(--on-gface); }
```

Markup §12.6.3: a labelled `role="group"`, exactly one `aria-pressed="true"`, each pill a Tab stop; ≤ 360px padding `9px 14px`.

#### 12.2.7 Toggle pills

```html
<div class="toggles" role="group" aria-label="{Group}"><button type="button" aria-pressed="true">{Option}</button>…</div>
<div class="toggles pick" role="group" aria-label="{Pick one}"><button type="button" aria-pressed="false" class="busy" aria-label="{T2}, busy">{T2} <small>· 4</small></button>…</div>
```

```css
.toggles { display: flex; flex-wrap: wrap; gap: 14px; margin: 4px 0 12px; }
.toggles button { cursor: pointer; display: inline-flex; align-items: center; font: 700 13.5px var(--ui); padding: 8px 16px 8px 8px; border: 0;
  border-radius: 999px; background: var(--face); color: var(--muted); gap: 10px; box-shadow: var(--raise-s); transition: box-shadow 0.2s, color 0.2s; }
.toggles button::before { content: ""; width: 34px; height: 20px; border: 0; border-radius: 999px; box-shadow: var(--sink-s);
  background: radial-gradient(circle at 10px 50%, color-mix(in srgb, var(--muted) 60%, transparent) 6px, transparent 6.5px); }
.toggles button[aria-pressed="true"] { color: var(--ink); }
.toggles button[aria-pressed="true"]::before, .toggles button.busy::before { box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.25);
  background: radial-gradient(circle at 24px 50%, var(--paper) 6.5px, transparent 7px), var(--gface); }
.toggles button.busy { color: var(--accent-ink); } /* B.23 */
.toggles button:hover { color: var(--ink); box-shadow: var(--raise); }
.toggles button small { font-size: inherit; letter-spacing: 0.06em; }
.toggles button:active { box-shadow: var(--sink-s); }
.toggles.pick { margin: 6px 0 4px; }
.toggles.pick button { padding: 10px 18px; }
.toggles.pick button::before { content: none; }
.toggles.pick button[aria-pressed="true"] { color: var(--on-gface); background: var(--gface); }
.toggles.pick button.down { text-decoration: line-through; opacity: 0.6; }
@media (pointer: coarse) { .toggles button { padding: 9px 12px; } }
```

One `::before` gradient draws the switch (knob jumps, text fades). Pick-one fills ink. `.busy` (taken by something else) is colour only, so its state **MUST** be in the `aria-label`; `.down` is unavailable but visible.

### 12.3 Drawers `details.dock`

```html
<details class="dock" open>
  <summary><span class="dock-t">{Verb}</span><span class="dock-s">{v1} · {v2}</span><i class="dock-c" aria-hidden="true"></i></summary>
  <div class="dock-b">…controls, moved, not copied…</div>
</details>
```

```css
.dock { min-width: 0; margin: 6px 0 18px; border-radius: var(--r, 16px); background: var(--surf, var(--paper2)); box-shadow: var(--raise-s, none); }
.dock > summary { list-style: none; display: flex; align-items: center; gap: 12px; min-height: 46px; padding: 10px 16px; cursor: pointer; }
.dock > summary::-webkit-details-marker { display: none; }
.dock > summary:focus-visible { outline: 1px solid var(--accent); outline-offset: 2px; }
.dock-t { flex: none; font: 700 13.5px var(--ui, var(--sans)); }
.dock-t::before { content: ""; display: inline-block; width: 13px; height: 9px; margin-right: 9px; vertical-align: 1px;
  background: linear-gradient(var(--accent), var(--accent)) 0 0/100% 1.5px no-repeat, linear-gradient(var(--accent), var(--accent)) 0 50%/100% 1.5px no-repeat,
    linear-gradient(var(--accent), var(--accent)) 0 100%/100% 1.5px no-repeat; }
.dock-s { flex: 1; min-width: 0; contain: inline-size; font: 600 12.5px var(--ui, var(--sans)); color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.dock-c { flex: none; width: 8px; height: 8px; border-right: 1.5px solid var(--ink); border-bottom: 1.5px solid var(--ink);
  transform: rotate(45deg) translateY(-2px); transition: transform 0.2s; }
.dock[open] > summary .dock-c { transform: rotate(-135deg) translate(-2px, -1px); }
.dock[open] > summary .dock-s { visibility: hidden; }
.dock-b { padding: 4px 16px 16px; }
.dock-b > .ctrls, .dock-b > .sig-ctrls, .dock-b > .fields { margin: 0; }
.dock-b > .ctrls + .lab, .dock-b > .runrow { margin: 14px 0 0 !important; }
@media (prefers-reduced-motion: reduce) { .dock-c { transition: none; } }
```

```js
var wide = matchMedia("(min-width: 701px)"), docks = [], touched = false;
function dock(nodes, title, sum, shut) {         // built, nodes moved into .dock-b
  function upd() { var t = sum(); if (s.textContent !== t) s.textContent = t; }
  ["input", "change", "click"].forEach(function (ev) { d.addEventListener(ev, function () { setTimeout(upd, 0); }); });
  d.open = !shut && wide.matches; upd(); docks.push({ d: d, upd: upd, shut: shut });
}
// each summary click: touched = true (the first manual toggle ends auto-management)
wide.addEventListener("change", function () { if (!touched) docks.forEach(function (k) { if (!k.shut) k.d.open = wide.matches; }); });
setInterval(function () { docks.forEach(function (k) { if (!k.d.open) k.upd(); }); }, 1000);   // self-changing state
```

- Verb title ("Adjust", "Edit inputs", "Admin") after a three-line accent glyph. Summary: values with units joined by `" · "`, state last (`… · paused`), ≤ 40 characters. `contain: inline-size` keeps it from widening the column.
- Primary drawers open ≥ 701px, closed below; `shut` ones (signature figure, A4/A5 admin) start closed. Popover: §15.

### 12.4 Readouts, groove, verdict, status line

```css
.nums { display: flex; gap: 26px; flex-wrap: wrap; margin-top: 14px; font: 700 11px var(--ui); letter-spacing: 0.06em; text-transform: uppercase; color: var(--muted); }
.nums b { display: block; font: 400 clamp(28px, 7vw, 40px)/1.05 var(--serif); letter-spacing: -0.01em; color: var(--ink); text-transform: none; }
.track { display: flex; height: 8px; border-radius: 8px; overflow: hidden; background: var(--surf); box-shadow: var(--sink-s); }
.track i { transition: width 0.05s linear; }
.track .d { background: var(--ink); }
.track .f { background: var(--accent); }
@media (prefers-reduced-motion: reduce) { .track i { transition: none; } }
.verdict { margin-top: 24px; font: 400 clamp(18px, 4.4vw, 24px)/1.3 var(--serif); min-height: 32px; max-width: 820px; }
.verdict b { font-weight: 400; color: var(--accent-ink); } /* B.23 */
```

- **Readout** `<span>{Label}<b>0</b></span>`: raw integers, time `{n}s`, rewritten per tick, no tween, never live.
- **Groove:** two `<i>`, only `width` (%) written; outcome ink, the argued part accent; `aria-hidden`. 50ms just exceeds the 45ms tick.
- **Verdict:** one sentence at the end, cleared on reset. **Status line** (A4, A5; 2.6em, §13): one sentence per action, saying its consequence; A5 falls back to a counter summary after 5000ms of silence. Every slot reserves its height.

### 12.5 Guided demo and auto-play

`Simulate` runs a script through the user's own functions. Its `setTimeout` handles are kept; `stopDemo()` clears them and re-enables the button, and **every** user handler calls it first: the user wins. Timing: prerequisite step + 2200ms, lead-in 1300ms, step gap `clamp(2600 / steps, 170, 520)`ms, commit + 500ms (4–6s in all); reduced motion caps each wait at 120ms. Auto-play once, 900ms after the stage is first > 35% visible (§14), only if untouched, idle and motion is allowed.

### 12.6 Figure archetypes

Chosen by B §9.2; each at most once; at most two 3D besides the signature object; data invented, labelled.

| | Work is about | Required parts | Note pattern |
|---|---|---|---|
| A1 Race | two strategies under one limit | drawer, run, two lanes, verdict | `simulated {units}, real {rule}` |
| A2 Calculator → document | inputs → a document | fields, statement, download | `invented figures, real {file}` |
| A3 Before/after | design quality | switch, foil, good form, rationale | `{version one} is wrong by design` |
| A4 3D scene + tab | a place people act in | Simulate, stage, pickers, tab, list | `three.js, invented {data}` |
| A5 3D flow + tracker | stages, hand-offs, retries | stage, tab, tracker, 6 readouts, list | `three.js · simulated {flow}` |
| A0 No figure | better told than touched | none | none |

#### 12.6.1 A1 · Race

```html
<div class="race">
  <div class="lane"><h5>{Strategy A}</h5><small>{its rule}</small>
    <div class="track" aria-hidden="true"><i class="d" id="figNad" style="width:0"></i><i class="f" id="figNaf" style="width:0"></i></div>
    <div class="nums"><span>{Outcome}<b id="figNadn">0</b></span><span>{Loss}<b id="figNafn">0</b></span><span>Time<b id="figNat">0s</b></span></div></div>
  <div class="lane">…ids figNb…</div>
</div>
```

```css
.race { display: grid; grid-template-columns: 1fr 1fr; }
.lane { padding: 4px 30px 4px 0; }
.lane + .lane { padding: 4px 0 4px 30px; border-left: 1px solid var(--rule); }
.lane h5 { font: 400 26px/1.1 var(--serif); margin: 0 0 2px; letter-spacing: -0.01em; }
.lane small { display: block; font: 400 14px var(--sans); color: var(--muted); margin-bottom: 14px; }
@media (max-width: 700px) { .race { grid-template-columns: 1fr; } .lane { padding-right: 0; }
  .lane + .lane { border-left: 0; padding-left: 0; border-top: 1px solid var(--rule); margin-top: 20px; padding-top: 20px; } }
```

```text
TICK_MS = 45 (one simulated second, ≈ 22 Hz); lane = {init(params), step(), t, done, ok, lost}; ids {P}d {P}f {P}dn {P}fn {P}t
run(): reset(); run.disabled = true; read inputs ONCE; every TICK_MS each unfinished lane t++, step()
  widths = count / total × 100 + "%"; unfinished work shows as loss once its lane stops
  both done → clear, re-enable, verdict "{A} … {tA}s but only <b>{pA}%</b> ({lost} lost). {B} … {tB}s, <b>{pB}%</b>. {moral}"
```

Units are Bernoulli trials (`Math.random() >= p`). Ranges **MUST** keep the claim true everywhere, else the verdict branches (§3.9); defaults finish in 1–6s. Lanes stack at 621–700; ≤ 620 they pair again, compact: gap 12px, no rule, `h5` 17px, `small` hidden, labels 10.5px, numerals 22px.

#### 12.6.2 A2 · Calculator → document

```html
<div class="calc"><div class="fields">…paired fields + a currency select…</div>
  <div><div class="slip"><div class="head"><span>{DOC TITLE}</span><span>{name}</span></div><table>
    <tr><td>{Item}, {qty} × {rate}</td><td>{CUR} 1,000.00</td></tr>…<tr class="total"><td>{Total}</td><td aria-live="polite"></td></tr></table></div>
    <div class="runrow" style="margin:16px 0 0"><button class="btn solid" id="figNPdf" type="button">Download {doc} (PDF)</button></div></div></div>
```

```css
.calc { display: grid; grid-template-columns: 1fr 1fr; gap: 44px; }
.slip { padding: clamp(14px, 4vw, 24px) clamp(12px, 4vw, 26px); border: 0; border-radius: 26px; background: var(--surf); box-shadow: var(--raise); }
.slip .head { display: flex; justify-content: space-between; align-items: baseline; gap: 12px; padding-bottom: 14px; border-bottom: 1px solid var(--ink); margin-bottom: 6px; }
.slip .head span:first-child, .slip tr.total td:first-child { font: 700 11px var(--ui); letter-spacing: 0.06em; text-transform: uppercase; }
.slip .head span:last-child { font: 400 italic 24px var(--serif); }
.slip table { width: 100%; border-collapse: collapse; font-size: 15px; }
.slip td { padding: 9px 0; border-bottom: 1px solid var(--hair); vertical-align: baseline; }
.slip td:last-child { text-align: right; font-variant-numeric: tabular-nums; white-space: nowrap; }
.slip tr.total td { border-bottom: 0; padding-top: 16px; border-top: 1px solid var(--ink); }
.slip tr.total td:last-child { font: 400 clamp(26px, 7vw, 40px)/1 var(--serif); letter-spacing: -0.01em; }
@media (max-width: 800px) { .calc { grid-template-columns: 1fr; gap: 26px; } }
@media (max-width: 460px) { .calc { grid-template-columns: minmax(0, 1fr); } }
```

≤ 300px: name 18px, table 13px; ≤ 240px: total 24px nowrap; ≤ 220px: amounts wrap, 12px.

```text
num = parseFloat, finite and > 0 else 0 (silent); JS clamps mirror HTML min/max; name = trim() || "{noun}"
last = {name, cur, inputs, derived}: one snapshot, so screen and file never disagree
every "input" → set cells (rows built once) by textContent, name escaped; only the total cell is live
money = cur + " " + n.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})
file: lazy loader (§2.7) warms jsPDF on first pointerenter/focus; click → then(save, () => window.print())
save: h = draw(new jsPDF({unit: "mm", format: [80, 400]}))                 // pass 1: measure on a tall probe
      doc = new jsPDF({unit: "mm", format: [80, Math.ceil(h)]}); draw(doc)  // pass 2: cut to fit
      title "{Doc} (invented data)"; "{doctype}-{slug}-{YYYY}-{MM}.pdf", slug = kebab of safe(name, 30) || "{fallback}"
draw: Courier on a narrow roll, 80 wide, margin 6, returns y + 6; dashes [0.9, 0.9], dot leaders [0.15, 0.85], working 7.5pt grey;
      title, "{INVENTED DATA}" stamp, meta, items, subtotal, adjustments, double rule, total 11 bold, grey honesty lines
```

Labels show their working; `−` (U+2212) on screen, ASCII `-`, ISO codes, `"en-US"` and `?` beyond Latin-1 in the file. The note says the download is real; stamp and footer say the data is not.

#### 12.6.3 A3 · Before/after

```html
<div class="seg" role="group" aria-label="{Form} version"><button type="button" id="figNBefore" aria-pressed="false">Before</button>
  <button type="button" id="figNAfter" aria-pressed="true">After</button></div>
<div class="uxwrap"><div id="figNHost"></div><ul class="why" id="figNWhy"></ul></div>
```

```css
.uxwrap { display: grid; grid-template-columns: minmax(0, 5fr) minmax(0, 5fr); gap: 44px; align-items: start; }
@media (max-width: 800px) { .uxwrap { grid-template-columns: 1fr; gap: 26px; } }
.form { display: grid; gap: 18px; }
.form .err { font: 300 14px var(--sans); min-height: 20px; margin-top: 3px; color: var(--accent-ink); } /* B.23 */
.okmsg { font: 400 italic 24px var(--serif); min-height: 30px; }
.why { list-style: none; margin: 0; padding: 0; counter-reset: w; }
.why li { counter-increment: w; display: grid; grid-template-columns: 40px 1fr; padding: 13px 0; border-top: 1px solid var(--hair); font-size: 15.5px; line-height: 1.55; }
.why li:first-child { border-top: 0; padding-top: 0; }
.why li::before { content: counter(w); font: 400 italic 26px/1 var(--serif); color: var(--accent); }
.why li b { font-weight: 400; text-decoration: underline 1.5px var(--sand); text-underline-offset: 0.22em; } /* B.23 */
```

- `show(which)` sets both buttons, rebuilds form and rationale, enhances only the good form's selects; opens on "after".
- **Foil** `form.form.before[aria-label="{Form}, before"]`, hard-coded, light even in dark mode: Arial 11px `#000` on `#e9e9e9`, `border: 2px outset #d0d0d0`, padding 8px, gap 2px; flex rows gap 4px, labels `min-width: 70px`; fields `1px solid #777` on `#fff`, padding `1px 3px`, `appearance: auto`, `flex: 1`, **`border-radius: 0; box-shadow: none`** (else the pill rule leaks in and it looks designed); buttons `2px 8px` on `#ddd`; error `red`. Shows 5–7 of: cryptic abbreviations, seven equal fields, `type="text"` everywhere, a `--` first option, letter codes, three identical buttons, one generic error (`role="alert"`). Keeps `label for`: bad UX, not broken access.
- **Good form** (`novalidate`): 3–4 fields, labels saying what and why, example placeholders, `autocomplete`, `inputmode`, a pre-selected default, one verb + object `.btn.solid`, an `.err` per field (`aria-describedby`, `aria-invalid`), `.okmsg[role=status]`.
- **Validation:** on `blur` only; submit runs all, then focuses the first invalid field. Messages instruct with an example and a reason ("Add {parts} so {who} can {act}."), never "invalid", never red.
- **Rationale:** one `<li><span><b>{2–5 word lead}</b> {reason}.</span></li>` per fix.

#### 12.6.4 A4 · 3D scene + browser tab

Use for a place people act in. Run row above (`{Primary}`, label flips with state · `Simulate {a user}` · `Reset`); `.scene3` = `.stage` + `.scene-side` (`.toggles` in a shut drawer · `.toggles.pick` stations · `.tab`, §13); then `.tk-h` + `ol.tickets`, status `.verdict`.

Each scene action has an HTML twin; scene labels are `aria-hidden`. A 3D pick calls `stopDemo()`, selects, fires `want` (§2.8); ≤ 900px the sheet opens 520ms later, closes 480ms after the primary action (§13). No WebGL: buttons disabled.

#### 12.6.5 A5 · 3D flow + tracker

`.scene3` = `.stage.sq` + `.scene-side` (`.tab` built once · `ol.steps` · `.steps-now` · `.steps-log`, §13); one shut drawer: `.ctrls` (rate, failure), route `.toggles`, `.runrow` (`Pause`/`Run`, `Reset`); six readouts; `.tk-h` + `ol.orders`; status `.verdict`.

- One token per item keeps one logical position through every change of form; halo, tracker, log and row all read it. Spotlight: the user's item, else the newest; a finished user item holds 4000ms.
- Spawn accumulator starts at 0.92 (first item at once); rate 6–90/min (30), failure 0–40% (8%); > 4 tries → retry in 2.2s. DOM ≤ 4 Hz; 6 recent rows; a readout **MAY** prove a guarantee by staying 0.
- Pause freezes moves, not rendering; the user's action resumes. Reduced motion starts paused; Reset → `setRun(!reduce)`.

#### 12.6.6 A0 · No figure

No tray; L5 (giant numeral, §10.6.5) or L7. Also for the weaker of two features wanting one archetype, or a third 3D figure. Neighbours without figures **MUST NOT** both lack a quote (§10.8).

### 12.7 Pitfalls and adapting

- Override `--surf`, never `--face` or shadows (§4.6.3). Never rebuild focused controls. Only verdict, status and total cell are live.
- Note ≤ 6 words (≈ 28 characters at 390); title 3–5 words, else clamp max ≈ 44px. `.ctrls` 210px fits ≈ 34-character labels, else 240px. Two-line verdict: `min-height: calc(2 * 1.3em)`.
- A1: swap step rules, keep lane API. A2: even field count. A3: a form from the person's domain.

---

## 13 · Browser-tab mock & compact figures (≤900px)

A 3D figure's side panel is a browser window moulded from the tray; ≤ 900px it folds into a launcher that grows into a bottom sheet; receipts become a spring row. Owns B.17, B.18, D.10. CSS is exact; buttons are reset (`border:0;background:none;padding:0;font:inherit;cursor:pointer;text-align:left`) unless stated.

### 13.1 The browser-tab mock

Purpose: the user's side of the model (the scene is the operator's): a raised window, sunk URL pill, body set like a page.

```html
<div class="tab" id="figNTab"><div class="tab-bar"><i aria-hidden="true"></i><i aria-hidden="true"></i><i aria-hidden="true"></i><span class="url" id="figNUrl">{app.example/path}</span></div>
  <div class="tab-body" id="figNList" aria-live="polite"><p class="empty">{What to do first.}</p>
    <button type="button" class="mi" data-i="7" aria-pressed="false"><span><em>2×</em>{Item}<small>{Sub-line}</small></span><span>{12.50} +</span></button></div>
  <div class="tab-foot"><span id="figNTotal">{No items yet}</span><button class="btn solid" id="figNGo" type="button" disabled>{Verb}</button></div></div>
```

```css
.tab{display:flex;flex-direction:column;min-height:min(330px,70svh);border:0;border-radius:26px;overflow:hidden;background:var(--surf);box-shadow:var(--raise)}
.tab-bar{display:flex;align-items:center;gap:6px;padding:12px 14px}
.tab-bar i{width:26px;height:26px;border:0;border-radius:50%;display:grid;place-items:center;font:700 13px/1 var(--ui);font-style:normal;color:var(--muted);background:var(--face);box-shadow:2px 2px 5px var(--lo),-2px -2px 5px var(--hi)}
.tab-bar i:nth-child(1)::before{content:"\2190"}.tab-bar i:nth-child(2)::before{content:"\2192";opacity:.45}.tab-bar i:nth-child(3)::before{content:"\21BB"}
.tab-bar .url{flex:1;color:var(--muted);overflow:hidden;text-overflow:ellipsis;white-space:nowrap;margin-left:8px;padding:6px 14px;border-radius:999px;background:var(--surf);box-shadow:var(--sink-s);font:600 12.5px/1.4 var(--ui)}
.tab-body{flex:1;padding:10px clamp(12px,3vw,18px) 6px;max-height:clamp(220px,52svh,340px);overflow:auto}
#figNList{flex:none;height:clamp(220px,52svh,340px)}
.tab-body .empty{margin:24px 0;font:400 italic 21px/1.3 var(--serif);color:var(--muted)}
.tab-body h6{margin:12px 0 2px;font:400 11px var(--sans);text-transform:uppercase}
.tab-body h6:first-child{margin-top:2px}
.mi{display:flex;width:100%;justify-content:space-between;align-items:baseline;gap:10px;padding:7px 0;border:0;border-bottom:1px solid var(--hair);background:none;color:var(--ink);font:300 15px/1.35 var(--sans);text-align:left;cursor:pointer;border-bottom-color:color-mix(in srgb,var(--ink) 8%,transparent)}
.mi span:last-child{color:var(--muted);font-variant-numeric:tabular-nums;white-space:nowrap}
.mi em{font-style:normal;margin-right:6px}
.mi small{display:block;font:400 10.5px/1.4 var(--sans);text-transform:uppercase;color:var(--muted)}
.mi.sel{padding-left:12px;border-radius:14px;background:var(--face);box-shadow:var(--raise-s)}
.mi:disabled{opacity:.45;cursor:not-allowed}
.tab-foot{display:flex;justify-content:space-between;align-items:center;gap:10px;font:300 14px var(--sans);padding:12px 18px 18px}
.tab-foot .btn{padding:11px 20px}
@media (max-width:620px){.tab{min-height:0}.tab-body .empty{margin:14px 0}}
@media (max-width:460px){.tab-foot{flex-wrap:wrap}}
```

- **Finishing (§5).** Micro-labels: Nunito 700 `.06em`; heads, counts, selected name, tracker "now": `--accent-ink`.
- **States.** Hover underlines the first span. Faded → = no forward history. Choose-one rows: `.sel` + `aria-pressed`; add-one rows: `em` count, trailing ` +`. `#figNGo` is disabled until there is something to act on.
- **Rules.** A variable-length body **MUST** be fixed-height. Why: the tray would jump. Re-rendered bodies get `aria-live="polite"`; built-once ones do not (4 updates/s). Glyph `aria-hidden`:. Side column ≥ 340px at 1280px. ≤ 300: glyphs hidden.

### 13.2 Panel sub-patterns

**Ticket cards** are receipts: `li.tk[data-t="{numeric key}"]` holding `.tk-top`, `.tk-more`, `ul`, `.sum` with `button[data-close]`; ink rule under the title, hairline over the total. Empty `.none` rows (full width, block): italic serif, muted, 20px / 19px.

```css
.tickets{list-style:none;margin:10px 0 0;padding:0;min-height:230px;display:grid;grid-template-columns:repeat(auto-fill,minmax(min(220px,100%),1fr));gap:12px}
.tickets li.tk{padding:12px 14px;font:300 14px/1.5 var(--sans);border-radius:20px;background:var(--face);box-shadow:var(--raise-s)}
.tk-top{display:flex;justify-content:space-between;align-items:baseline;border-bottom:1px solid var(--ink);padding-bottom:6px;margin-bottom:6px}
.tk-top b{font:400 22px/1 var(--serif)}.tk-top span{font:400 10.5px var(--sans);text-transform:uppercase}
.tk ul{list-style:none;padding:0}.tk li{display:flex;justify-content:space-between;gap:10px}
.tk .sum{display:flex;justify-content:space-between;align-items:center;margin-top:8px;padding-top:6px;border-top:1px solid var(--hair)}
.tk .sum b{font:400 20px var(--serif)}.tk .sum button{padding:0 0 2px;font:700 12.5px var(--ui)}
```

**Tracker** (`ol.steps` of six `li`, `p.steps-now`, `p.steps-log`): sunk → raised → accent.

```css
.steps{list-style:none;margin:0;padding:0;counter-reset:s}
.steps li{counter-increment:s;display:grid;align-items:center;grid-template-columns:40px 1fr;font:400 14.5px/1.35 var(--sans);color:var(--muted);padding:9px 10px 9px 6px;border-radius:14px}
.steps li::before{content:counter(s);display:grid;place-items:center;width:26px;height:26px;border-radius:50%;font:700 11.5px/1 var(--ui);background:var(--surf);box-shadow:var(--sink-s)}
.steps li.done{color:var(--ink)}
.steps li.done::before{background:var(--face);color:var(--ink);box-shadow:3px 3px 7px var(--lo),-3px -3px 7px var(--hi)}
.steps li.now::before{animation:pulse 1.4s ease-in-out infinite;background:var(--accent);color:var(--on-accent);box-shadow:2px 2px 5px var(--lo),-2px -2px 5px var(--hi)}
@keyframes pulse{50%{box-shadow:0 0 0 5px color-mix(in srgb,var(--accent) 25%,transparent)}}
.steps-log{margin:6px 0 0;font:400 italic 17px/1.4 var(--serif);min-height:48px}
html.sheet-on .steps li.now::before{animation-play-state:paused}
@media (prefers-reduced-motion:reduce){.steps li.now::before{animation:none}}
@media (min-width:621px){.steps-now{display:none}}
@media (max-width:620px){.steps{display:grid;grid-template-columns:repeat(6,minmax(0,1fr));align-items:center}
.steps li{display:flex;justify-content:center;padding:4px 0;font-size:0;position:relative}
.steps li:not(:last-child)::after{content:"";position:absolute;left:calc(50% + 15px);right:calc(-50% + 15px);top:50%;height:1.5px;background:color-mix(in srgb,var(--ink) 14%,transparent)}
.steps li.done:not(:last-child)::after{background:var(--accent)}
.steps-now{margin:2px 0 0;font:700 13px var(--ui,var(--sans))}.steps-log{font-size:15.5px;min-height:0}}
```

One 50% keyframe: the disc breathes, never glows. Reduced-motion rule:. On phones `.steps-now` ("Step 2 of 6 · {label}") restores hidden labels. N steps: `repeat(N,…)`.

**Recent activity** (`li[.fol]`: `b` id, `span` item, `span` "{First → Final}", `em.st0…st5`; six rows):

```css
.orders{list-style:none;margin:10px 0 0;padding:0;border-top:1px solid var(--ink);min-height:246px}
.orders li{display:grid;grid-template-columns:64px minmax(0,1.2fr) minmax(0,1.6fr) auto;gap:4px 14px;align-items:baseline;padding:9px 0;border-bottom:1px solid var(--hair);font:300 14.5px/1.4 var(--sans)}
.orders li.fol{box-shadow:inset 2px 0 0 var(--accent);padding-left:10px}
.orders b{font:400 18px var(--serif)}.orders span:nth-of-type(2){color:var(--muted)}
.orders em{font:400 10.5px var(--sans);font-style:normal;text-transform:uppercase;color:var(--muted);white-space:nowrap}
@media (max-width:620px){.orders{min-height:132px}.orders li{grid-template-columns:auto 1fr auto}.orders span:nth-of-type(2){grid-column:1/-1;order:5;font-size:13px}.orders li:nth-child(n + 4){display:none}}
```

The followed row's bar is an inset shadow. Why: the box never shifts.

**≤ 620.** Readouts (`.nums.shnums`, §12) become raised mini cards: `repeat(3,minmax(0,1fr)) !important`, gap 8px, `span` padding 8px 10px, radius 12px, `--face` + `--raise-s`, 10.5px label, `b` 22px (34px wide); ≤ 460 two columns. A built-once picker (`#figNPick`) becomes two-column tiles (gap 8px; `h6`, `.toggles` span `1/-1`): `.mi{flex-direction:column;gap:2px;padding:9px 11px;border-radius:12px;font-size:14px}` raised; `.sel` adds `inset 0 0 0 1.5px var(--accent)`, so size holds. Pick-one pills (`#figNStations`) become a sideways strip (`flex-wrap:nowrap;overflow-x:auto;padding:6px 4px 10px`, no scrollbar, buttons `flex:none`). Why: padding keeps shadows.
### 13.3 Side panel in lock-step with a scene

1. **MUST** build focusable controls once, then update in place (`textContent`, `disabled`, `classList.toggle`, `aria-pressed`); only read-only lists use `innerHTML`. Why: rebuilds kill focus.
2. **MUST** compare every write (`if(box.innerHTML!==h)`). Why: no churn, re-announcement or observer loops (generated markup is already serialised).
3. Update the DOM at ≤ 4 Hz (`if(T-lastOut<0.25)return`) plus right after user actions; lists rebuild only when `dirty`. One followed record drives halo (§14), tracker, log and `.fol`.
4. Live regions reserve `min-height`.

### 13.4 Compact system (≤900)

```css
.tab-pop,.tk-more,.tk-count{display:none}
@media (max-width:900px){.has-pop>.tab{display:none}.tk-count{display:inline;margin-left:.6em;color:var(--accent)}}
```

`cmp` (`max-width:900px`) and `still` (reduced motion) are live; widening calls `done()`.

**Launcher** (before the tab; parent `.has-pop`): raised capsule; sunk well with window and black cursor; muted URL over status; ink "Open" pill.

```html
<button type="button" class="tab-pop" aria-haspopup="dialog" aria-expanded="false" aria-label="Open the tab: {url}, {status}"><span class="tp-ico"><svg viewBox="0 0 46 40" aria-hidden="true"><rect class="w" x="1.5" y="2.5" width="36" height="29" rx="6"/><path class="w" d="M1.5 10.5h36"/><circle class="d" cx="6.5" cy="6.6" r="1.25"/><circle class="d" cx="10.5" cy="6.6" r="1.25"/><circle class="d" cx="14.5" cy="6.6" r="1.25"/><path class="r" d="M7 16.5h14M27 16.5h4M7 21.5h10M27 21.5h4M7 26.5h12"/><circle class="tp-ring" cx="29" cy="23" r="4.5"/><path class="tp-cur" d="M29 23v15.2l3.9-3.6 2.9 6.4 2.8-1.3-2.9-6.3 5.3-.4z"/></svg></span><span class="tp-txt"><span class="tp-url">{url}</span><span class="tp-sub">{status}</span></span><span class="tp-go" aria-hidden="true">Open</span></button>
```

```css
@media (max-width:900px){
.tab-pop{display:grid;grid-template-columns:auto minmax(0,1fr) auto;align-items:center;gap:14px;width:100%;margin:2px 0 8px;padding:11px 12px 11px 11px;border-radius:22px;background:var(--face);color:var(--ink);box-shadow:var(--raise);transition:box-shadow .2s}
.tab-pop:active{box-shadow:var(--sink)}.tab-pop:focus-visible{outline:2px solid var(--accent);outline-offset:3px}
.tp-ico{width:58px;height:50px;border-radius:15px;display:grid;place-items:center;background:var(--surf);box-shadow:var(--sink-s);color:var(--muted)}
.tp-ico svg{width:44px;height:38px;overflow:visible}
.tp-ico .w{fill:none;stroke:currentColor;stroke-width:1.6}
.tp-ico .r{fill:none;stroke:currentColor;stroke-width:1.5;stroke-linecap:round;opacity:.55}
.tp-ico .d{fill:currentColor}
.tp-cur{fill:var(--ink);stroke:var(--surf);stroke-width:1.5;stroke-linejoin:round;transform-box:fill-box;transform-origin:0 0}
.tp-ring{fill:none;stroke:var(--accent);stroke-width:1.6;opacity:0;transform-box:fill-box;transform-origin:50% 50%}
.tab-pop.hint .tp-cur{animation:tpCur 1.5s cubic-bezier(0.3,0.7,0.3,1) 2}
.tab-pop.hint .tp-ring{animation:tpRing 1.5s ease-out 2}
@keyframes tpCur{0%{transform:translate(5px,6px)}38%{transform:translate(0,0)}48%{transform:translate(0,0) scale(0.84)}60%,100%{transform:translate(0,0)}}
@keyframes tpRing{0%,44%{opacity:0;transform:scale(0.4)}52%{opacity:1}100%{opacity:0;transform:scale(2.3)}}
.tp-txt{display:grid;gap:2px;min-width:0}
.tp-url,.tp-sub{white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.tp-url{font:600 12.5px/1.35 var(--ui);color:var(--muted)}.tp-sub{font:300 15.5px/1.3 var(--sans)}
.tp-go{font:700 13.5px/1 var(--ui);padding:10px 15px;border-radius:999px;background:var(--gface);color:var(--on-gface);box-shadow:var(--raise-s);transition:background .2s,color .2s}
.tab-pop:hover .tp-go{background:var(--accent);color:var(--on-accent)}}
@media (prefers-reduced-motion:reduce){.tab-pop.hint .tp-cur,.tab-pop.hint .tp-ring{animation:none}}
```

`fill-box` scales the cursor about its tip, the ring about its centre. ≤ 300: no pill, icon 46×42. **Hint:** a `MutationObserver` on URL and status writes `.tp-url`, `.tp-sub` (`sub[status]||status`), `aria-label` only on change; a URL change calls `hint()`: if not open and not `still`, remove `.hint`, `void pop.offsetWidth` (reflow restarts it), add `.hint`; ring `animationend` removes it. An `IntersectionObserver` (`threshold:1`) plays it once on first full view.

**Sheet: `popover="manual"`, not `<dialog>`.** Why: `showModal()` inerts the page by restyling every element, heavy beside WebGL; a manual popover still gets the top layer, and the script does the dialog's jobs. Fallback `z-index:1000`. The live `.tab` **MUST** be moved, not cloned. Why: ids and listeners survive.

```html
<div class="sheet" role="dialog" aria-modal="true" aria-label="{What this tab is}" popover="manual"><div class="sheet-scrim"></div>
  <div class="sheet-in" tabindex="-1"><span class="sheet-grab" aria-hidden="true"></span><!-- .tab moves here --></div></div>
<!-- added to .tab-bar: <button type="button" class="tab-x" aria-label="Close the tab">×</button> -->
```

```css
.sheet{display:none;position:fixed;top:0;right:0;bottom:0;left:0;z-index:1000;margin:0;padding:0;width:auto;height:auto;max-width:none;max-height:none;border:0;background:none;color:var(--ink);overflow:hidden;overscroll-behavior:contain;flex-direction:column;justify-content:flex-end;align-items:center;--surf:var(--paper2)}
.sheet.on{display:flex}
.sheet-scrim{position:absolute;top:0;right:0;bottom:0;left:0;background:rgba(12,11,9,.58);touch-action:pinch-zoom;will-change:opacity}
.sheet-in{position:relative;display:flex;flex-direction:column;width:min(640px,100%);max-height:100%;padding:0 10px calc(10px + env(safe-area-inset-bottom,0px));transform-origin:50% 0;will-change:transform,opacity}
.sheet-in:focus{outline:none}
.sheet-grab{align-self:stretch;height:26px;flex:none;display:grid;place-items:center;touch-action:none;cursor:grab}
.sheet-grab::before{content:"";width:44px;height:5px;border-radius:5px;background:rgba(246,244,236,.7)}
.sheet .tab{display:flex;flex:1 1 auto;min-height:0;max-height:calc(100svh - 46px - env(safe-area-inset-bottom,0px));box-shadow:0 -8px 40px -14px rgba(0,0,0,.5)}
.sheet .tab-bar{touch-action:none;cursor:grab}
.sheet .tab-body,.sheet #figNList,.sheet #figNPick{flex:1 1 auto;height:auto;max-height:none;min-height:0;overflow:auto;overscroll-behavior:contain}
.tab-x{display:none}
.sheet .tab-x{display:grid;place-items:center;flex:none;width:32px;height:32px;margin-left:6px;border-radius:50%;background:var(--face);font:600 19px/1 var(--ui);box-shadow:2px 2px 5px var(--lo),-2px -2px 5px var(--hi)}
html.sheet-on{overflow:hidden;scrollbar-gutter:stable}
```

```js
var E="cubic-bezier(.2,.85,.25,1)";
function onLauncher(r){ if(!r||r.height===0||r.bottom<0||r.top>innerHeight) return null; // off screen: slide up
  var p=pane.getBoundingClientRect(),t=tab.getBoundingClientRect(),s=Math.min(1,r.width/t.width),
      dx=r.left+r.width/2-(p.left+p.width/2),dy=r.top-(p.top+(t.top-p.top)*s);
  return "translate("+dx.toFixed(1)+"px,"+dy.toFixed(1)+"px) scale("+s.toFixed(4)+")"; }
function show(){ if(open||!cmp.matches) return; clearTimers(); var r=pop.getBoundingClientRect(); // before the move
  open=true; going=false; stop(); pane.appendChild(tab); sheet.classList.add("on"); if(layer) sheet.showPopover();
  root.classList.add("sheet-on"); pop.setAttribute("aria-expanded","true"); hold(true);
  if(window.CloseWatcher){ cw=new CloseWatcher(); cw.onclose=function(){ cw=null; hide(); }; }
  picker(); x.focus({preventScroll:true}); if(still.matches) return fadeIn(160);
  var from=onLauncher(r);
  anims.push(pane.animate(from?[{transform:from,opacity:0},{opacity:1,offset:0.3},{transform:"none",opacity:1}]:[{transform:"translateY(100%)"},{transform:"none"}],{duration:from?440:380,easing:E}),
    scrim.animate({opacity:[0,1]},{duration:300,easing:"ease"})); }
function hide(dragged){ if(!open||going) return; clearTimeout(tGo); drag=null; if(still.matches||!cmp.matches) return done();
  var cp=getComputedStyle(pane),y0=cp.transform,p0=+cp.opacity,o0=+getComputedStyle(scrim).opacity; // from NOW
  going=true; stop(); var to=dragged?null:onLauncher(pop.getBoundingClientRect());
  var a=pane.animate(to?[{transform:y0,opacity:p0},{opacity:p0,offset:0.55},{transform:to,opacity:0}]:[{transform:y0},{transform:"translateY(105%)"}],
    {duration:to?340:260,easing:to?"cubic-bezier(.4,0,.2,1)":"cubic-bezier(.4,0,1,1)",fill:"forwards"});
  anims.push(a,scrim.animate({opacity:[o0,0]},{duration:300,easing:"ease",fill:"forwards"})); a.onfinish=done; }
// done(): flags off; stop(); clearTimers(); cw.destroy(); hidePopover(); drop .on, inline styles, .tp-pick, html.sheet-on;
//   home.insertBefore(tab,pop.nextSibling); aria-expanded=false; rAF: if(!open) hold(false); pop.focus({preventScroll:true})
function hold(on){ document.dispatchEvent(new CustomEvent("sheetchange",{detail:{open:on}})); }
function dStart(e){ if(!open||going||e.button>0||e.target.closest("button")) return; // on .sheet-grab and .tab-bar
  drag={y:e.clientY,t:performance.now(),dy:0,v:0,id:e.pointerId}; e.currentTarget.setPointerCapture(e.pointerId); }
function dMove(e){ if(!drag||e.pointerId!==drag.id) return;
  if(anims.some(function(a){return a.playState==="running"})){ drag.y=e.clientY; return; }
  var now=performance.now(),dy=Math.max(0,e.clientY-drag.y); drag.v=(dy-drag.dy)/Math.max(1,now-drag.t); drag.t=now; drag.dy=dy;
  pane.style.transform=dy?"translate3d(0,"+dy+"px,0)":""; scrim.style.opacity=dy?Math.max(0.2,1-dy/480).toFixed(3):""; }
function dEnd(){ if(!drag) return; var d=drag; drag=null; if(d.dy>90||(d.dy>16&&d.v>0.5)) return hide(true);
  var o0=scrim.style.opacity||1; pane.style.transform=scrim.style.opacity=""; // spring back unless still:
  if(d.dy>0&&!still.matches) anims.push(pane.animate([{transform:"translate3d(0,"+d.dy+"px,0)"},{transform:"none"}],{duration:240,easing:E}),scrim.animate({opacity:[o0,1]},{duration:240})); }
```

× hovers `--accent`, presses `--sink-s`. The pane stays opaque, fading only near the launcher. Why: it reads as one object. Close **MUST** start from the computed state. Why: no jump. Reduced motion: 160ms fade in, instant close.

- **Focus.** Open → ×; Escape → `hide()`; Tab cycles the pane's enabled, visible `button,[href],input,select,textarea,[tabindex]:not([tabindex='-1'])`, wrapping both ends; `focusin` outside → ×; close → launcher.
- **Re-renders.** Body `focusin` records the nearest `[data-i]`/`[data-p]`/`[data-g]`; on body `childList`: `picker()`, and if focus fell to `<body>`, refocus that key or the pane. Re-rendered controls **MUST** carry stable `data-*` keys.
- **Chooser.** An `.empty` body gets a clone of the outside chooser, `.toggles.tp-pick[role=group]` (`margin:4px 0 14px`, buttons `padding:10px 18px`, no `::before`); clones call `src.click()`.
- **Coupling.** `sheetchange` makes stages simulate without drawing (§14). A 3D pick fires `want` on the `.tab`; the sheet opens **520ms** later; a `go` click inside closes it **480ms** later. Why: a pick never teleports; the scene reacts first, the result lands before the sheet leaves. API: `sheet({tab,status,go,pick?,name,sub})`.

### 13.5 Ticket row (≤900)

```css
@media (max-width:900px){
.tk-scroll{--ip:clamp(12px,3.4vw,32px);margin:6px calc(-1 * var(--ip)) 0;overflow-x:auto;overflow-y:hidden;overscroll-behavior-x:contain;scrollbar-width:none}
.tk-scroll::-webkit-scrollbar{display:none}
.tk-scroll>.tickets{--tkw:min(236px,76vw);position:relative;display:block;margin:0;padding:8px var(--ip) 16px;min-height:0;overflow:hidden}
.tk-scroll li.tk{position:absolute;left:0;top:0;width:var(--tkw);display:flex;flex-direction:column;overflow:hidden;padding:12px 14px;transform-origin:50% 30%;backface-visibility:hidden}
.tk-scroll .tk-top{flex:none}
.tk-scroll li.tk ul{display:none;flex:0 1 auto;min-height:0;overflow:hidden;margin:0 0 8px}
.tk-scroll li.tk.open ul,.tk-scroll li.tk.closing ul{display:block}
.tk-scroll .tk .sum{flex:none;margin-top:auto;padding-top:8px}
.tk-scroll .tk-more{display:flex;align-items:center;gap:7px;width:100%;margin:4px 0 8px;padding:7px 10px 7px 12px;border-radius:12px;background:var(--surf);box-shadow:var(--sink-s);font:600 12.5px/1.35 var(--ui);color:var(--muted)}
.tk-more i{flex:none;width:7px;height:7px;margin:0 3px 3px 0;border-right:1.5px solid var(--ink);border-bottom:1.5px solid var(--ink);transform:rotate(45deg);transition:transform .25s}
.tk.open .tk-more i{transform:rotate(-135deg) translate(-2px,-2px)}
.tk-more:focus-visible{outline:2px solid var(--accent);outline-offset:2px}}
```

`.tk-more[aria-expanded][aria-controls]` holds `b` count (700, ink), `.tk-p` ellipsis preview (`flex:1;min-width:0`), `.tk-l` "Show less" (replaces it when `.open`), `i[aria-hidden]`. The row bleeds to the tray edge; the next card peeks at 320px wide.

```text
G 12 · K 240 · C 23 · SWIRL 520 · height 380/36 · stagger 0.045s · new card +48px, fade 320ms ease-out · dt 0.001–0.032
per numeric key k: x y vx vy h vh tx ty th d; sg = +k%2 ? 1 : −1
measure(): stash + clear inline height/class → closedH; "open" on the open card → openH; restore (no paint between)
layout(fresh): fresh = no motion (boot, resize, breakpoint, fonts); not compact → clear inline styles, state, openKey
 W = card width; hc = max closedH; oi = open index or −1; he = oi<0 ? hc : openH; R = oi<0 ? 1 : max(1,floor((he+G)/(hc+G)))
 card i: oi<0 or i≤oi → col=i,row=0; else j=i−oi−1; col=oi+1+floor(j/R); row=j%R
  tx=padL+col(W+G); ty=padT+row(hc+G); th = open ? he : closedH; style.left/top = tx/ty  // FLIP
  new → x=tx+(fresh?0:48), fade; moved >1px and |vx|+|vy|<20 → d=0.045·wave++
 fresh or still → snap; paint; run rAF
paint(): transform = translate3d(x−tx,y−ty,0) rotate(clamp(vx·0.0028,±3.5)deg) scale(1+min(0.035,|v|/26000)), "" at rest
 height = settled(|h−th|<0.5,|vh|<1) ? "" : h; box = ceil(max(max(x,tx)+W)+padR) × ceil(max(ty+h)+padB)
frame(): d>0 → d−=dt; else a=K(t−p)−Cv; dist>1 → a += min(1,dist/180)·SWIRL·sg·(−dy,dx)/dist; v+=a·dt; p+=v·dt
 dist<0.3 and |vx|+|vy|<3 → snap; vh += (380(th−h)−36vh)·dt; h += vh·dt; settled → drop "closing"; loop while moving
```

R stacks closed cards beside the open one. Position ζ ≈ 0.74 overshoots slightly; height ζ ≈ 0.92 does not (derived). The swirl fades over the last 180px: cards curve, then land straight. **Toggle:** old card `.closing`, new `.open`; its `ul` fades `[0, 0 at 25%, 1]` over 380ms; `layout(false)`; scroll in with 12px margins. Rebuild: re-apply `.open`, restore focus, scroll new cards in. `layout(true)` on `cmp` change, width-only resize (Why: a height trigger loops), `fonts.ready`, boot. Reduced motion: snap, scroll `auto`, no caret transition.

---

## 14 · 3D stages & scene technique

A 3D figure (archetypes A4 and A5, §12.6) is ink drawn on the page. A transparent WebGL canvas sits in a sunk `--paper` well. The scene is unlit hairlines over tracing-paper fills, and the camera turns gently and always comes home. Owns B.16 and D.06. The tab, sheet and readouts are in §13, controls in §12. CSS is minified and its values are exact. Example ids use `figN…`.

### 14.1 The stage box

**Purpose.** A well pressed into the `--paper2` tray. It holds the canvas, a label overlay, a caption strip and a pill of zoom keys.

```html
<div class="scene3">
  <div class="stage" id="figNHost">                <!-- .stage.sq: scenes deep rather than wide -->
    <canvas id="figNCanvas" tabindex="0" role="img"
      aria-label="A 3D {scene}: {what is where}. Drag or use the arrow keys to turn it, plus and minus to zoom. Click a {station} to {act}."></canvas>
    <div class="tags" id="figNTags" aria-hidden="true"></div>
    <span class="stage-hint lab" aria-hidden="true">Fig. {{n}} · Drag to turn · pinch or +/− to zoom</span>
    <!-- Stage() appends div.zoom last -->
  </div>
  <div class="scene-side">…tab or tracker (§13)…</div>
</div>
```

```css
.scene3{display:grid;grid-template-columns:minmax(0,7fr) minmax(0,4fr);gap:30px;align-items:start}
.scene3>*{min-width:0}
.scene-side{display:grid;gap:12px;align-content:start}
.stage{position:relative;width:100%;aspect-ratio:7/6;background:var(--paper);overflow:hidden;max-height:88svh;border:0;border-radius:22px;box-shadow:var(--sink-s)}
.stage.sq{aspect-ratio:1/1}
.stage canvas{display:block;width:100%;height:100%;cursor:grab;touch-action:pan-y;outline-offset:-2px;border-radius:22px}
.stage canvas.grab{cursor:grabbing}
.stage canvas.hot{cursor:pointer}
.stage-hint{position:absolute;left:14px;bottom:10px;pointer-events:none;right:62px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.stage-hint.lab{font-size:11px;letter-spacing:0}
.stage.zoomed .stage-hint{opacity:0}
.stage.tiny .stage-hint{display:none}
.stage-fail{margin:0;padding:40px 20px;font-size:15px;color:var(--muted);text-align:center}
.zoom{position:absolute;display:flex;flex-direction:column;right:14px;bottom:14px;border:0;border-radius:999px;gap:10px;padding:8px;background:var(--surf);box-shadow:var(--raise-s);-webkit-touch-callout:none;user-select:none}
.zoom button{width:34px;height:34px;border:0;color:var(--ink);cursor:pointer;padding:0;border-radius:50%;font:600 19px/1 var(--ui);background:var(--face);box-shadow:3px 3px 7px var(--lo),-3px -3px 7px var(--hi);transition:box-shadow .2s,color .2s}
.zoom button:hover:not(:disabled){background:var(--face);color:var(--accent-ink)}
.zoom button:active:not(:disabled){box-shadow:var(--sink-s)}
.zoom button:disabled{color:var(--muted);opacity:.45;cursor:default}
.stage.tiny .zoom button{width:28px;height:28px;font-size:17px}
@media (pointer:coarse){.zoom button{width:40px;height:40px}.stage.tiny .zoom button{width:36px;height:36px}.stage-hint{right:76px}}
```

Caption: DM Mono 11px muted (§12.1). `.zoom` takes the tray's `--surf`: a raised capsule on the well, 50 × 138px. Why `right:62px`: 14 inset + 34 key + 2 × 8 padding, so the ellipsis stops before the keys; coarse `76px` is an for the 56px pill.

**States.** `.zoomed` (zoom > 1.03): caption fades, one finger pans. `.tiny` (host < 300px): small keys, no caption, 7px/10.5px labels. `canvas.grab` dragging; `.hot` over a pickable. Keys `disabled` at the limits. If WebGL is missing, `fail()` replaces the host's content with `<p class="stage-fail">This interactive 3D figure needs WebGL, which this browser has turned off.</p>` and the figure disables its buttons. The well stays.

**Responsive.**

| Query | `.stage` | `.stage.sq` | Labels |
|---|---|---|---|
| default | 7/6 | 1/1 | 10.5px · `b` 17px |
| `(max-width:900px)` | `.scene3` one column, gap 22px | | |
| `(max-width:900px) and (min-width:701px)` | 4/3 | 4/3 | — |
| `(max-width:700px)` | 1/1 | 4/5 | 8.5px · `b` 13px |
| `(max-height:520px) and (orientation:landscape)` | `max-height:82svh` | same | — |

When `max-height` binds, the box grows wider than its ratio, and framing still fits both axes.

**Accessibility.** Only the canvas is focusable; its label teaches the controls. Labels and caption are `aria-hidden`, their state repeated in the side panel. The ring (§17) is drawn inside by `outline-offset:-2px`. The pill is `role="group" aria-label="Zoom"`.

**Pitfalls.** Never paint the well in WebGL: transparent over CSS `--paper` recolours for free. No border; `--sink-s` is the edge.

### 14.2 Labels pinned to 3D points

```css
.tags{position:absolute;inset:0;pointer-events:none;overflow:hidden}
.tag{position:absolute;left:0;top:0;white-space:nowrap;font:700 10.5px/1.25 var(--ui);letter-spacing:.06em;text-transform:uppercase;color:var(--muted);transform:translate(-50%,-100%);will-change:transform;transition:color .3s,opacity .3s}
.tag b{display:block;font:400 17px/1.05 var(--serif);letter-spacing:-.005em;text-transform:none;color:var(--ink)}
.tag.off{opacity:.38}
.tag.gone{opacity:0}
.tag.on,.tag.on b{color:var(--accent-ink)}
@media (max-width:700px){.tag{font-size:8.5px}.tag b{font-size:13px}}
.stage.tiny .tag{font-size:7px;letter-spacing:.08em}
.stage.tiny .tag b{font-size:10.5px}
```

Effective rule (B.23 overrides B.16). Markup is `<b>{Name}</b>{status}`, with a serif name over a tiny caps line. The name is optional. States: `.on` for a selected or active object, `.off` for a disabled one, `.gone` for one not present yet, which fades in. Labels move **only** by `transform: translate(Xpx,Ypx) ` + the anchor `t.off`:

| `t.off` | Use |
|---|---|
| `translate(-50%,-100%)` (default) | object names, point 0.3–0.6 above the object's top |
| `translate(0,8px)` | zone labels hanging below-right of a floor corner |
| `translate(-100%,-50%)` | row markers, left of the point |

Project to NDC; if `z > 1` or `|x|`/`|y| > 1.3`, `visibility:hidden`. Why 1.3: labels past the frame are clipped by `overflow:hidden`, so they slide out instead of blinking. No occlusion or collision handling: keep names ≤ 12 characters, statuses two or three words.

### 14.3 `Stage(o)`: API

It returns `null` (after `fail()`) when `THREE` is undefined or `WebGLRenderer` throws.

| Option | Meaning |
|---|---|
| `host`, `canvas`, `tags` | the `.stage` (observed; gets `.zoomed`, `.tiny`, `.zoom`), its canvas, the `.tags` overlay |
| `yaw`, `pitch` (rad) | resting angles (become `yaw0`, `pitch0`) |
| `target` (Vector3) | the point orbited and looked at |
| `fit` `[halfW, halfH]` | world half-extents that must stay in view, in the view plane through the target |
| `view(aspect)` → `{key,yaw,pitch,target,fit}` | optional; called on each resize; a new `key` replaces the framing and resets pan (zoom kept) |

| Member | Notes |
|---|---|
| `scene`, `camera` | `PerspectiveCamera(30, 1, 0.1, 120)` |
| `glows`, `tags`, `pick` | glows follow resizes; label records; meshes for the raycaster (non-recursive) |
| `yaw pitch yaw0 pitch0 target fit zoom pan` | read/write state; `pan` moves in x/z only |
| `visible seen held viewKey` | `held`: a full-screen sheet covers the page |
| `tag(html, point, cls?)` | → `{el, p, set(html), off}`; mutate `t.p` to follow a mover; `set` writes only on change |
| `resetView()` / `start()` | back to rest / resize, keys, loop |
| `onFrame(dt, t)` | every frame while visible, **also while held**; `dt ≤ 0.05`, `t = now/1000` |
| `onPick(obj)` · `onSeen()` · `onTheme(P)` | tap on a `pick` mesh · once at > 35% visible · first drawn frame and each token change |

Contract: `if(!S){disable buttons;return}`; push glows **before** `S.start()` (the first resize sets `uScale`); build, set hooks, then `S.start()`.

### 14.4 Camera, framing, compositions

```js
function dist(){var vf=deg(camera.fov),hf=2*Math.atan(Math.tan(vf/2)*camera.aspect);
  return Math.max(S.fit[0]/Math.tan(hf/2),S.fit[1]/Math.tan(vf/2))/S.zoom;}
function place(){var d=dist();aim.copy(S.target).add(S.pan);
  camera.position.set(aim.x+d*Math.cos(S.pitch)*Math.sin(S.yaw),aim.y+d*Math.sin(S.pitch),aim.z+d*Math.cos(S.pitch)*Math.cos(S.yaw));
  camera.lookAt(aim);camera.updateMatrixWorld();}   // labels are projected before render()
```

Yaw 0 is on +z looking toward −z, and pitch is the elevation. The narrow 30° FOV gives an architectural-model look. Whichever axis binds wins, and zoom is a dolly, so the perspective never changes.

**Two compositions.** `view(aspect)` returns wide or tall. Put the threshold between the tablet ratio (4:3) and the phone ratios.

| Scene | Threshold | Wide | Tall |
|---|---|---|---|
| square-ish room (`.stage`) | `a < 1.1` | yaw −0.2, pitch 0.6, target (0, 0.85, −0.5), fit [4.85, 3.05] | yaw −0.12, pitch 0.72, target (0, 0.6, −0.3), fit [4.95, 3.5] |
| long flow (`.stage.sq`) | `a < 0.9` | yaw −0.04, pitch 0.92, target (0.3, 0, −0.5), fit [5.45, 2.8] | yaw 1.5, pitch 0.98, target (0.3, 0, −0.2), fit [3.55, 5.0] |

On a portrait box, turn a long scene about 90° so its long axis runs down the screen, swap the fit extents and raise the pitch a little.

### 14.5 Input model

| Gesture | Rule (constants verbatim) |
|---|---|
| turn | `yaw = clamp(yaw − dx·0.006, yaw0 ± 0.9)`, `pitch = clamp(pitch + dy·0.004, 0.3, 1.3)`; `vy = −dx·0.006`. The view never goes under the floor or behind the scene |
| fling | after release, yaw only: `yaw += vy` (clamped), `vy *= 0.002^dt`, stops at `|vy| ≤ 0.0004` |
| drag start | 5px Manhattan threshold, then `.grab` + `setPointerCapture` (try/catch) |
| pan mode | right-drag, Shift-drag, or any drag at zoom > 1.03; `contextmenu` prevented |
| pan | `wpp = 2·dist·tan(fov/2)/H`; ground right `(cos yaw,0,−sin yaw)` × `−dx·wpp`; ground forward `(−sin yaw,0,−cos yaw)` × `dy·wpp/max(0.35, sin pitch)` (undoes foreshortening); clamp x,z to `±1.1·fit[0]`; pan = 0 at zoom ≤ 1.001 |
| zoom | range 0.8–4.5. Ray hits ground `y=0` at G; `k = 1 − z0/z1`; `pan.x += (G.x−aim.x)·k`, same for z: a homothety about G, so G keeps its pixel. `+` is disabled at ≥ 4.49, `−` at ≤ 0.81 |
| Ctrl/⌘ + wheel | `zoomAt(Math.exp(−deltaY·0.01), cursor)`, `{passive:false}` + `preventDefault`. A plain wheel is never captured |
| pinch | factor `d_new/d_old` about the midpoint; midpoint motion pans; the release cannot be a tap |
| keys | ←/→ `turnBy(∓14,0)`, ↑/↓ `turnBy(0,±15)`, `+`/`=` ×1.25, `-`/`_` ×0.8 about the centre, `0` reset. `preventDefault` only for these; no fling |
| pill | `+` ×1.4, `−` ÷1.4 about the centre, `⟲` `resetView()` |
| tap | `pointerup`, not moved, one pointer → `onPick(hit)`; a second tap within 320ms → reset. `pointercancel` is never a tap |
| touch | `touch-action:pan-y`: vertical swipes scroll, sideways swipes turn. At zoom > 1.03 JS sets `touchAction="none"` so one finger pans; a reset restores `pan-y` |

Why: **a canvas never traps a scrolling finger or wheel.** Picks hit invisible proxies in `S.pick` (a cylinder around an object, `MeshBasicMaterial({visible:false})`, id in `userData`), still raycast in r149. Mouse hover toggles `.hot`.

### 14.6 Loop and skeleton

| Concern | Rule |
|---|---|
| visibility | `IntersectionObserver` `[0, 0.35]`. Off-screen means no rAF at all. Re-entry restarts with a fresh clock. `onSeen` fires once at > 0.35 |
| dt | `min((now − last)/1000, 0.05)` |
| held | `document` `sheetchange` `{detail:{open}}`. Only `onFrame` runs (the simulation keeps going), with no theme check, camera, labels or render |
| theme | when `frame % 20 === 1`, compare `themeKey()`; on change call `onTheme(palette())` |
| idle return | 6s after the last turn/pan/zoom, not zoomed, motion allowed: `f = 1 − e^(−0.8·dt)` toward rest (τ 1.25s) |
| resize | `ResizeObserver(host)`: `setSize(w,h,false)`, aspect, `view()` swap, `uScale`, `.tiny`. If held, render once, because resizing clears the WebGL buffer under the scrim |

```js
function Stage(o){
  var host=o.host,canvas=o.canvas,renderer,deg=THREE.MathUtils.degToRad;
  function fail(){host.innerHTML='<p class="stage-fail">This interactive 3D figure needs WebGL, which this browser has turned off.</p>';}
  if(typeof THREE==="undefined"){fail();return null;}
  try{renderer=new THREE.WebGLRenderer({canvas:canvas,antialias:true,alpha:true});}catch(e){fail();return null;}
  renderer.setClearColor(0x000000,0);renderer.setPixelRatio(Math.min(window.devicePixelRatio,2));
  var scene=new THREE.Scene(),camera=new THREE.PerspectiveCamera(30,1,0.1,120),ZMIN=0.8,ZMAX=4.5;
  var S={scene:scene,camera:camera,glows:[],tags:[],pick:[],yaw:o.yaw,pitch:o.pitch,yaw0:o.yaw,pitch0:o.pitch,
         target:o.target,fit:o.fit,zoom:1,pan:new THREE.Vector3(),visible:false,seen:false,held:false};
  var W=1,H=1,pts={},down=false,moved=false,vy=0,idle=0,pinch=null,lastTap=0,last=performance.now(),frame=0,key="",loop=null,raf=0;
  /* S.tag, placeTags (§14.2); dist, place (§14.4); setNdc, hit, turnBy, panBy, zoomAt, clampPan (§14.5) */
  function resize(){var w=host.clientWidth,h=host.clientHeight;if(!w||!h)return;W=w;H=h;
    renderer.setSize(w,h,false);camera.aspect=w/h;camera.updateProjectionMatrix();
    if(o.view){var v=o.view(camera.aspect);if(v.key!==S.viewKey){S.viewKey=v.key;S.yaw=S.yaw0=v.yaw;S.pitch=S.pitch0=v.pitch;S.fit=v.fit;S.target=v.target;S.pan.set(0,0,0);}}
    var sc=h*renderer.getPixelRatio()/2/Math.tan(deg(camera.fov/2));S.glows.forEach(function(g){g.uni.uScale.value=sc;});
    host.classList.toggle("tiny",w<300);if(S.held){place();placeTags();renderer.render(scene,camera);}}
  S.resetView=function(){S.zoom=1;S.pan.set(0,0,0);S.yaw=S.yaw0;S.pitch=S.pitch0;vy=0;zoomed();};
  var zb=document.createElement("div");zb.className="zoom";zb.setAttribute("role","group");zb.setAttribute("aria-label","Zoom");
  zb.innerHTML='<button type="button" data-z="in" aria-label="Zoom in">+</button><button type="button" data-z="out" aria-label="Zoom out">−</button><button type="button" data-z="fit" aria-label="Reset the view">⟲</button>';
  host.appendChild(zb);   // one delegated click: fit → resetView(); in/out → zoomAt(1.4 or 1/1.4, canvas centre)
  function zoomed(){var z=S.zoom>1.03;canvas.style.touchAction=z?"none":"";host.classList.toggle("zoomed",z);
    zb.querySelector('[data-z="in"]').disabled=S.zoom>=ZMAX-0.01;zb.querySelector('[data-z="out"]').disabled=S.zoom<=ZMIN+0.01;}
  /* pointerdown/move/up/cancel, contextmenu, wheel, keydown: §14.5 */
  new ResizeObserver(resize).observe(host);
  document.addEventListener("sheetchange",function(e){S.held=!!(e.detail&&e.detail.open);});
  new IntersectionObserver(function(es){S.visible=es[0].isIntersecting;
    if(S.visible&&es[0].intersectionRatio>0.35&&!S.seen){S.seen=true;if(S.onSeen)S.onSeen();}
    if(S.visible&&loop&&!raf){last=performance.now();raf=requestAnimationFrame(loop);}},{threshold:[0,0.35]}).observe(host);
  S.start=function(){resize();zoomed();loop=function(now){raf=0;if(!S.visible)return;raf=requestAnimationFrame(loop);
    var dt=Math.min((now-last)/1000,0.05);last=now;frame++;
    if(S.held){if(S.onFrame)S.onFrame(dt,now/1000);return;}
    if(frame%20===1){var k=themeKey();if(k!==key){key=k;if(S.onTheme)S.onTheme(palette());}}
    if(!down&&!pinch){if(Math.abs(vy)>0.0004){S.yaw=clamp(S.yaw+vy,S.yaw0-0.9,S.yaw0+0.9);vy*=Math.pow(0.002,dt);}
      idle+=dt;if(idle>6&&!reduce&&S.zoom<=1.03){var f=1-Math.exp(-dt*0.8);S.yaw+=(S.yaw0-S.yaw)*f;S.pitch+=(S.pitch0-S.pitch)*f;}}
    if(S.onFrame)S.onFrame(dt,now/1000);place();placeTags();renderer.render(scene,camera);};   // sim → camera → labels → draw
    raf=requestAnimationFrame(loop);};
  return S;
}
```

### 14.7 Colour: CSS tokens to WebGL

```js
var probe=document.createElement("span");probe.style.cssText="position:absolute;visibility:hidden;pointer-events:none";document.body.appendChild(probe);
var cv1=document.createElement("canvas");cv1.width=cv1.height=1;var cx1=cv1.getContext("2d",{willReadFrequently:true});
function readColor(v,fb){try{probe.style.color="var("+v+")";var c=getComputedStyle(probe).color;
  cx1.clearRect(0,0,1,1);cx1.fillStyle="#010203";cx1.fillStyle=c;
  if(cx1.fillStyle==="#010203"&&c!=="rgb(1, 2, 3)")return fb;   // unparseable: assignment ignored
  cx1.fillRect(0,0,1,1);var d=cx1.getImageData(0,0,1,1).data;return [d[0]/255,d[1]/255,d[2]/255];}catch(e){return fb;}}
var rootStyle=getComputedStyle(document.documentElement);
function themeKey(){return ["--ink","--accent","--paper","--paper2","--muted"].map(function(v){return rootStyle.getPropertyValue(v);}).join("|");}
function palette(){return{ink:readColor("--ink",[0.07,0.07,0.06]),acc:readColor("--accent",[0.33,0.4,0.2]),
  paper:readColor("--paper",[0.96,0.96,0.95]),paper2:readColor("--paper2",[0.92,0.92,0.9]),muted:readColor("--muted",[0.4,0.4,0.37])};}
// css(c) → "rgb(r,g,b)" for 2D-canvas textures; tint(mat,c) → mat.color.setRGB(c[0],c[1],c[2])
```

Why: the span resolves the token as the cascade sees it now, and the 1×1 canvas turns any colour format into sRGB bytes. The probe runs only when the key changes, while visible. Call `palette()` at build time too, for textures.

### 14.8 Materials and helpers

```js
function Mats(){var m={
  ink:new THREE.LineBasicMaterial({transparent:true,opacity:0.8}),     // object edges
  soft:new THREE.LineBasicMaterial({transparent:true,opacity:0.32}),   // walls, furniture, zones
  faint:new THREE.LineBasicMaterial({transparent:true,opacity:0.12}),  // floor grid
  acc:new THREE.LineBasicMaterial({transparent:true,opacity:0.85}),    // live / chosen
  fill:new THREE.MeshBasicMaterial({transparent:true,opacity:0.85,polygonOffset:true,polygonOffsetFactor:1,polygonOffsetUnits:1})};
  m.apply=function(P){tint(m.ink,P.ink);tint(m.soft,P.ink);tint(m.faint,P.ink);tint(m.acc,P.acc);tint(m.fill,P.paper2);};return m;}
function edges(geo,mat,thr){var k="edges"+(thr||1);geo.userData[k]=geo.userData[k]||new THREE.EdgesGeometry(geo,thr||1);return new THREE.LineSegments(geo.userData[k],mat);}
function ghost(a){return new THREE.MeshBasicMaterial({transparent:true,opacity:a,depthWrite:false,side:THREE.DoubleSide});}
```

Further helpers:
- `solid(geo,fill,line,thr)` is a Group of the filled mesh and its `edges`.
- `lines(pts,mat)` turns `[[x,y,z],…]` segment pairs into `LineSegments`.
- `floorGrid(x0,x1,z0,z1,step,mat)` builds an inclusive grid with a 1e-6 epsilon.
- `rect(x0,x1,z0,z1,y)` returns the 8 points of a floor outline.

Fill faces are `--paper2`, one step deeper than the `--paper` well (the tray-on-page step), and about 15% see-through. `polygonOffset` lets the edges win. Edge thresholds are 1° for boxes and 30° for cylinders, discs and rings, so only rims draw. WebGL lines are one device pixel wide: hairlines at DPR 2. Halos are `ghost(0)` accent shapes, a ring `RingGeometry(R,R+0.05,72)` at y 0.006 over a disk at y 0.004. Their opacity eases `a += (target−a)·min(1,5dt)` toward ring 0/0.45/0.9 and disk 0/0.05/0.1 for none/selected/occupied. An arrival flash adds `flash·0.15` (decay 2/s).

### 14.9 Glow, arc, path

```js
var mat=new THREE.ShaderMaterial({uniforms:{uColor:{value:new THREE.Color()},uScale:{value:100}},transparent:true,depthWrite:false,
  vertexShader:"attribute float aSize; attribute float aAlpha; varying float vA; uniform float uScale;"+
    "void main(){ vA=aAlpha; vec4 mv=modelViewMatrix*vec4(position,1.0); gl_PointSize=aSize*uScale/-mv.z; gl_Position=projectionMatrix*mv; }",
  fragmentShader:"uniform vec3 uColor; varying float vA;"+
    "void main(){ float d=length(gl_PointCoord-0.5)*2.0; float a=smoothstep(1.0,0.0,d); a=a*a; gl_FragColor=vec4(uColor, a*vA); }"});
// Points(geo, mat): frustumCulled=false, renderOrder=5. Float32Array position/aAlpha/aSize of capacity max (900 / 1600 busy).
// begin(): n=0. add(x,y,z,a,s): drop if n>=max or a<=0.003. end(): setDrawRange(0,n) + needsUpdate on all three.
```

`uScale = H·pixelRatio/2/tan(fov/2)` is set on resize, so `aSize` is a diameter in **world units**: sprites shrink with distance and grow with zoom. The default `NormalBlending` makes dots tint rather than bloom: nothing glows. `uColor` = `--accent`. Keep `s ≤ 0.15` (GPU point-size caps). Strict GLSL **MAY** use `1.0-smoothstep(0.0,1.0,d)`.

`arc(a,b,h,t,out)` is a quadratic Bézier with control point `(mid.x, max(a.y,b.y)+h, mid.z)`. The apex is `h/2` above equal ends, and it allocates nothing. Drive it with `ease(t) = t<.5 ? 4t³ : 1−(−2t+2)³/2`. `Path(pts).at(s,out)` walks by cumulative length, clamps `s` to `[0,L]` and guards zero-length segments. For heading, look ahead 0.05: `rotation.y = atan2(−(a.z−p.z), a.x−p.x)` if `distanceToSquared > 1e-6` (the model faces +x).

### 14.10 Scenes from primitives

- **Kit.** Boxes, cylinders, hex prisms (`CylinderGeometry(r,r,h,6)`), rings and planes. Draw each as `solid()`, or as edges only in `soft` for secondary furniture. Use raw `lines()` for posts, rails, roofs, crosses, and the strokes that draw "text" on a panel.
- **Room.** `floorGrid(-5,5,-4.5,4,0.5,M.faint)` plus at most a back-wall outline in `soft`: an open drafting box about 10 × 8 units.
- **Zones** are `rect(…, y)` outlines lifted 0.003–0.01 (halos 0.004–0.006), so nothing z-fights the grid. Draw the active zone in `ink` and the rest in `soft`.
- **No lights.** Use only `Basic` materials and the glow shader; hierarchy is line opacity.
- **Textures** are tiny canvas textures only. A 512×128 slip face with `--paper`, an 8px accent spine and the page `--sans` by name gets `anisotropy=4` and is redrawn on `document.fonts.ready` and on theme. A pixel tile uses `NearestFilter`, with its bits generated once.
- **Economy.** One scaled unit box per gauge (`scale.y = max(0.02, 0.36·level/MAX)`, grown from its base). One `LineSegments` per rack system. Shared geometries and pooled meshes. Scale from 0.001, never 0 (singular matrix).

### 14.11 Motion techniques

**Spring + swirl** (objects sorting into slots):

```js
var d=tmp.subVectors(tgt,c.p),w=Math.min(1,d.length()/1.6)*5.5;   // swirl fades to 0 within 1.6 units
acc.copy(d).multiplyScalar(10).addScaledVector(c.v,-5.2);          // k 10, c 5.2 (zeta ~0.82)
acc.x+=w*Math.sin(c.p.y*1.7+T*1.3+c.seed);
acc.y+=w*0.55*Math.cos(c.p.z*1.4+T*1.1+c.seed);
acc.z+=w*Math.sin(c.p.x*1.2-T*0.9+c.seed);
c.v.addScaledVector(acc,dt);c.p.addScaledVector(c.v,dt);
c.g.quaternion.slerp(tq,1-Math.exp(-dt*4.5));
```

Objects loop in transit and land dead still. Trail: 7 points back `0.03·v`, alpha `min(1,|v|/3)·f²·0.8`, size `0.13·(1−0.5j/7)`, only above speed 0.25. Stagger: sort `0.05k + 0.12·group` s, scatter `random·0.35` s. A disabled group goes to alpha 0.16 (`min(1,6dt)`) and drops 0.12.

**Arc flights with comets.** `arc(from,to,h,ease(u))`, with h 1.6 across a room or 0.35–0.8 for short legs. The copy billboards (`lookAt(camera.position)`) and shrinks `1−0.6e`. The trail re-samples the same curve: 9 points at `e − j·0.035`, alpha `0.95·f²`, size `0.15·(1−0.5j/9)`. Start each flight at −0.16 s × index and create its mesh lazily. On landing, the object pops in: `s += 6dt`, scale `1−(1−s)³`.

**Path walking.** Walkers move at 2.1 u/s and a carrier at 2.3 u/s. Stops are arc-length marks: clamp at the next one, dwell 0.7 s. The walker trail is 5 samples at 0.07, lifted 0.07, alpha `(1−j/6)·0.5`, size 0.08. Moving labels mutate `t.p`.

**Move scheduler.** One list of `{kind,o,g,from,to,h,t,dur,done}`, stepped in reverse so `done()` can push the next leg. Negative `t` delays a move. Time is `running ? dt : 0`, so pause is a time scale and halos keep breathing. The kinds:

- `arc`: a mesh moves along the arc.
- No mesh: the token is a 10-point comet at alpha 0.95, step 0.03, size 0.12 (dim variant 0.45).
- `echo`: 4 points, alpha `0.45(1−u)`, cut at `u > 0.45`.
- `walk`, `wait` and `fade`. `fade` scales to 0.001 over 0.6 s, then returns the mesh to the pool.

Legs take 0.7–1.0 s, transfers 0.5–0.55 s with a `−k·0.12` stagger. A queue self-compacts by lerping each item to `slot(i)` at `min(1,6dt)`.

**Event pulses** (glow and geometry only):

| Event | Recipe |
|---|---|
| beam | 14 points up to `j·0.09·min(1,3k)`, alpha `(1−k)(1−j/14)·0.9`, size 0.09, 1.1 s |
| ring | 28 points, r 0.1 → R, alpha `(1−k)·0.7`, size 0.08 (with a beam: a scan) |
| ripple | 16–18 floor points, `r0 + (1−a)·0.35…0.45`, alpha `a·0.6`, size 0.07, decay 1.2–2.2/s |
| shake | `x0 + sin(60T)·0.03·a`, decay 1.8/s |
| flash | ghost opacity `0.25 + a·0.7`, decay 1.2/s |
| beacon | alpha `0.32 + 0.22·sin 5T`, size 0.11 |

### 14.12 One object the whole way

Each token keeps a logical `o.pos` that every motion kind updates, while its look changes: comet, solid, hidden in a carrier, solid, fade. One follow halo stays on it: `glow.add(pos.x, pos.y+0.06, pos.z, 0.55+0.25·sin 6T, 0.42)`. While the token is hidden, the halo rides the carrier at y 0.62 (alpha `0.5+…`, size 0.4). Tracker, log and row (§13) read the same record.

**Spotlight hand-over:** `if (mine || !f || (f.done && (!f.mine || now − f.doneAt > 4000))) followed = o`. The user's own item always takes the spotlight. Otherwise a new item takes over only once the followed one finishes, and the user's finished item holds it for 4 s.

### 14.13 Reduced motion and versions

Read `matchMedia("(prefers-reduced-motion: reduce)")`. A `change` listener **SHOULD** keep the flag live. Same story, no travel: no idle return or auto-demo; springs, slerps and staggers snap; flights take 0.01 s; scans run 4×; scripted waits `min(ms,120)`; a continuous simulation starts paused (`Run`) and Reset keeps it paused. User turning and zoom stay.

Ship three.js r149 (`assets/vendor/`); its raw colour values make probed sRGB match CSS exactly. On r152+ you **MUST** set `THREE.ColorManagement.enabled=false` and `renderer.outputColorSpace=THREE.LinearSRGBColorSpace`, or colours wash out.

### 14.14 Pitfalls and adapting

- `setSize` without `false` gives layout loops, because CSS must own the size.
- Custom materials and textures missing from `onTheme` stay in the old theme.
- Zoom about the cursor is exact only at `target.y = 0`.
- One mouse-wheel notch is ×0.37; clamp `deltaY` to ±30.

**Adapting.** `fit` = half-extents + 5–10% + label headroom; thresholds between box ratios; yaw ±0.9, pitch ≥ 0.3. Glow capacity must be at least movers × trail samples + markers. Trail size is about 1–1.5% of the scene width, and arc lift about 20% of the throw. Keep idle ≥ 5 s.

---

## 15 · The signature object

Fig. 0 of the issue: one hand-turnable hairline 3D object pinned in the orbit (§8), chosen to stand for the person's craft. A technical drawing, not a render: ink lines on a transparent canvas, one accent on what is alive, beads that obey gravity as you turn it, and a plain sentence saying what happens. Hooks use the `sig` prefix; `{{orbit.signatureObject}}` names the archetype (15.9).

### 15.1 Purpose

It gives the reader something to *do* and shows the craft without claiming it. One per page; the only thing that glows (§4). The canonical archetype, the default for software and hardware people, is an **integrated-circuit package**: a slab outline, pins on every side, five orthogonally routed power lines from top-edge pins down to a core, and a layered core that brightens as power arrives.

### 15.2 Anatomy

```html
<figure class="sig" id="sigFig">
  <div class="sig-view" id="sigHost">
    <canvas id="sigCanvas" tabindex="0" role="img"
      aria-label="Interactive 3D {{orbit.signatureObject}}. Drag or use the arrow keys to turn it; Q and E roll it."></canvas>
  </div>
  <div class="sig-ui" id="sigUi">
    <div class="sig-ctrls"><!-- D.08 wraps this in details.dock "Adjust", always closed at load (§12) -->
      <label for="rx">Rotate X <output id="oRx">14°</output></label><input id="rx" type="range" min="-180" max="180" value="14">
      <label for="ry">Rotate Y <output id="oRy">-28°</output></label><input id="ry" type="range" min="-180" max="180" value="-28">
      <label for="rz">Rotate Z <output id="oRz">0°</output></label><input id="rz" type="range" min="-180" max="180" value="0">
      <label for="sup">Supply <output id="oSup">0.5× gravity</output></label><input id="sup" type="range" min="0" max="4.4" step="0.1" value="1.1">
    </div>
    <div class="sig-foot">
      <span class="sig-status" id="sigStatus" aria-live="polite">{Measure}: starting.</span>
      <button class="btn" id="sigReset" type="button">Reset</button>
    </div>
  </div>
  <figcaption class="note"><span>{{orbit.aside[0]}}</span><span>{{orbit.aside[1]}}</span></figcaption>
</figure>
```

`<output>` texts are first-paint values, rewritten on init. Supply `0 … 4.4` is `0 … 2G`.

### 15.3 Spec

#### 15.3.1 CSS (base; the pinned overrides are in §8.3.6)

```css
.sig { margin: 0; width: 100%; pointer-events: auto; }   /* inside the pointer-transparent stage */
.sig-view { position: relative; width: 100%; aspect-ratio: 10/9; }
.sig-view canvas { display: block; width: 100%; height: 100%; cursor: grab; touch-action: none; outline-offset: 2px; }
.sig-view canvas.grab { cursor: grabbing; }   /* set while dragging */
.sig-fail { margin: 0; padding: 40px 20px; font-size: 15px; color: var(--muted); text-align: center; }
.sig-ui { position: relative; width: 100%; max-width: 440px; min-width: 0; }
.sig-ctrls { display: grid; grid-template-columns: repeat(2, 1fr); gap: 14px 26px; }
.sig-foot { display: flex; align-items: center; justify-content: space-between; }
.sig-status { font: 300 14px/1.45 var(--sans); }
.sig figcaption { margin: 12px 0 0; justify-content: space-between; gap: 12px; width: 100%; max-width: 440px; }
```

#### 15.3.2 Renderer and camera

```js
if (typeof THREE === "undefined") return fail();
try { renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true }); } catch (e) { return fail(); }
renderer.setClearColor(0x000000, 0);   // paper grain shows through
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
camera = new THREE.PerspectiveCamera(30, 1, 0.1, 60); camera.position.set(0, 0, 8.3);
obj = new THREE.Group(); obj.rotation.order = "ZXY"; scene.add(obj);
```

Why 30° at z 8.3: half-height at z 0 is `8.3·tan 15° = 2.224`, so the ±1.5 object fills 67% of the view, near-axonometric. WebGL lines are 1 device pixel (0.5 CSS px at DPR 2): that *is* the hairline. **MUST NOT** use fat lines; hierarchy is opacity, never width.

#### 15.3.3 Canonical geometry (integrated-circuit archetype)

Authored on a **400 × 400 design grid** (y down, centre 200,200), converted with `U = 100`: `px(x) = (x − 200)/U`, `py(y) = −(y − 200)/U`. `H = 0.07` (half-thickness), `ZL = 0.078` (trace layer). All layers but pins and mark are drawn on **both faces**. Why: every orientation shows a complete drawing, and the parallax of the doubled copies is the only depth cue.

| z (front; back mirrored) | Layer | Size | Material |
|---|---|---|---|
| ±0.07 | slab `BoxGeometry(2.6, 2.6, 0.14)`: mesh + `EdgesGeometry` | design 70…330 | fill + ink |
| 0 | pins | 20 units out | ink |
| ±ZL | passive traces · power lines · mark (front) | — | soft · accent · ink |
| ±(ZL + 0.004) | bead sprites | — | points |
| ±(H + 0.015) | plate, edges of `BoxGeometry(1.64, 1.64, 0.03)` | 118…282 | soft |
| ±(H + 0.025) | core die, edges of `BoxGeometry(0.8, 0.8, 0.05)` | 160…240 | ink |
| ±(H + 0.052) / ±(H + 0.054) | sink fill `PlaneGeometry(0.8, 0.8)` / dot `PlaneGeometry(0.2, 0.2)` | — | core fill / dot |
| ±(H + 0.07) | sink glow (2 sprites) | — | points |

```js
PINS = [82 + 19·i for i in 0..12]                    // 13 per side, pitch 19 (row centred on 196, kept as shipped)
POWER = [120, 158, 196, 234, 272]                    // top-edge pins that carry the live lines
pins: top non-power [p,70]→[p,50]; bottom [p,330]→[p,350]; left [70,p]→[50,p]; right [330,p]→[350,p]   // 47, ink, z 0
passive (soft, ±ZL): for k, i in [1,3,4,7,9,11]: p = 82+19i; a = 84+9(k%3); t = clamp(p + (k%2 ? -22 : 22), 128, 272)
  bottom [p,330][p,316−9(k%3)][t,316−9(k%3)][t,282]; left [70,p][a,p][a,t][118,t]; right [330,p][316−9(k%3),p][316−9(k%3),t][282,t]
mark (ink, front only): [78,78][104,78][78,104][78,78]           // open triangle: tells the faces apart after a flip
DEFS = [{p:120,t:170,a:118},{p:158,t:185,a:106},{p:196,t:200,a:94},{p:234,t:215,a:106},{p:272,t:230,a:118}]
line path: [p,50] → [p,70] → [p,a] → [t,a] → [t,160]               // accent, ±ZL; the pin itself is accent
```

Routing: orthogonal only; landings 15 apart, centred; jog rows in a V so routes nest without crossings. Lengths 1.60 / 1.37 / 1.14 / 1.29 / 1.52 world. Precompute per line `P`, `len`, `cum`, unit `tan`, `L`; `pointAt(s)` clamps to `[0, L]`. 19 draw calls; nothing rebuilt after init.

#### 15.3.4 Materials (all unlit, all transparent)

| Name | Type | Token | Opacity | Draws |
|---|---|---|---|---|
| ink | `LineBasicMaterial` | `--ink` | **0.85** | slab edges, pins, die, mark |
| accent | `LineBasicMaterial` | `--accent` | **0.55** | the five live lines |
| soft | `LineBasicMaterial` | `--ink` | **0.42** | plate, passive traces |
| fill | `MeshBasicMaterial` | `--paper2` | **0.55**, `depthWrite: false` | slab body |
| core fill | `MeshBasicMaterial`, `DoubleSide` | `--accent` | `0.08 + 0.55·bright` | sink square |
| dot | `MeshBasicMaterial`, `DoubleSide` | `--accent` | `0.35 + 0.65·bright` | sink dot |

Fills never write depth, so they never hide a line. The slab reads a shade deeper than the page in light, lighter in dark.

#### 15.3.5 Glow: one `Points` cloud

`BEADS = 3` per line, `TR = 7` sprites per bead, both faces, +2 sink sprites: `NPTS = 5·3·7·2 + 2 = 212`, **one draw call**, the §14.10 shader (world-unit `aSize`, `smoothstep(1,0,d)²`, normal blending). Per frame, per bead:

```text
dir = v ≥ 0 ? 1 : −1;  gap = 0.012 + min(|v|·0.022, 0.05)        // trail stretches with speed
for t in 0..6: pos = pointAt(s − dir·t·gap)                       // trail always behind the motion
  parked: alpha = (t == 0 ? 0.4 : 0), size 0.06                   // dim dot waiting at the pin tip
  moving: alpha = 0.95·(1 − t/7)²,  size = 0.085·(1 − 0.55·t/7)
  write at z = +(ZL+0.004) and −(ZL+0.004)
sink glow ×2 at z = ±(H+0.07): alpha 0.55·bright, size 0.2 + 0.7·bright
```

#### 15.3.6 Bead physics (world space)

```text
G = 2.2  FR = 1.1  V0 = 0.9  D (supply) = 1.1, slider 0 … 4.4 step 0.1
per frame: q = obj world quaternion; ty[j] = (tan[j].x, tan[j].y, 0) rotated by q → .y   // +1 = straight up the screen
substeps: n = max(1, ceil(dt / 0.004)); h = dt / n                                   // ≤ 4 ms, stable at any refresh rate
per bead: if parked: wait −= h; if wait ≤ 0 → on, s = 0.0005, v = V0
  acc = D − G·ty[seg(s)] − FR·v;  v += acc·h;  s += v·h                              // semi-implicit Euler
  s ≥ L → arrived:  E += 1, lastArrive = T, park, wait = 0.2 + 0.5·rand
  s ≤ 0 → slid back: lastReturn = T, park, wait = 0.15 + 0.35·rand                  // it keeps retrying
initial wait = 0.15 + 0.13·line + 0.55·bead + 0.3·rand                               // left-to-right cascade, three waves
```

Gravity is screen-down (world −Y) and the camera never rotates, so turning the object changes every slope. Level terminal speed `D/FR = 1.0`, straight down ≈ 3.0. A segment stalls when `ty > D/G`: at default (0.5× gravity) **any run climbing more than 30° drains back**; at ≥ 1× beads climb vertical runs. At rest the vertical runs fall (ty ≈ −0.97), so the default is flowing. Readout: `(D/G).toFixed(1) + "× gravity"`.

#### 15.3.7 Sink brightening

```text
E *= exp(−dt / 0.9)                                   // arrival energy decays, τ 0.9 s
bright += (1 − exp(−E/3) − bright)·min(1, 8·dt)       // saturating target, eased ≈ 125 ms
```

Full flow (≈ 10 arrivals/s) holds `bright ≈ 0.95`, a near-solid core; a stalled object fades to the 0.08 tint in ≈ 2 s.

#### 15.3.8 Status sentence

Every 250 ms (4 Hz): `flowing` = lines with `T − lastArrive < 1.4`; `drained` = any line with `T − lastReturn < 1.4`. Grammar: one sentence, numerals, full stop.

```text
T < 1.6 and !flowing and !drained → "{Measure}: starting."
else "{Measure}: {flowing} of {N} {units}. {State}."
State: flowing ≥ 0.8·N → {full}; ≥ 1 → {partial}; drained → {draining}; else → {stalled}
```

Canonical: `{Measure}` names the core, `{units}` the lines; states *Flowing · Partly flowing · Draining back · Stalled*. Write `textContent` only on change (no re-announcing). `T` advances only while the loop runs, so "recent" never goes stale.

#### 15.3.9 Theme, sizing, loop

- **Theme**: every 20 frames compare the string `--ink|--accent|--paper2`; on change, resolve through the probe (§14.8) and `setRGB` all materials and `uColor`. No wiring to the switch (§7).
- **Sizing**: `ResizeObserver(host)` → `setSize(w, h, false)`, aspect, `uScale`, re-measure; a second observer on `#orbit`. Why `false`: CSS owns the canvas size, so no feedback.
- **Offscreen**: `IntersectionObserver` stops rAF; reset `last` on re-entry; `dt ≤ 0.05`.
- **Frame order**: theme → fling → scroll yaw → slopes → substeps → sink → sprites → render → status.

### 15.4 States

| State | Look |
|---|---|
| Starting (< 1.6 s) | dim dots parked at the pin tips, faint core |
| Flowing | bright dotted strings on every vertical run, doubled; solid glowing core |
| Partly / draining / stalled | fewer strings; beads slow, stop, slide back; core fades |
| Dragging | `cursor: grabbing`; scroll yaw suspended |
| Held | stays where the visitor left it until **Reset** |
| No WebGL / no library | `#sigHost` holds `<p class="sig-fail">{{orbit.signatureObject}} is drawn with WebGL, which is unavailable here. The note beside it describes it.</p>`; `#sigUi` gets `.hidden`; the 10/9 box stays |

### 15.5 Responsive

| Query | Change |
|---|---|
| `(any-pointer: coarse)` | `.sig-view canvas { touch-action: pan-y; }`: vertical swipes scroll, sideways drags turn |
| `(max-width: 1279px)` | `.orbit .sig-ctrls output { white-space: nowrap; }` |
| `(max-width: 460px)` / `(max-width: 360px)` | `.sig-ctrls { gap: 10px 18px; }` / `grid-template-columns: 1fr` |
| `(max-height: 520px) and (orientation: landscape)` | `.sig-view { max-width: min(520px, 78svh); }` |
| one column (§8.5) | ≤ 560px, bar below, figcaption shown, no scroll turn |
| print | stage and control bar hidden (§17) |

### 15.6 Motion and input

State: `REST = {x: 14, y: −28, z: 0}` (three-quarter view), `rot`, `yaw`, `held`, `vel`; angles wrap to −180…180. `obj.rotation.set(rad(rot.x), rad(rot.y + yaw), rad(rot.z))`, order **ZXY**: Y turns it about its own axis (drag sideways, ←/→, scroll), X tips it (drag vertically, ↑/↓), Z rolls it in the screen plane whatever X and Y are (Q/E).

```text
hold(): if held return; held = true; rot.y = wrap(rot.y + yaw); yaw = 0   // seen angle becomes owned: no jump
drag:   pointerdown → capture, vel = 0, .grab; move: if !held and |dx|+|dy| > 2 → hold()
        rot.y += 0.55·dx; rot.x += 0.55·dy (°/px); vel.y = 33·dx, vel.x = 33·dy (°/s)
fling:  !drag and |vel| > 1.2 °/s → rot += vel·dt; vel *= 0.0006^dt   // per second: same throw at any Hz
keys:   ←/→ y ∓5, ↑/↓ x ∓5, Q/E z ∓5; Shift 15°; hold() first; preventDefault used keys only; vel = 0
sliders: X, Z absolute; Y relative (d = value − round(rot.y); hold(); rot.y += d): no yaw jump
supply: sets D and readout only; not an orientation input, so no hold()
scroll: if !held and !drag and PIN and !reducedMotion:
          t = clamp((scrollY − oTop) / max(1, orbitHeight − innerHeight), 0, 1)
          yaw += ((t − 0.5)·24 − yaw)·min(1, 4·dt)   // ±12°, τ 0.25 s; rotation.y only
Reset:  held = false; rot = REST; vel = 0; supply 1.1; readouts   // yaw eases to the scroll target
```

`PIN` is the §8 query. Metrics are measured on resize; `scrollY` is read in rAF, no scroll listener. Why scroll writes `rotation.y` directly: readouts stay, so scrolling touches no DOM. Why ±12°: it breathes, never spins. A fling **SHOULD** be zeroed when the last move is older than 80 ms.

### 15.7 Accessibility

- Canvas: focusable `role="img"` labelled with every control; Tab never captured; global ring, offset 2px. Keys plus labelled sliders give parity.
- `#sigStatus` is polite-live, written on change only; all the drawing says is also text.
- Reduced motion: no scroll turn, no make-room transition. Beads keep moving: they are content, not decoration.
- Fallback: a sentence, no dead controls.

### 15.8 Pitfalls

1. **`NPTS` counts both faces and the +2 sink sprites.** Why: otherwise trails write past the buffer.
2. **`frustumCulled = false`.** Why: the bounding sphere comes from the zero buffer and culls every bead.
3. **Sink glow vs point-size caps.** Why: some GPUs cap `gl_PointSize` at 64 px and the full glow reaches ≈ 175 device px on a large view; clipping there is acceptable, a quad avoids it.
4. **Probe only opaque tokens.** Why: `getImageData` reads the colour over transparent black.
5. **r152+ needs `THREE.SRGBColorSpace` in `setRGB`.** Why: later versions linearise and wash colours out.
6. **DPR is set once**; re-apply on a `(resolution)` change **MAY** be added.
7. **Transparent sort veils back-face inner layers** under the slab fill but not the two-face traces. Part of the look; `renderOrder` only for a uniform back face.

### 15.9 Adapting: other archetypes

Pick an artefact that (a) reads as a flat-ish wireframe, (b) has channels carrying something to one sink, (c) gives gravity a story. Same 400 grid, 3–7 orthogonal channels (1.1–1.6 world, fanning in, no crossings), same constants and grammar.

| Craft | Structure (hairline) | Beads | Rule (at rest flowing; then) | Status |
|---|---|---|---|---|
| Architect | floor plan, wall outlines, door ticks as pins, stair core sink | people walking corridors | tip the plan and uphill corridors empty | "Stair core: 4 of 5 corridors reaching it. Partly walking." |
| Data scientist | node graph: 5 sources, 2 hidden layers, one output | records on edges | invert and records slide back to sources | "Output: 5 of 5 inputs feeding it. Feeding." |
| Designer | 4 offset artboards at z steps, swatches on the edge | components entering the main frame | upside down, the frame sheds them | "Main frame: 2 of 4 layers composing. Partly composing." |
| Musician | body outline, 6 strings, frets as pins, sound-hole sink | vibrations down each string | neck tilted up, notes fade back | "Sound hole: 6 of 6 strings ringing. Ringing." |
| Writer | bound book open, page edges as pins, spine sink | lines drifting to the spine | tipped, lines slip back to the margin | "Spine: 3 of 5 chapters binding in. Partly binding." |
| Mechanical engineer | gear train (30° edge threshold), shafts, galleries to a sump | oil | rolled over, oil drains back | "Sump: 4 of 4 galleries returning oil. Circulating." |
| Photographer | lens barrel: 5 element rings, aperture blades, sensor sink | light on converging rays | pointed up, rays stall | "Sensor: 5 of 7 rays landing. Partly exposed." |
| Product manager | 4 roadmap rails, milestone ticks, release-gate sink | work items | tilted, items roll back a milestone | "Release gate: 3 of 4 tracks landing. Partly landing." |
| Researcher | burettes, tubing routed to a flask sink | drops | inverted, tubing drains to the reservoirs | "Flask: 4 of 4 lines dripping. Filling." |

Every archetype **MUST** keep:

- Hairline only (tiers 0.85 / 0.55 / 0.42, `--paper2` fill 0.55 without depth write); unlit, depth from two faces and the 30° lens.
- One accent, on live channels, beads and sink only.
- World-space physics `supply − G·ty − FR·v`, ≤ 4 ms substeps, default 0.5 G, slide back and retry.
- Scroll coupling ±12° with `hold()` and Reset; ≤ 1 draw call for glow; energy sink; four-state "n of N" sentence.
- Theme probing, offscreen pause, DPR cap 2, readable fallback.

Re-tuning: for an extent other than ±1.5 set `z = extent / (0.67·tan 15°)`; scale sprite sizes with the object; `REST` shows thickness on two edges and channels run downhill at rest. Longer channels: raise `V0` or lower `FR`, never `G`. Full stays ≥ 80% of N.

---

## 16 · Motion system

This chapter owns the scroll arrivals. Every other moving part is listed in the inventory (16.4) and specified in its home chapter.

### 16.1 Principles

1. **Motion explains place and causality.** Text arrives *from the screen centre* and settles into its column. A sheet grows out of the launcher that opened it. A theme spreads from the button that was pressed. Why: motion that says neither where nor why is decoration.
2. **Springs for travel, tweens for state.** Anything that crosses distance (arrivals, ticket cards, scene objects) is a damped spring that carries velocity. Colour, shadow and caret changes are CSS transitions of 0.2–0.55s.
3. **Only `transform` and `opacity` animate.** The exceptions are `background-size` (pencil underlines) and `box-shadow` (moulded presses). Why: no layout, no reflow.
4. **Forward-only on scroll.** Progress ratchets. Scrolling back never sends anything out again. Why: what was read stays put.
5. **Deterministic.** Page (DOM) motion **MUST NOT** use `Math.random`. Per-element character comes from a hash of the element's index, so every visit moves identically. Scene simulations inside stages (§14) **MAY** use it.
6. **Sleep when idle.** Every loop stops requesting frames when nothing moves or when it is off screen.
7. **Reading wins.** Nothing is hidden without JS or under reduced motion, and nothing is left half-arrived (16.2.5).

### 16.2 Arrivals (`[data-fly]`)

**Purpose.** Blocks rise into place as the reader scrolls; on wide screens they also travel from the viewport's centre to their column, slightly tilted. Scroll sets *where* a block should be; a per-element spring chases it with lag and a small overshoot.

#### 16.2.1 Authoring contract

| Attribute | Parsed as | Effect |
|---|---|---|
| `data-fly` (empty) | `pull = 1` | full sideways pull toward the centre |
| `data-fly=".7"` | `parseFloat`; non-finite → 1 | sideways offset × pull |
| `data-fly="rise"` | `rise`, pull 0 | vertical only, never tilts |
| `data-fly-delay=".06"` | `clamp(parseFloat(v) \|\| 0, 0, 0.36)` | leaves later, lands on the same line; in rise mode, a timer of `d0 × 900ms` |

By role: `h2`, opening line, orbit lane items, contents stops, contact heading `data-fly`; lede `.7`; quote, `.snap`, email `.6`; letter body `.55` + `.08`; `.f-text` `.5` + `.06`; `.side` `.5` + `.12`; link row `.5`; `.f-top`, `.insert`, glance kicker and card, `.next` `rise`. Parts converge with decreasing sideways energy in a slight stagger; anything wide or interactive just rises. Feature numerals stay still as anchors.

- `data-fly` **MUST NOT** nest. The outermost element wins.
- A flying element **MUST NOT** carry a CSS `transform`, or a `transition` on `transform`/`opacity`. Static tilt and hover motion go on an inner element (`.snap-in`, `.inside .n`). Why: the inline transform written each frame replaces it, and `finish()` clears it.
- Put the attribute on a block that moves as one (heading, column, `aside`, tray), never on many small spans.

```html
<script>document.documentElement.classList.add("js")</script> <!-- in <head>, before <style> -->
```

```css
.js [data-fly] { opacity: 0; animation: flyShow 0.01s linear 2.5s forwards; }
@keyframes flyShow { to { opacity: 1; } }
.fly-ready [data-fly] { opacity: 1; animation: none; }
@media (prefers-reduced-motion: reduce) { .js [data-fly] { opacity: 1; animation: none; } }
@media print { [data-fly] { opacity: 1 !important; transform: none !important; animation: none !important; } }
```

`.fly-ready` **MUST** come after `.js` (same specificity, so source order decides) and **MUST** include `animation: none`. Why: a filled animation overrides inline styles and would pin opacity 1 over the script's fade. The failsafe is a step, not a design moment.

#### 16.2.2 The model

| | `travel` ≥ 1100px | `drift` ≥ 760px | `rise` < 760px |
|---|---|---|---|
| sideways `dx` | `(vw/2 − inkCx) × pull`; clamped ±48 if wide | clamped ±48 | 0 |
| dead zone | \|dx\| < 16 → 0 | \|dx\| < 16 → 0 | — |
| `dy` (start below home) | `clamp(0.06·vh, 28, 72)`, × 1.2 if wide | 40px | 22px |
| tilt | ±tiltMag if dx ≠ 0, not wide, ink width < 0.45·vw, box height < 0.6·vh | 0 | 0 |
| progress | scrubbed; lands at 0.58·vh | scrubbed; lands at 0.66·vh | timer, armed when top < 0.9·vh |

*Wide* means an ink width above 0.64·vw. **Ink centre:** for `H1|H2|H3|P|A`, a `Range` over the element's contents gives the union of its line boxes. Other tags use their box. Why: a right-aligned headline flies from where its *words* are; a short heading in a wide area stays narrow and may tilt. Left of centre (dx > 0) leans counter-clockwise, right of centre clockwise. A clamp keeps it 2px inside the viewport.

**Per-element constants** come from `jit(i, salt) = fract(sin(i·12.9898 + salt·78.233)·43758.5453)`, where `i` is the index in `querySelectorAll("[data-fly]")`.

| Salt | Quantity | Formula | Range |
|---|---|---|---|
| 1 | stiffness `k` (unit mass) | `170 × (0.86 + 0.28·jit)` | 146.2–193.8 |
| 2 | damping ratio ζ, `c = 2ζ√k` | `0.64 + 0.15·jit` | 0.64–0.79 (c ≈ 15.5–22.0) |
| 4 | tilt magnitude | `0.35 + 0.9·jit` | 0.35°–1.25° |
| 5 | start-line jitter `jS` | `(jit − 0.5) × 0.06` | ±0.03 vh |

That gives ≈ 2 Hz and 1.7–7.3% overshoot. A 0→1 step rests in 0.69–0.94s.

**Progress (scroll modes):**

```text
a   = vh × (0.96 + jS) // start line: top at 93–99% of vh
b   = vh × (travel ? 0.58 : 0.66) // landing line
b   = min(a − 40, max(b, top − maxS)) // reachable at the page end; journey ≥ 40px
p   = clamp(((a − (top − s)) / (a − b) − d0) / (1 − d0), 0, 1) // delay remap [d0,1] → [0,1]
pt  = max(pt, p) // ratchet
tgt = 1 − (1 − pt)³ // ease-out cubic
```

In rise mode, the target steps to 1 at `now ≥ armAt`. Why not scrub on phones: scrubbed motion fights the thumb.

**Spring.** Semi-implicit Euler, `n = ceil(dt·120)` substeps; `dt` ≤ 0.05s, 1/60 after sleep; rest at `|tgt − q| < 0.001` and `|v| < 0.004`; finished for good at `q ≥ 1`. **Paint** (`u = 1 − q`): sideways overshoot ≤ 5% of dx, then `14·tanh(x/14)`; vertical ≤ 10%; tilt × `u`; opacity `clamp(q/0.45, 0, 1)`. Rounded, string-cached writes: still elements write nothing.

**Admission and settle.** Admit (and set `will-change`) at top < `s + 1.2·vh`. A record already above the viewport, or above 0.58·vh in a scroll mode, finishes instead. Why: a fling never plays a backlog. Each scroll restarts a **180ms** timer; `idle()` then lands every live record with top above `vh − 24`, so stopped text finishes on its spring.

#### 16.2.3 Measurement

- **Write → read → write.** Clear the pending inline transforms, read everything in one layout, then finish or paint.
- **Document-space cache:** `top = rect.top + scrollY`, so a frame reads only `scrollY`. Re-sort by `top` after each measure, because visual order can differ from DOM order.
- **Re-measure**, debounced **140ms**, on `resize`, `load`, a body `ResizeObserver`, `fonts.ready`, and a 1100/760 `change`. In-flight `q`/`v` survive. Transforms do not change layout size, so the observer cannot loop.
- Use `clientWidth`, not `innerWidth`. Why: the centre must not sit under the scrollbar.

#### 16.2.4 Reference implementation

This is the first script at the end of `<body>`, before three.js.

```js
(function () {
  var root = document.documentElement, W = window;
  var REDUCE = W.matchMedia("(prefers-reduced-motion: reduce)");
  var TRAVEL = W.matchMedia("(min-width: 1100px)"), DRIFT = W.matchMedia("(min-width: 760px)");
  var recs = [], live = [], next = 0; // sorted records · simulated · admission pointer
  var raf = 0, last = 0, idleT = 0, measT = 0, scrolled = false, on = false;
  var vw = 0, vh = 0, maxS = 0, mode = "travel", pressed = null;

  function clamp(v, a, b) { return v < a ? a : v > b ? b : v; }
  function ease(p) { var u = 1 - p; return 1 - u * u * u; }
  function soft(v, m) { var e = Math.exp(-2 * Math.abs(v) / m); return (v < 0 ? -1 : 1) * m * (1 - e) / (1 + e); } // m·tanh(v/m)
  function jit(i, s) { var x = Math.sin(i * 12.9898 + s * 78.233) * 43758.5453; return x - Math.floor(x); }

  function collect() {
    var all = document.querySelectorAll("[data-fly]"), i, el, a, rise, pull, k, z;
    for (i = 0; i < all.length; i++) {
      el = all[i];
      if (el.parentElement && el.parentElement.closest("[data-fly]")) continue; // outer wins
      a = el.getAttribute("data-fly"); rise = a === "rise";
      pull = rise ? 0 : a ? parseFloat(a) : 1;
      k = 170 * (0.86 + 0.28 * jit(i, 1)); z = 0.64 + 0.15 * jit(i, 2);
      el._fly = { el: el, rise: rise, pull: isFinite(pull) ? pull : 1,
        d0: clamp(parseFloat(el.getAttribute("data-fly-delay")) || 0, 0, 0.36),
        k: k, c: 2 * z * Math.sqrt(k), tiltMag: 0.35 + 0.9 * jit(i, 4), jS: (jit(i, 5) - 0.5) * 0.06,
        top: 0, h: 0, l: 0, rr: 0, cx: 0, w: 0, gone: false, dx: 0, dy: 0, rot: 0, minX: -1e4, maxX: 1e4,
        q: 0, v: 0, tgt: 0, pt: 0, armAt: 0, live: false, done: false, held: false, ts: "", os: "" };
      recs.push(el._fly);
    }
  }
  function inkBox(el) {
    if (!/^(H1|H2|H3|P|A)$/.test(el.tagName)) return null;
    var rg = document.createRange(); rg.selectNodeContents(el);
    var b = rg.getBoundingClientRect(); return b.width > 0 ? b : null;
  }
  function targetTop(h) { // doc Y above which everything should be landed
    var id = "", t;
    try { id = h && h.charAt(0) === "#" ? decodeURIComponent(h.slice(1)) : ""; } catch (e) {}
    t = id && document.getElementById(id);
    return t ? t.getBoundingClientRect().top + W.scrollY + vh * 1.2 : -1;
  }
  function geom(r) {
    var wide = r.w > 0.64 * vw, dx = 0;
    if (mode !== "rise" && !r.rise && r.pull) {
      dx = (vw / 2 - r.cx) * r.pull;
      if (mode === "drift" || wide) dx = clamp(dx, -48, 48);
      if (Math.abs(dx) < 16) dx = 0;
    }
    r.dx = dx;
    r.dy = mode === "travel" ? clamp(vh * 0.06, 28, 72) * (wide ? 1.2 : 1) : mode === "drift" ? 40 : 22;
    r.rot = mode === "travel" && dx && !wide && r.w < 0.45 * vw && r.h < 0.6 * vh ? (dx > 0 ? -1 : 1) * r.tiltMag : 0;
    r.minX = Math.min(0, 2 - r.l); r.maxX = Math.max(0, vw - 2 - r.rr);
  }
  function measure(first) {
    measT = 0;
    mode = TRAVEL.matches ? "travel" : DRIFT.matches ? "drift" : "rise";
    var pend = recs.filter(function (r) { return !r.done; }), i, r, b, ink, s, hashTop;
    for (i = 0; i < pend.length; i++) { pend[i].el.style.transform = ""; pend[i].ts = ""; } // write
    vw = root.clientWidth; vh = W.innerHeight; s = W.scrollY; // read
    maxS = Math.max(0, root.scrollHeight - vh);
    for (i = 0; i < pend.length; i++) {
      r = pend[i]; b = r.el.getBoundingClientRect(); ink = inkBox(r.el) || b;
      r.top = b.top + s; r.h = b.height; r.l = b.left; r.rr = b.right;
      r.cx = ink.left + ink.width / 2; r.w = ink.width; r.gone = !b.width && !b.height;
    }
    hashTop = first ? targetTop(location.hash) : -1;
    for (i = 0; i < pend.length; i++) geom(pend[i]);
    recs.sort(function (p, q) { return p.top - q.top; }); next = 0;
    for (i = 0; i < pend.length; i++) { // write
      r = pend[i];
      if (r.gone || (first && (r.top - s < vh * 0.9 || r.top < hashTop))) finish(r); else paint(r);
    }
  }
  function paint(r) {
    var u = 1 - r.q, x = r.dx * (u < -0.05 ? -0.05 : u);
    if (u < 0) x = soft(x, 14);
    x = clamp(x, r.minX, r.maxX);
    var y = r.dy * (u < -0.1 ? -0.1 : u), a = r.rot * u;
    var ts = "translate(" + x.toFixed(1) + "px," + y.toFixed(1) + "px)" + (a ? " rotate(" + a.toFixed(2) + "deg)" : "");
    var os = clamp(r.q / 0.45, 0, 1).toFixed(3);
    if (ts !== r.ts) { r.el.style.transform = ts; r.ts = ts; }
    if (os !== r.os) { r.el.style.opacity = os; r.os = os; }
  }
  function finish(r) { // land instantly, forever
    if (r.done) return;
    r.done = true; r.live = false; r.q = r.tgt = r.pt = 1; r.v = 0;
    r.el.style.transform = r.el.style.opacity = r.el.style.willChange = ""; r.ts = r.os = "";
  }
  function admit(r, s) {
    if (r.done || r.live) return;
    if (r.top + r.h < s || (mode !== "rise" && r.top - s < vh * 0.58)) { finish(r); return; }
    r.live = true; r.el.style.willChange = "transform,opacity"; live.push(r);
  }
  function target(r, s, now) {
    if (mode === "rise") {
      if (!r.armAt && r.top - s < vh * 0.9) r.armAt = now + r.d0 * 900;
      r.tgt = r.armAt && now >= r.armAt ? 1 : 0; return;
    }
    var a = vh * (0.96 + r.jS), b = vh * (mode === "travel" ? 0.58 : 0.66);
    b = Math.min(a - 40, Math.max(b, r.top - maxS));
    var p = clamp(((a - (r.top - s)) / (a - b) - r.d0) / (1 - r.d0), 0, 1);
    if (p > r.pt) r.pt = p; // forward-only
    r.tgt = ease(r.pt);
  }
  function frame(now) {
    raf = 0;
    var dt = last ? Math.min((now - last) / 1000, 0.05) : 1 / 60; last = now;
    var s = W.scrollY, busy = false, i, j, r, n, h, acc;
    while (next < recs.length && recs[next].top < s + vh * 1.2) admit(recs[next++], s);
    for (i = live.length - 1; i >= 0; i--) { // backwards: safe splice
      r = live[i];
      if (r.done) { live.splice(i, 1); continue; }
      if (r.held) continue; // frozen under the pointer
      target(r, s, now);
      if (r.armAt > now) busy = true;
      if (r.tgt === r.q && r.v === 0) continue;
      n = Math.ceil(dt * 120); h = dt / n;
      for (j = 0; j < n; j++) { acc = r.k * (r.tgt - r.q) - r.c * r.v; r.v += acc * h; r.q += r.v * h; }
      if (Math.abs(r.tgt - r.q) < 0.001 && Math.abs(r.v) < 0.004) {
        r.q = r.tgt; r.v = 0;
        if (r.q >= 1) { finish(r); live.splice(i, 1); continue; }
      } else busy = true;
      paint(r);
    }
    if (busy || scrolled) { scrolled = false; raf = W.requestAnimationFrame(frame); } else last = 0; // sleep
  }
  function kick() { if (on && !raf) raf = W.requestAnimationFrame(frame); }
  function idle() {
    var s = W.scrollY, i, r;
    for (i = 0; i < live.length; i++) {
      r = live[i];
      if (!r.done && r.top - s < vh - 24) { r.pt = 1; if (!r.armAt) r.armAt = 1; }
    }
    kick();
  }
  function onScroll() { scrolled = true; kick(); clearTimeout(idleT); idleT = setTimeout(idle, 180); }
  function schedule() { if (on) { clearTimeout(measT); measT = setTimeout(function () { measure(false); kick(); }, 140); } }
  function settleTo(y) { for (var i = 0; i < recs.length; i++) if (!recs[i].done && recs[i].top < y) finish(recs[i]); }
  function settleAll() { settleTo(Infinity); }
  function start() {
    if (on || REDUCE.matches) return;
    if (!recs.length) collect();
    if (!recs.length) return;
    on = true; measure(true);
    W.addEventListener("scroll", onScroll, { passive: true });
    kick();
    idleT = setTimeout(idle, 180); // idle() once, after the first frame has admitted
  }
  function stop() { settleAll(); on = false; W.removeEventListener("scroll", onScroll); }
  function release() {
    if (!pressed) return; var r = pressed; pressed = null;
    setTimeout(function () { r.held = false; finish(r); }, 0); // let the click resolve first
  }

  W.addEventListener("resize", schedule);
  W.addEventListener("load", schedule);
  new ResizeObserver(schedule).observe(document.body);
  document.fonts.ready.then(schedule);
  TRAVEL.addEventListener("change", schedule);
  DRIFT.addEventListener("change", schedule);
  REDUCE.addEventListener("change", function () { if (REDUCE.matches) stop(); else start(); });
  document.addEventListener("click", function (e) {
    var a = on && e.target.closest('a[href^="#"]'); if (a) settleTo(targetTop(a.getAttribute("href")));
  }, true);
  W.addEventListener("hashchange", function () { if (on) settleTo(targetTop(location.hash)); });
  document.addEventListener("pointerdown", function (e) {
    var f = e.target.closest("[data-fly]");
    if (f && f._fly && !f._fly.done) { pressed = f._fly; pressed.held = true; }
  }, true);
  document.addEventListener("pointerup", release, true);
  document.addEventListener("pointercancel", release, true);
  document.addEventListener("focusin", function (e) {
    var f = e.target.closest("[data-fly]"); if (f && f._fly && f._fly !== pressed) finish(f._fly);
  }, true);
  W.addEventListener("beforeprint", settleAll);

  start();
  root.classList.add("fly-ready"); // always, in every path
})();
```

Test vectors for a port: `jit(0,1) = 0.182916` gives k 154.907; `jit(0,2) = 0.027105` gives ζ 0.6441 and c 16.032; i = 0 also gives tilt 0.540° and jS −0.02663. For i = 1: k 181.428, ζ 0.6511, tilt 1.093°.

#### 16.2.5 Always-readable escapes

| Situation | Behaviour |
|---|---|
| no JS / script fails | never hidden / `flyShow` reveals at 2.5s |
| reduced motion | CSS never hides; `start()` exits before collecting. Turned on mid-visit: `stop()` lands everything. Turned off: `start()`, and landed records stay landed |
| print | `!important` reset, plus `beforeprint` landing everything |
| first screen | at load, records with top < 0.9·vh finish: the first screen never animates in |
| hash / deep link | everything above `target + 1.2·vh` lands at load, on a capture-phase `a[href^="#"]` click (before the smooth scroll) and on `hashchange` |
| `focusin` | lands the focused block, so the focus ring is drawn at the true position |
| `pointerdown` | freezes the block under the pointer; release → `setTimeout(0)` → land. Why: a link cannot slide away mid-click |
| unscrolled page | `idle()` runs once, 180ms after start, so blocks between 0.9·vh and `vh − 24` land without a scroll |

**Pitfalls.** Measuring with transforms on returns displaced rects; a per-frame rect read reflows every frame; without the string cache, still elements recalculate style; tilted wide panels look broken.

**Adapting.** Nothing depends on text length. Match 1100/760 to the layout's breakpoints; lower the pull for long text; keep delays ≤ .36, tilt ≲ 1.5°, ζ ≥ 0.6 (lower wobbles). Right-aligned or short headings travel further, by design.

### 16.3 Other motion families

| Family | Essentials | Home |
|---|---|---|
| Theme reveal | View Transition; old/new root `animation: none`, z 1/2; new root `clip-path: circle(0 → farthest corner)` from the button centre, **700ms**. No API or reduced motion → instant | §7.2.6 |
| Signature object | scroll yaw ±12°, `yaw += ((t−0.5)·24 − yaw)·min(1, 4dt)`; fling `0.0006^dt`; make-room on `.sig-view` | §8.4, §15.6 |
| Sheet | dismiss at 90px, or 16px at 0.5px/ms; opens 520ms after a 3D pick, closes 480ms after a commit | §13.4 |
| Ticket springs | K 240, C 23 (ζ ≈ 0.74) + SWIRL 520 fading over the last 180px; height 380/36; stagger 0.045s; tilt `clamp(vx·0.0028, ±3.5)°`, scale ≤ 1.035 | §13.5 |
| Card flights, flow field | k 10, c 5.2 + swirl `min(1, d/1.6)·5.5` fading on arrival; arc flights with comets; pop-in `1−(1−s)³` | §14.11 |

### 16.4 Motion inventory

RM = reduced motion. "Same" = kept (short, user-initiated, no travel).

| Motion | Trigger | Properties | Duration | Easing | RM |
|---|---|---|---|---|---|
| Arrivals | scroll / timer | `transform`, `opacity` | spring ≈ 0.7–0.95s | scrubbed cubic + spring | static, visible |
| `flyShow` failsafe | script never ran | `opacity` | 0.01s after 2.5s | `linear` | not hidden |
| Anchor scroll | in-page link | scroll | UA | `smooth` | `auto` |
| Theme reveal | `.tbtn` click | `clip-path` (new root) | 700ms | `cubic-bezier(.4,0,.2,1)` | instant |
| Cover `cvIn` | load | `.cv-hero::before` `opacity`, `scale(1.025)→none` | 0.9s `both` | `cubic-bezier(0.2, 0.7, 0.2, 1)` | none |
| Bar nav / contents title underline | hover, focus-visible | `background-size` → 100% 0.3em / 0.32em; `color` 0.2s | 0.4s / 0.45s | `cubic-bezier(0.3, 0.9, 0.3, 1.15)` | same |
| Contents number | hover | `translateX(4px) rotate(-4deg)`; `color` | 0.45s; 0.25s | `cubic-bezier(0.34, 1.56, 0.64, 1)` | same |
| Object idle turn | scroll while pinned | WebGL `rotation.y` | τ 0.25s | first-order lag | off |
| Make room | drawer while pinned | `.sig-view` `transform` | 0.55s | `cubic-bezier(0.3, 1.15, 0.5, 1)` | instant |
| Photo-slot lift | hover `.snap` | `.snap-in` tilt × 0.4, `translateY(-3px)` | 0.5s | `cubic-bezier(0.34, 1.56, 0.64, 1)` | none |
| Race bars | demo tick | `width` | 0.05s | `linear` | none |
| Dropdown / drawer caret | toggle | `transform` 45° → −135° | 0.2s | `ease` | none |
| Buttons, segments, pills, zoom keys, ×, `.tbtn`, launcher | hover / press | `box-shadow`, `color` (`background` on "Open") | 0.2s | `ease` | same |
| Scene labels | state | `color`, `opacity` 1/.38/0 | 0.3s | `ease` | same |
| Tracker pulse | step `.now` | `box-shadow` ring 0→5px | 1.4s infinite | `ease-in-out` | none; paused while the sheet is open |
| Launcher hint | first full view, URL change | `tpCur` `transform`; `tpRing` `opacity`, scale 0.4→2.3 | 1.5s × 2 | `cubic-bezier(0.3, 0.7, 0.3, 1)`; `ease-out` | none |
| Sheet open | launcher | pane `transform`, `opacity` (1 at 30%); scrim | 440/380ms; 300ms | `cubic-bezier(.2,.85,.25,1)`; `ease` | 160ms fade |
| Sheet close | ×, Escape | pane `transform`, `opacity` held to 55%; scrim | 340/260ms; 300ms | `cubic-bezier(.4,0,.2,1)` / `cubic-bezier(.4,0,1,1)`; `ease` | instant |
| Sheet drag, spring-back | grab, bar | `translate3d`; scrim `max(0.2, 1 − dy/480)` | direct; 240ms | `cubic-bezier(.2,.85,.25,1)` | no spring-back |
| Ticket flights | open, close, add | `transform`, `height` | spring | spring + swirl | snap |
| New ticket / list reveal | add / open | `opacity`; `opacity` [0, 0 @ 25%, 1] | 320ms; 380ms | `ease-out`; `linear` | none |
| Ticket caret, scroll-in | toggle | `transform`; `scrollLeft` | 0.25s; UA | `ease`; `smooth` | none; `auto` |
| Scene motions (§14), guided demo (§12.5) | simulation; button or auto | WebGL; scripted | legs 0.5–1.1s; demo 4–6s | springs, cubic | snap, flights 0.01s; waits ≤ 120ms |
| Email underline, domain | hover, focus | `background-size` → 0.28em; `color` | 0.55s; 0.25s | `cubic-bezier(0.3, 0.9, 0.3, 1.15)` | same |
| Link row | hover | `border-color`, `color`; arrow `translate(2px,-2px)` | 0.2s; 0.3s | `ease`; back-out | same |

### 16.5 Easing palette

| Curve | Character | Used by |
|---|---|---|
| `cubic-bezier(0.3, 0.9, 0.3, 1.15)` | fast out, ≈ 4% overshoot: a pencil stroke past its end | all pencil underlines |
| `cubic-bezier(0.34, 1.56, 0.64, 1)` | back-out, ≈ 10% overshoot: a physical nudge | contents number, photo lift, link arrow |
| `cubic-bezier(0.3, 1.15, 0.5, 1)` | barely-there overshoot (< 1%) on a big move | make room |
| `cubic-bezier(0.2, 0.7, 0.2, 1)` | long deceleration | cover photo |
| `cubic-bezier(0.3, 0.7, 0.3, 1)` | quick approach, soft stop | launcher cursor |
| `cubic-bezier(.2,.85,.25,1)` | strong ease-out: a surface arriving | sheet open, spring-back |
| `cubic-bezier(.4,0,.2,1)` | standard in-out | theme reveal, close into the launcher |
| `cubic-bezier(.4,0,1,1)` | accelerate away | sheet drop |
| `ease`, `ease-out`, `ease-in-out`, `linear` | defaults | state, scrim, pulse, bars |
| `1 − (1 − p)³` / damped spring (JS) | ease-out cubic / carries velocity | arrivals, scene arcs, tickets |

Durations: 0.2s state; 0.25–0.3s colour, carets, arrows; 0.4–0.55s underlines and overshoots; 0.7s theme; 0.9s cover. New motion **SHOULD** use this scale and this table.

---

## 17 · Accessibility, performance, dark mode & print

Cross-cutting rules, stated once as requirements; component chapters hold the detail.

### 17.1 Accessibility

#### 17.1.1 Landmarks and heading outline

Banner `header.bar` and contentinfo `footer` sit outside `main#top`. The bar's `nav` is `aria-label="Sections"`, the orbit's `nav#inside` is `aria-label="What is inside"`. `section.cover`, `.opening` and `#orbit` take a short `aria-label`. `#glance`, `#letter`, `article#f1…#fN`, `#next` and `#contact` take `aria-labelledby` → their `h2`.

- There **MUST** be exactly one `h1`: `{{fullName}}` in the cover (`h1.mast`). Every section heading is an `h2`; figure titles are `h3`. Levels **MUST NOT** be skipped. Kickers, asides, feature numbers and the opening line are not headings. Why: heading navigation should list sections only.
- The contact heading **MUST** be an `h2.con-h`, styled as specified in §11. Why: a `<p>` label drops "Contact" from heading navigation.

#### 17.1.2 Duplicates and decoration

Anything drawn twice or drawn only for the eye **MUST** be hidden from assistive technology:

- `p.mast.mast-echo` (the name's ink copy, §7): `aria-hidden="true"`, and it is a `<p>`, never a heading.
- 3D label layers `.tags` (§14.2), progress bars `.track` (§12), feature numerals `.f-num`, hanging quote marks `.hang`, placeholder hatches `.snap-ph`, chevrons and carets, the launcher's "Open" pill (§13), and every inline decorative `<svg>` (`aria-hidden="true" focusable="false"`).
- Brush, pencil, loop, grain and tape stay CSS backgrounds or pseudo-elements, never `<img>`.
- Information shown only in a hidden layer **MUST** also exist as text elsewhere: 3D labels are mirrored in the side panel, and the phone stepper keeps its `.steps-now` line ("Step N of M · …").

#### 17.1.3 Focus

```css
:focus-visible {
  outline: 1.5px solid var(--accent);
  outline-offset: 3px;
}
:focus:not(:focus-visible) {
  outline: none;
}
```

| Target | Ring | Why |
|---|---|---|
| Default | 1.5px `--accent`, offset 3px | clears moulded edges; ≥ 4.64 : 1 |
| Range thumb | track none; thumb `0 0 0 2px var(--surf), 0 0 0 4px var(--accent)` (WebKit, Gecko) | the thumb is the control |
| Fields | `0 0 0 2px var(--accent)` added to the groove | ring follows the radius |
| `select`, `.csel-btn` after a mouse click | no ring; `--sink-s` | mouse focus reads as "pressed" |
| 3D stage canvas | `outline-offset: -2px` + matching radius | the well clips overflow |
| Signature canvas | `outline-offset: 2px` | §15 |
| Inside `.next` | accent swapped to the other theme's tone (§11) | the page accent is 2.92 : 1 on ink |

- A focused element still arriving **MUST** land at once (capture `focusin`, §16). Why: the ring belongs at the real position.
- The sheet focuses ×, traps Tab, guards `focusin`, returns focus to the launcher, and restores focus across re-renders by `data-*` keys (§13).

#### 17.1.4 Keyboard map (summary)

| Control | Keys | Built in |
|---|---|---|
| Signature canvas | ←/→ yaw ∓5°, ↑/↓ pitch ∓5°, Q/E roll; Shift = 15° | §15 |
| 3D stage canvas | arrows turn, `+`/`=` and `-`/`_` zoom, `0` resets | §14.5 |
| Custom dropdown | ↓/↑ open and move, Home/End, Enter/Space choose, Escape closes, Tab closes without choosing, type-ahead (600 ms) | §12 |
| Drawers (`details`) | Enter/Space toggle; Escape closes and refocuses the summary | §12, §8 |
| Sheet | Escape closes, Tab cycles inside, Android back (`CloseWatcher`) | §13 |

Canvases `preventDefault` only handled keys and never capture Tab.

#### 17.1.5 Live regions

- The only politeness level is `aria-live="polite"`. Use `role="alert"` only for a blocking form error, and `role="status"` for a submit confirmation.
- Announce **outcomes, not ticks**. A verdict is written once per run. A tab body or status sentence is written only when its text changes (`if (el.textContent !== s)`).
- A line that changes several times a second (the signature status, 4 Hz) **MUST NOT** be live. A recomputed table **SHOULD NOT** be live. Make a one-line total live instead. Why: the table would be re-read on every keystroke.
- Every live or late-filled region **MUST** reserve its height. Status lines `min-height: 2.6em`; `.steps-log` 48px; the records list 246px (132px ≤ 620px); the ticket list 230px; the verdict, error and success lines 32px, 20px and 30px.

#### 17.1.6 Names and labels

Every control **MUST** have an accessible name:

- Inputs: a wrapping `<label>` or `for`/`id`. Custom comboboxes: `aria-labelledby`. The hidden native `select` gets `aria-hidden="true" tabindex="-1"`.
- Icon buttons: `aria-label` (zoom keys, ×). Groups: `role="group"` plus `aria-label` (toggles, segmented controls, the zoom pill).
- Canvases: `role="img"` and an `aria-label` that says what the scene is *and* which keys drive it.
- Theme button: a static `aria-label="Switch between light and dark mode"` that contains both visible words.
- Disabled actions: `disabled` (plus `aria-disabled="true"`), still announced and visibly moulded (§11).
- Form errors **SHOULD** use `aria-describedby` and `aria-invalid`. On submit, focus moves to the first invalid field.

#### 17.1.7 Touch and pointer

- Hit areas **MUST** be at least 44px on coarse pointers. Buttons measure about 48px and bar links about 44px. Where the drawn size is smaller (zoom keys 40px, range inputs 40px tall, contact links about 34px), extend the hit area without changing the drawing (e.g. a positioned `::before` at `inset: -4px`).
- Canvases **MUST NEVER** trap a scrolling finger. Use `touch-action: pan-y` at 1×, and switch to `none` only while zoomed past 1.03 (§14.5). Wheel scrolls; zoom needs Ctrl/⌘, a pinch or the buttons.
- Controls, canvases and labels get `user-select: none; -webkit-touch-callout: none` (§6). Running text stays selectable. Use `-webkit-tap-highlight-color: transparent`; the pressed sink is the tap feedback.
- Hover-only motion **MUST** sit inside `@media (hover: hover)`.

#### 17.1.8 Contrast and colour

The core pairs are in §4.3. The additional pairs:

| Pair | Light | Dark | Rule |
|---|---|---|---|
| Inverse-card text: paper 84 / 78 / 72 / 68% on ink | 10.6 / 9.3 / 8.0 / 7.3 | 9.2 / 7.6 / 6.2 / 5.5 | no `--muted` on the card |
| Disabled text, paper 60% on the card | 5.9 | 4.3 | inert control only |
| Swapped accent on the card | 8.55 | 6.96 | ring and hover (§11) |

- Text under 18px **MUST** use `--accent-ink` or `--sand-ink`, never `--accent` or `--sand` (§4.4).
- Colour **MUST NEVER** be the only signal. "On" states are an ink fill (`--gface`) plus `aria-pressed`. Tracker states are a press depth (sunk, raised, filled) plus order and the text line. Late statuses use accent ink *and* their wording. A followed row gets an inset bar, not just a tint. The verdict is a sentence.
- `.ink` drops its text-shadow under `forced-colors: active` and inside `::selection`.

#### 17.1.9 Reduced motion

Under `prefers-reduced-motion: reduce` nothing is hidden and nothing moves by itself. Motion the user drives (turning, flinging, a run they press) and figure content (beads, simulations) stays.

| Element | Normal | Reduced |
|---|---|---|
| Anchor scrolling | `smooth` | `auto` |
| Theme switch | 700 ms circle reveal | instant swap |
| Cover photo `cvIn` | 0.9s fade and scale | `animation: none` |
| Arrivals `[data-fly]` | springs | visible from first paint; script never starts; `stop()` if switched on mid-visit |
| Signature object | scroll-linked turn; `.sig-view` 0.55s move | no scroll turn; `transition: none` |
| Drawer `.dock-c`, bars `.track i`, chevrons `.tk-more i`, `.csel-caret` | transitions | `transition: none` |
| Tracker pulse `.steps li.now::before` | 1.4s infinite | `animation: none` |
| Print hover lift `.snap-in` | 0.5s | `transition: none` |
| Launcher hint (cursor, ring) | 2 × 1.5s | `animation: none`; `hint()` returns early |
| Sheet | grows 440 ms / slides 380 ms; closes 340 / 260 ms | 160 ms fade; instant close; instant snap-back |
| Ticket cards | springs, swirl, stagger | snapped |
| Stage idle drift, auto-demo | after 6 s / 900 ms | off; auto-running figures start paused |

Scripts read `matchMedia("(prefers-reduced-motion: reduce)")`. Scripts that act continuously (arrivals, the signature object) **MUST** also listen for `change` on it.

#### 17.1.10 Without JS, without WebGL

- No JS: no `html.js` class, so nothing is hidden. Native `select`s show, controls sit inline in trays, browser-tab mocks stay in place, 3D wells are empty, and the theme follows the OS. All copy, links and the e-mail **MUST** be in the HTML. The footer year **SHOULD** be written in the HTML and only refreshed by the script.
- JS loads but the arrivals script fails: `flyShow` reveals everything at 2.5 s (§16).
- No WebGL: a muted sentence replaces the canvas; its controls are hidden or disabled, never dead.

### 17.2 Performance

#### 17.2.1 Loading

- Preload the portrait (`as="image" type="image/webp" fetchpriority="high"`) and the seven first-screen faces (`as="font" type="font/woff2" crossorigin`; all but Nunito 600 and Caveat 500 (§2.5). Why: the LCP portrait lives in `--me`, invisible to the preload scanner; without `crossorigin` a font downloads twice.
- Every `@font-face` uses `font-display: swap` and `font-synthesis: none`.
- `html.js` is set by the head script, before `<style>`. The saved theme is applied there too, inside try/catch, so there is no flash.
- End of `body`: arrivals first, then three.js, then the rest. Why: `fly-ready` lands before 608 KB is parsed.
- Any other heavy library (the PDF library) **MUST** load on intent. Start the load on the button's first `pointerenter` or `focus`, memoise it as a Promise, forget it and remove the `<script>` on error, and fall back to `window.print()` (§2.7).

#### 17.2.2 Frame and DOM rules

1. **Loops sleep off-screen.** An `IntersectionObserver` per canvas stops scheduling rAF when hidden; on re-entry reset `last` and clamp `dt ≤ 0.05 s`. Arrivals sleep when nothing moves; an open sheet holds stages (simulate, don't draw).
2. **Measure rarely, write only on change.** Positions are cached in document space (`rect.top + scrollY`), so a frame reads only `scrollY`. Re-measure on resize, `load`, body `ResizeObserver`, `fonts.ready` and breakpoint `change`, debounced 140 ms, as write → read all → write. `renderer.setSize(w, h, false)` lets CSS own the canvas size.
3. **`will-change` only while live.** Arrivals set `will-change: transform,opacity` on admission and clear it on landing. Only elements that move every frame (3D labels, the open sheet) keep it.
4. **DPR cap 2:** `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))`.
5. **Cache geometry.** `EdgesGeometry` is stored on `geometry.userData`. Static geometry is batched per material and face (19 objects for the signature object); materials are shared; hit proxies are simple.
6. **One draw call for all glow.** One `Points` cloud with preallocated typed arrays, `setDrawRange`, `frustumCulled = false`, and sprites with alpha ≤ 0.003 skipped.
7. **String-compare DOM writes.** Compare `innerHTML`, `textContent` and `aria-label` before writing. Arrivals cache transform and opacity strings, rounded with `toFixed`. Why: no recalc, no live-region chatter, no observer loops.
8. **Panels refresh at 4 Hz.** Status and readouts update at most every 250 ms (`if (T - lastOut < 0.25) return;`). Theme checks run every 20 frames as a string compare (§17.3).
9. Passive scroll listeners; no layout reads inside rAF.

#### 17.2.3 Budgets

| Item | Budget |
|---|---|
| `index.html` | ≈ 345 KB raw / ≈ 78 KB gzip |
| Portrait WebP | ≤ 210 KB |
| Fonts | 9 faces ≈ 137 KB; 7 preloaded ≈ 102 KB |
| three.js r149 | 608 KB / ≈ 152 KB gzip |
| PDF library | ≈ 364 KB, never on first load |
| First load | ≈ 1.26 MB raw / 0.53 MB gzip; no third-party request |
| LCP (portrait), mid phone, 4G | ≤ 2.5 s |
| CLS | ≤ 0.02 |
| INP | ≤ 200 ms |
| Idle CPU with no canvas visible | ≈ 0 (every loop asleep) |

A build **SHOULD** stay within about 10% of each size.

### 17.3 Dark mode rules

1. **Two declarations, always.** Every dark rule, for tokens or components (theme knob, `.next` mixes, `.snap-in`), is written under `:root[data-theme="dark"]` *and* under `@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) … }` (§4.2).
2. **Theme-color follows the theme.** `#tcLight` / `#tcDark` hold the two papers; after a manual choice both take the current paper and lose `media`. The label also updates on an OS `change` when no choice is stored (§7).
3. **WebGL recolours itself by probing.** Hidden span `color: var(--token)` → 1×1 canvas → sRGB; a token string key is compared every 20 visible frames and probing runs only on change (§14.7). The theme switch **MAY** recolour figures synchronously so the view-transition snapshot is already right.
4. **Photos are never dimmed.** No `filter`, no theme-dependent shade.
5. **Prints turn charcoal:** `.snap-in` `#2e2c27`, hatch `#25231f` with `rgba(255, 250, 240, 0.07)` lines, caption `#b9b3a7`, shadow `0 14px 28px -18px rgba(0, 0, 0, 0.8)`.
6. **The inverse card turns cream.** `.next` stays `var(--ink)` (now `#eeebe3`) with its own mixes (`#fff 70%`, `#000 15%`) and the swapped accent (§11).
7. **Each mark is re-declared.** `--brush`, `--pencil` and `--loop` have their colour baked into the SVG, so each theme block carries its own encoded copy (sand `.36`/`.2`, leaf `#a9b77c`). So does `--grain`. Why: `var()` cannot reach inside `url()`.

### 17.4 Print

Print is one column of ink on white: no controls, decoration, WebGL or photos, and every arrival visible. The block comes last in the stylesheet:

```css
@media print {
  .bar,
  .runrow,
  .seg,
  .tbtn,
  .toggles,
  .zoom,
  .tab-pop {
    display: none;
  }
  [data-fly] {
    opacity: 1 !important;
    transform: none !important;
    animation: none !important;
  }
  body {
    background: none;
  }
  .orbit {
    display: block;
  }
  .orbit-flow {
    display: block;
    padding: 0;
  }
  .orbit-flow > *,
  .orbit-flow .inside > * {
    width: auto;
    margin-top: 12px;
  }
  .orbit .sig-ui,
  .o-hand,
  .snap,
  .mast-echo,
  .orbit-pin {
    display: none;
  }
  .next {
    background: none;
    color: var(--ink);
  }
  .next p,
  .next .lab,
  .next .built,
  .next .built .note {
    color: var(--ink);
  }
  .cv-hero .mast {
    margin-bottom: 0;
  }
  .cv-hero {
    min-height: 0;
    background: none;
  }
  .cv-hero::before {
    display: none;
  }
  .cv-hero .mast,
  .cv-hero .lab,
  .cv-hero .note {
    color: #111;
  }
  .gl-acts {
    display: none;
  }
  .gl-card {
    background: none;
  }
  :root:is([data-theme], :not([data-theme])) {
    color-scheme: light;
    --paper: #e9e6de;
    --paper2: #e0ddd3;
    --ink: #151412;
    --muted: #54514a;
    --hair: rgba(21, 20, 18, 0.14);
    --rule: rgba(21, 20, 18, 0.32);
    --accent: var(--brand);
    --accent-ink: #435425;
    --sand-ink: #7a5b3b;
  }
}
```

- `.zoom` and `.tab-pop` in the first rule are: controls mean nothing on paper. So is the closing token block: dark-theme ink (`#eeebe3`) would print near-invisible on white. `:is()` gives it specificity 0,2,0, and because it comes later it beats both dark blocks.
- `beforeprint → settleAll()` lands every arrival in JS as well (§16). Why: the `!important` reset covers paper, the settle covers the screen after printing.
- The orbit flattens to one column with 12px between lane items. The pinned object, its controls, the pencil aside, prints and the name's echo are dropped: they are WebGL, decorative or duplicate, and the echo would print the name twice. The cover keeps the name in `#111` and drops the photo. `.next` loses its slab and turns its translucent-paper text to ink.
- The PDF fallback `window.print()` prints the whole page under this sheet. A build **MAY** isolate one card with a flag class set before printing and removed on `afterprint`.

---

## 18 · Responsive matrix, quality bar & acceptance tests

The checklist a finished build is judged against. It adds no values; where it disagrees with a component chapter, the component chapter wins.

### 18.1 Responsive matrix

Bands follow §6.5. "Short landscape" is `(max-height: 520px) and (orientation: landscape)` and stacks on the width band that applies. The orbit also goes one column under 680px tall (§8.5).

| Component | ≥ 1600 | 1100–1599 | 761–1099 | ≤ 760 | ≤ 460 / 360 / 300 | Short landscape |
|---|---|---|---|---|---|---|
| Frame and type base | body 18px (19px ≥ 2200); frame caps at 1560px (≈ 1711px viewport), 1680px from 1920; bleeds widen past `--gut` | body 17px; 12-column frame `100% − 2·--gut` | body 17px; `--cg` ≈ 13–19px | `--gut` → 16px floor at 364px; `--cg` 12px | body 15.5px at ≤ 360, 14.5px/1.6 at ≤ 300; `.lab, label` tracking 0.1em at ≤ 300 | unchanged; `scroll-padding-top: 0` |
| Bar `header.bar` | name, five links, theme pill; 54px; hairline | same | same, link gaps ≈ 18px | name, one link (`#glance`), pill; never a hamburger | ≤ 359: name and pill only; ≤ 300: pill padding `5px 8px` | `position: static` (scrolls away) |
| Cover `section.cover` | card 26px radius, ≈ 3 : 1, name at its cap (196px at 1920), one line | landscape card ≈ 2.17 : 1 → 2.35 : 1; name ≈ 167px at 1440 | landscape, name ≈ 95px at 820 | ≤ 620: portrait card ≈ 4 : 5, radius 22, photo re-framed to `{{portrait.focal}}`; name ≈ 44px at 390 | name one line at every width (32.2px at 300); corner labels stay in the top corners | card `min-height: 78svh` so the name is in view |
| Opening `section.opening` | two lines, under 40% of width, right side empty paper | 104px at 1440, starts 65px past the frame | 59px, ≈ 45% of width | ≤ 620: `clamp(46px, 14.6vw, 66px)`, fills the width like a poster | 46px floor at 300; still two to three lines | unchanged |
| Orbit `section#orbit` | three lanes: left lane, pinned object column (`--objW` up to 620px), right lane; stops zig-zag | same, needs height ≥ 680; hand aside and arrow shown | one column: stand paragraph → object (≤ 560px) → control bar → caption → contents as a 4-column grid under an ink rule; aside hidden | same order; contents 2 columns (≤ 620); object ≈ 190px wide at 390 | control grid gaps `10px 18px` (≤ 460), 1 column (≤ 360); contents 1 column (≤ 300) | one column; `.sig-view` `max-width: min(520px, 78svh)` |
| At a glance `section#glance` | label column cols 1–2 (aside tilted −2.5°), card `col 3 / bleed-end`, radius `28px 0 0 28px`; band 3.4 : 1; actions right of pitch; facts 3 × 2; stack groups side by side | same from 1280; 1100–1279 card `col 1 / bleed-end` | kicker and aside on one line above; band 2.5 : 1 (≤ 999); actions under pitch; stack groups as rows (150px label) | band 1.75 : 1 (≤ 640); radius 24; actions a 3-cell grid; facts 2 columns | ≤ 380: aside hidden, band 1.6 : 1, actions 2 columns; ≤ 300: facts 1 column | band back to 3.4 : 1 |
| Letter `section#letter` | headline cols 1–7; print cols 2–4, ≈ 140px lower; body two newspaper columns cols 6–12 | same | headline ≈ 44px; print cols 1–4; body one column from `col 5` | headline ≈ 36px, 3 lines; body one column; print after the text, right-aligned, ≈ 62% wide, opposite tilt | drop cap 4.2em at ≤ 300 | unchanged |
| Feature articles `article#fN` | seven compositions L1–L7, `br.d` breaks on, type at caps | same compositions (§10) | one tablet pattern in DOM order: full-width top line, body 2 columns, details half-width under an ink rule; L2 still right-aligned, L3 still stepped, L4 still bleeds left | one column in DOM order; headlines at clamp minimums (36–48px), balanced; details full width; quotes from `col 2` | details label 104 → 84px (≤ 460), rows stacked (≤ 360) | unchanged |
| Figure tray `.insert` | footprint per layout; radius 28; L4 bleeds left | same | full frame; L4 still bleeds | full frame, L4 radius `0 22px 22px 0`; drawers closed with a one-line summary (≤ 700) | run-row buttons stretch, fields 1 column (≤ 460); control grids 1 column, smaller `.btn`/`.seg` padding (≤ 360) | unchanged |
| 3D stage `.stage` | well 7 : 6 (`.sq` 1 : 1), side panel beside it | same | ≤ 900: stage and side panel stack; 4 : 3 (701–900) | ≤ 700: 1 : 1 (`.sq` 4 : 5), scene re-framed tall, labels 8.5 / 13px | host < 300px: `.tiny` keys, no caption | `max-height: 82svh` |
| Tab mock, ticket row | inline tab; ticket grid | same | ≤ 900: launcher row → bottom sheet; tickets a sideways spring row | same | tab footer wraps (≤ 460); tab glyphs and launcher pill hidden (≤ 300) | sheet `max-height` from `100svh` |
| Readouts, tracker, two-panel figures | inline readouts; vertical tracker | same | two-panel figures stack (≤ 800) | ≤ 620: readouts as raised cards, tracker a 6-dot stepper with its text line | readouts 2 columns (≤ 460); statement total one line (≤ 240), then wraps (≤ 220) | unchanged |
| Photo slots `.snap` | tilted, taped, in an empty quadrant | same; hover lift on `(hover: hover)` | beside or under the text block it belongs to | after its text, inset 8px so the tilted corner clears the edge | unchanged | unchanged |
| Next issue `section#next` | ink card `col 2 / bleed-end`, radius `32px 0 0 32px`; print across the top edge; headline 146px at 1920 | headline ≈ 109px at 1440 | ≤ 900: one column, print bottom-right, clipped by the card edge | card `col 1 / bleed-end`, radius 24; print 112px; headline ≈ 49px | unchanged | unchanged |
| Contact `section#contact` | address 116px at 1920, may enter the bleed | address from `col 2`, ≈ 89px | all parts `col 1 / col-end`; address ≈ 51px | address 30px, one line at ≥ 360 | ≤ 300: address breaks before `@` (`<wbr>`), never scrolls | unchanged |
| Footer | rule, copyright left, colophon right on one row | one row above ≈ 1046px viewport | colophon wraps 20px below, right-aligned | ≤ 620: colophon left-aligned | unchanged | unchanged |
| Arrivals (§16) | `travel` from the viewport centre | `travel` | `drift`, sideways travel ≤ ±48px | `rise` only (22px) | same | same as its width band |

### 18.2 Quality bar: the tells

A build passes the eye test when a reviewer finds every one of these.

1. Paper `#e9e6de` and ink `#151412` (dark `#1c1b18` / `#eeebe3`); no `#fff` or `#000` in any rendered colour.
2. Grain is on the page and the bar only; trays, cards and wells are smooth.
3. The grain is two noise tiles (200px and 173px) that darken and lighten in balance, so the paper's average colour does not drift.
4. The cover name is cut by the photo edge ≈ 0.09em above the baseline, and its ink echo carries the feet on the paper.
5. That name holds one line from 300px to 2560px through `min()` against the card's content width.
6. The only bold is a sideways text-shadow in the text colour (`max(1.1px, 0.022em) 0 0`); no bold face is loaded.
7. Exactly one italic hinge word per headline; italic words get looser tracking than roman ones.
8. Left-aligned headlines carry a negative left margin so the first stem sits on the rule's start.
9. Indexes are DM Mono with slashed zeros.
10. Pauses come in long/short pairs (202/137, 216/101, 173/86, 187 at 1440); the longest sit before the work and before the end.
11. No two consecutive features share a layout; the top-line rule itself varies (full, from `col 5`, under the headline, hairline, shortened with flipped labels).
12. Bleeds alternate right, left, right; the bleeding side squares its corners and pads by `--gut`.
13. Each pull quote hangs a zero-width sand opening mark in the margin; the closing mark is ink.
14. Quotes invert emphasis: italic quote, one upright word.
15. One quote overlaps the bottom edge of the tray above it.
16. "On" is an ink fill; green is never a fill and nothing glows.
17. Controls are moulded from the surface beneath them; on the ink card their light is re-mixed.
18. Shadows are warm brown, about 10px, lit from the top-left, the same direction everywhere.
19. Push buttons are 16px soft rectangles; fields, stack pills and segments are pills.
20. The slider knob is a raised disc with a 7px accent dot over a sunken groove.
21. "Now" pills carry a 4px sand crescent on their left curve only.
22. The letter has a real 3-line drop cap and columns that never split a paragraph.
23. Photo slots are square-cornered, off-white, hatched, captioned in mono, taped at a counter-angle, and straighten on hover.
24. Hand marks keep a constant stroke width at any word length (`vector-effect: non-scaling-stroke`).
25. 3D line work is one device pixel, three ink opacities, no lighting, a 30° lens: a drawing, not a render.
26. The 3D well is the page colour: lighter than its tray in light mode, darker in dark mode.
27. The accent is sampled from the portrait and split into a mark tone and a darker small-text tone (§4.4).
28. Kickers are sentence case; tracked capitals appear only on the photo and as form labels.
29. Body is Jost 300 at 17/1.68, never 400.
30. The address splits into ink local part, italic `@` and muted domain; the domain turns accent only on hover.
31. The inverse card stays inverse in dark mode (cream on charcoal).
32. The theme pill names the destination theme; its knob is the accent dot.
33. The bar is 99% opaque grained paper with a hairline and no shadow.
34. Ledes are balanced: no lede ends on a one-word line.
35. Trays reserve the height of output that appears later; nothing below jumps when a figure runs.
36. Every display element has an 11–13px companion on the same rule or in its corner, never centred under it.
37. About ten hand marks on the whole page; never more than two in one 1440 × 900 view.

### 18.3 Quality bar: anti-patterns

Each **MUST NOT** appear.

| # | Anti-pattern | Why it breaks Folio |
|---|---|---|
| 1 | Pure white or black backgrounds, black text, a cold blue-black dark mode | the whole page is warm uncoated stock (§4) |
| 2 | Gradient fills: hero gradients, mesh blobs, gradient text, brand gradients | the only gradients are the photo's legibility washes and the band's fade |
| 3 | Glassmorphism: translucency, `backdrop-filter` blur | nothing is see-through; even the bar is 99% opaque |
| 4 | More than one accent, per-technology colours, coloured section bands | one sampled accent and one sand, both used as marks (< 0.1% of the page) |
| 5 | Accent-filled buttons, accent links at rest, accent headings | "on" and primary are ink |
| 6 | Bold or heavy geometric sans headings | headlines are a 400-weight serif; emphasis is posture or the stamp |
| 7 | Emoji, icon fonts, logo walls, skill icons, skill bars, percentages | no icon font is loaded; stacks are plain mono pills; nothing is rated |
| 8 | Card grids with drop shadows or borders; a project-card grid with thumbnails and tags | trays are flat plates; each feature is a composed spread |
| 9 | Full neumorphism: embossed cards, embossed text | only pressable things are moulded |
| 10 | Hover scale on everything, coloured hover glows | hover changes colour or posture on a few named elements, never size or glow |
| 11 | Parallax backgrounds, scroll-jacking, snap scrolling | the page scrolls natively; only the orbit object and arrivals answer scroll (§16) |
| 12 | Typewriter or text-scramble effects, rotating taglines | display type is still; the name is the LCP and never animates |
| 13 | Custom cursors, cursor followers, magnetic buttons | the pointer is the system pointer |
| 14 | One fade-up for every element with the same timing | arrivals are composed per part; much text does not move |
| 15 | Lorem ipsum or invented biography | every string comes from the content model (§3); empty slots say so honestly |
| 16 | Stock illustrations, 3D blobs, AI-generated scenery, avatar circles | imagery is one real photograph, hairline drawings and taped prints |
| 17 | Centred everything: centred hero, centred headings, centred sections | composition is left-weighted and asymmetric; half a row is empty paper |
| 18 | One section template with equal spacing (e.g. 96px everywhere) | rhythm is paired pauses and seven layouts (§6.4) |
| 19 | The hero name wrapping to two lines or shrinking to a heading on phones | the name fills the card at one line on every width |
| 20 | A hamburger or off-canvas menu | the bar keeps one link and the pill |
| 21 | Uppercase tracked eyebrow labels above every heading | kickers are sentence case |
| 22 | Doodles everywhere | marks are rare; rarity is what makes them read as the editor's hand |
| 23 | Glossy, lit 3D, bloom, spinning logos | 3D is engraving: lines, alpha-tinted dots, no lights |
| 24 | Dark mode by inversion (inverted photos, same accent, white text) | every token is re-inked; photos are never filtered |
| 25 | Grey shadows | shade on paper is warm brown; grey reads as plastic |
| 26 | Justified body text, lines over ≈ 70 characters | body is ragged, in columns, ≤ 62ch |
| 27 | Frameworks, CSS frameworks, CDN fonts or scripts at runtime | one hand-written file, self-hosted assets (§2) |

### 18.4 Acceptance tests

Run each test in both themes at 300, 360, 390, 460, 760, 761, 820, 1099, 1100, 1440, 1600, 1920 and 2560px wide (900px tall), plus 844 × 390 landscape.

Console helpers:

```js
// T-overflow: offending elements wider than the viewport
[...document.querySelectorAll("body *")].filter(e => e.getBoundingClientRect().right > innerWidth + 0.5 && getComputedStyle(e).position !== "fixed");
document.documentElement.scrollWidth <= innerWidth;           // must be true

// T-shadow: every element with a box-shadow, review against test 18
[...document.querySelectorAll("*")].filter(e => getComputedStyle(e).boxShadow !== "none").map(e => e.className || e.tagName);

// T-colour: no pure white or black in any computed colour
[...document.querySelectorAll("*")].flatMap(e => { const s = getComputedStyle(e); return [s.color, s.backgroundColor]; })
  .filter(c => c === "rgb(255, 255, 255)" || c === "rgb(0, 0, 0)");   // must be []
```

#### 18.4.1 Layout and responsive

| # | Test | How to check |
|---|---|---|
| 1 | No horizontal scroll from 300 to 2560px | T-overflow at every listed width and a drag from 300 to 2560 in device mode |
| 2 | The cover name is one line at every width, and its ink echo lines up with it within 1px | its `h1` height ≤ 1.1 × its font-size; overlay the echo and the photo-clipped copy |
| 3 | `col 3 / col-end` spans x 286 → 1377 at 1440 and 78 → 373 at 390, ±1px | measure the glance card's left edge and a full-frame tray's right edge |
| 4 | The bar shows 5 links at ≥ 761, 1 at ≤ 760, 0 at ≤ 359; it never wraps to two rows | bar height = 54px + 1px at every width |
| 5 | Bleeding plates square their corners on the bleeding side and their content stays on the frame | glance card, L4 tray, next card: computed `border-radius`; text left edge = `col N` |
| 6 | The orbit pins only at ≥ 1100 × 680; at 1440 × 600 it is one column | resize height across 679/680 |
| 7 | Drawers are open at ≥ 701 and closed with a value summary at ≤ 700, unless the user toggled one | resize across 700/701 before and after a manual toggle |
| 8 | Below 1100 the visual order equals the DOM order in every section | tab through a feature; focus never jumps backwards on screen |
| 9 | In short landscape the bar is static and the cover name is visible without scrolling | 844 × 390: name bottom ≤ 390px |
| 10 | Nothing jumps when a figure runs or a status line fills | record a run; heights above and below the tray are constant |

#### 18.4.2 Typography

| # | Test | How to check |
|---|---|---|
| 11 | Only the nine declared faces load; no synthesised bold or italic | Network tab (fonts); computed `font-synthesis: none` |
| 12 | No headline uses `font-weight` above 400; emphasis is the stamp or italic | query `h1, h2, h3` computed weights |
| 13 | Each headline has at most one italic span and one stamped span, never the same word | inspect every `h2` |
| 14 | Body text is Jost 300, 17px at 390–1599, ragged-right, ≤ 62ch | computed style; no `text-align: justify` anywhere |
| 15 | Kickers are sentence case; `text-transform: uppercase` appears only on `.lab`, form labels and photo labels | search computed styles |

#### 18.4.3 Colour and surface

| # | Test | How to check |
|---|---|---|
| 16 | No pure white or black is rendered | T-colour returns `[]`; screenshots sampled show no `#ffffff` / `#000000` outside photos |
| 17 | Accent pixels are < 1% of any 1440 × 900 viewport outside photos and 3D canvases | screenshot, mask photos and canvases, count pixels within ΔE 6 of `--accent` / `--accent-ink` |
| 18 | Only these have a `box-shadow`: moulded controls and plates in the `--raise`/`--sink` family (wells, drawers, statement card), taped prints, the open bottom sheet, focus rings on thumbs and fields, the tracker pulse and the followed-row inset bar. The bar, trays, cards and photos have none | T-shadow list reviewed against this sentence |
| 19 | No `backdrop-filter`, no gradient `background` except photo washes, the glance band fade and drawer glyph strokes | search computed `background-image` |
| 20 | Grain appears on page and bar only | sample a tray: zero variance; sample the page: per-pixel deviation ≈ 20 around paper |
| 21 | Both themes pass contrast: body ≥ 7 : 1, secondary ≥ 4.5 : 1, small accent and sand text use the `-ink` tones | axe or a contrast checker on each text style; compare with §4.3 |
| 22 | Dark mode is re-inked: photos unfiltered, the next card cream, prints charcoal | toggle; computed `filter: none` on photos |
| 23 | A saved theme applies before first paint | reload with a stored dark choice and throttled CPU; no light frame in the performance filmstrip |
| 24 | The theme follows the OS when nothing is stored, and the pill label follows too | emulate `prefers-color-scheme` both ways |

#### 18.4.4 Motion

| # | Test | How to check |
|---|---|---|
| 25 | With `prefers-reduced-motion: reduce` nothing translates, scales or rotates by itself; every arrival is visible on first paint | emulate in DevTools; scroll the page; Animations panel shows no running animation |
| 26 | Under reduced motion the theme swaps instantly and anchor jumps are instant | click the pill and a contents stop |
| 27 | Arrivals travel at ≥ 1100, drift ≤ ±48px at 760–1099, rise only below 760 | scroll at each width and watch the sideways offset |
| 28 | No animation loop runs while no canvas is visible | Performance panel, scrolled to the letter: no rAF callbacks after landing |

#### 18.4.5 Accessibility

| # | Test | How to check |
|---|---|---|
| 29 | Tab reaches every control in visual order; the focus ring is visible on each | keyboard-only pass in both themes, including inside the next card |
| 30 | Exactly one `h1`; no skipped heading levels | heading outline in an accessibility tree viewer |
| 31 | Every input, select, custom combobox and icon button has an accessible name | axe: zero "label" and "button-name" violations |
| 32 | The cover echo, decorative glyphs, quote marks and hatches are hidden from assistive technology | accessibility tree shows the name once |
| 33 | Canvases never trap a scrolling finger at 1× zoom; arrow keys turn them when focused | touch emulation swipe; keyboard test per §17.1.4 |
| 34 | The sheet traps focus, closes on Escape and returns focus to its launcher | at 390 open and close by keyboard |
| 35 | Live regions announce outcomes only; the 4 Hz status line is not live | screen reader pass while a figure runs |
| 36 | Lighthouse accessibility ≥ 95 at 390 and 1440 | Lighthouse, both themes |
| 37 | Coarse-pointer hit areas ≥ 44px | measure buttons, bar links, zoom keys with their extended hit areas |

#### 18.4.6 Robustness and performance

| # | Test | How to check |
|---|---|---|
| 38 | With JS disabled every section and all copy, links and the address are visible | disable JavaScript and scroll the whole page |
| 39 | With WebGL unavailable each 3D figure shows a sentence and its controls are hidden or disabled | `--disable-webgl` or a blocked context |
| 40 | No console errors or warnings on load, on scroll, on theme switch and while running every figure | console at 390 and 1440 |
| 41 | CLS ≤ 0.02; LCP is the portrait, ≤ 2.5s on a mid phone over 4G | Lighthouse mobile, three runs, median |
| 42 | No request leaves the origin at runtime; the PDF library loads only on intent | Network tab filtered to third-party: empty; PDF script absent until the button is hovered or focused |
| 43 | Sizes stay within ≈ 10% of the §17.2.3 budgets | Network tab totals, raw and gzip |
| 44 | Print output is one column of ink on white with all arrivals visible and no controls | print preview |
| 45 | The page contains no placeholder syntax or filler: no `{{`, no lorem ipsum, no example names | search the served HTML |

---

## Appendix A · Token sheet

Every Folio custom property, ready to paste in this order into part B.02 (§2). A.7 goes with the next-issue card (§11). Roles and reasons are in §4 and §5.

`/* A */` marks a **default accent** value. Each one **MUST** be replaced with the derived accent (§4.4.2). In `--pencil` / `--loop`, write the stroke as `%23` + hex without the `#`. Paper, ink, sand, grain and the mixes **MUST NOT** change.

### A.1 Light (`html` + `:root`)

The default theme: colours, type families (`--serif` `--sans` `--mono` `--hand`, plus `--ui` for controls), `--gut` `--cg` `--max` `--r`, soft-light inputs, two-tile grain, and the marks `--brush` (highlighter), `--pencil` (underline) and `--loop` (circle).

```css
html {
  color-scheme: light;
}
:root {
  --brand: #546634; /* A */
  --paper: #e9e6de;
  --paper2: #e0ddd3;
  --ink: #151412;
  --muted: #54514a;
  --hair: rgba(21, 20, 18, 0.14);
  --rule: rgba(21, 20, 18, 0.32);
  --accent: var(--brand);
  --on-accent: #f6f4ec;
  --accent-ink: #435425; /* A */
  --serif: "Instrument Serif", "Times New Roman", Georgia, serif;
  --sans: "Jost", "Helvetica Neue", Arial, sans-serif;
  --mono: "DM Mono", ui-monospace, "SFMono-Regular", Menlo, Consolas, monospace;
  --hand: "Caveat", "Segoe Print", "Bradley Hand", cursive;
  --gut: clamp(16px, 4.4vw, 84px);
  --grain:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3' seed='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0.24 0 0 0 0 0.2 0 0 0 0 0.14 0.3 0 0 0 -0.105'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E"),
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='173' height='173'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' seed='19' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 1 0 0 0 0 0.99 0 0 0 0 0.96 0 0.75 0 0 -0.22'/%3E%3C/filter%3E%3Crect width='173' height='173' filter='url(%23n)'/%3E%3C/svg%3E");
  --cg: clamp(12px, 1.7vw, 32px);
  --max: 1560px;
  --ui: "Nunito", "Avenir Next", "Segoe UI", system-ui, sans-serif;
  --surf: var(--paper);
  --r: 16px;
  --hiMix: #fff 55%;
  --loMix: #5a4630 16%;
  --sand: #9a7650;
  --sand-ink: #7a5b3b;
  --brush: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 240 24' preserveAspectRatio='none'%3E%3Cpath fill='%239a7650' fill-opacity='.45' d='M4 13C38 8 92 9 140 8s80-2 96 2c3 2 1 5-5 5-40 1-110 2-160 4S14 21 6 20c-4-1-5-5-2-7z'/%3E%3Cpath fill='%239a7650' fill-opacity='.24' d='M18 18c50-2 120-4 200-3-60 3-130 5-200 5z'/%3E%3C/svg%3E");
  --pencil: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 10' preserveAspectRatio='none'%3E%3Cpath d='M2 6.5C30 4 61 3.2 97 4.1s69 2.4 101-.6' fill='none' stroke='%23546634' stroke-width='1.7' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E"); /* A */
  --loop: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 80' preserveAspectRatio='none'%3E%3Cpath d='M118 9C70 1 16 11 8 37c-7 25 49 39 103 36 50-3 86-17 82-40C189 12 141 3 95 6 71 8 52 13 38 20' fill='none' stroke='%23546634' stroke-width='2' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E"); /* A */
}
```

### A.2 Dark grain and marks

The dark data-URIs are declared once and aliased by A.3 and A.4, so the two dark blocks stay identical and short. This changes no values.

```css
:root {
  --grain-dark:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3' seed='2' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0.95 0 0 0 0 0.92 0 0 0 0 0.85 0.19 0 0 0 -0.072'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E"),
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='173' height='173'%3E%3Cfilter id='n' x='0' y='0'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2' seed='19' stitchTiles='stitch'/%3E%3CfeColorMatrix values='0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.85 0 0 -0.26'/%3E%3C/filter%3E%3Crect width='173' height='173' filter='url(%23n)'/%3E%3C/svg%3E");
  --brush-dark: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 240 24' preserveAspectRatio='none'%3E%3Cpath fill='%23c9a77c' fill-opacity='.36' d='M4 13C38 8 92 9 140 8s80-2 96 2c3 2 1 5-5 5-40 1-110 2-160 4S14 21 6 20c-4-1-5-5-2-7z'/%3E%3Cpath fill='%23c9a77c' fill-opacity='.2' d='M18 18c50-2 120-4 200-3-60 3-130 5-200 5z'/%3E%3C/svg%3E");
  --pencil-dark: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 10' preserveAspectRatio='none'%3E%3Cpath d='M2 6.5C30 4 61 3.2 97 4.1s69 2.4 101-.6' fill='none' stroke='%23a9b77c' stroke-width='1.7' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E"); /* A */
  --loop-dark: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 80' preserveAspectRatio='none'%3E%3Cpath d='M118 9C70 1 16 11 8 37c-7 25 49 39 103 36 50-3 86-17 82-40C189 12 141 3 95 6 71 8 52 13 38 20' fill='none' stroke='%23a9b77c' stroke-width='2' stroke-linecap='round' vector-effect='non-scaling-stroke'/%3E%3C/svg%3E"); /* A */
}
```

### A.3 Manual dark

Set by the theme switch (§7) and by the head flag script when a choice is saved. `--brand` is not redeclared.

```css
:root[data-theme="dark"] {
  color-scheme: dark;
  --paper: #1c1b18;
  --paper2: #25231f;
  --ink: #eeebe3;
  --muted: #a29e95;
  --hair: rgba(238, 235, 227, 0.15);
  --rule: rgba(238, 235, 227, 0.34);
  --accent: #a9b77c; /* A */
  --on-accent: #161a0e; /* A */
  --accent-ink: #a9b77c; /* A */
  --grain: var(--grain-dark);
  --hiMix: #fff 6%;
  --loMix: #000 45%;
  --sand: #c9a77c;
  --sand-ink: #c9a77c;
  --brush: var(--brush-dark);
  --pencil: var(--pencil-dark);
  --loop: var(--loop-dark);
}
```

### A.4 System dark

Applies when the OS is dark and the visitor has not chosen light. It **MUST** match A.3 line for line. Why: a selector and a media query cannot share one block (§4.2).

```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    color-scheme: dark;
    --paper: #1c1b18;
    --paper2: #25231f;
    --ink: #eeebe3;
    --muted: #a29e95;
    --hair: rgba(238, 235, 227, 0.15);
    --rule: rgba(238, 235, 227, 0.34);
    --accent: #a9b77c; /* A */
    --on-accent: #161a0e; /* A */
    --accent-ink: #a9b77c; /* A */
    --grain: var(--grain-dark);
    --hiMix: #fff 6%;
    --loMix: #000 45%;
    --sand: #c9a77c;
    --sand-ink: #c9a77c;
    --brush: var(--brush-dark);
    --pencil: var(--pencil-dark);
    --loop: var(--loop-dark);
  }
}
```

### A.5 Wide screens

At 1920px and wider, only the page width changes (§6).

```css
@media (min-width: 1920px) {
  :root {
    --max: 1680px;
  }
}
```

### A.6 Soft-UI light model

Declared on `*` so each element resolves it against its own `--surf`. Containers override only `--surf` `--hiMix` `--loMix` (§4.6.3). The "on" face `--gface` is ink, not green.

```css
* {
  --hi: color-mix(in srgb, var(--surf), var(--hiMix));
  --lo: color-mix(in srgb, var(--surf), var(--loMix));
  --raise: 4px 4px 10px var(--lo), -4px -4px 10px var(--hi);
  --raise-s: 2px 2px 5px var(--lo), -2px -2px 5px var(--hi);
  --sink: inset 3px 3px 6px var(--lo), inset -3px -3px 6px var(--hi);
  --sink-s: inset 1.5px 1.5px 4px var(--lo), inset -1.5px -1.5px 4px var(--hi);
  --face: var(--surf);
  --gface: var(--ink);
  --on-gface: var(--paper);
}
```

### A.7 Inverse-card surface

The card is ink in light and cream in dark, so it retunes the mixes in both themes (§4.6.4). Its accent lines take the other theme's tone. Why: the page accent is 2.92:1 on ink and 1.81:1 on cream; the swap gives 8.55:1 and 6.96:1.

```css
.next {
  background: var(--ink);
  color: var(--paper);
  --surf: var(--ink);
  --hiMix: #fff 9%;
  --loMix: #000 55%;
  --accent: #a9b77c; /* A */
  --accent-ink: #a9b77c; /* A */
}
:root[data-theme="dark"] .next {
  --hiMix: #fff 70%;
  --loMix: #000 15%;
  --accent: #435425; /* A */
  --accent-ink: #435425; /* A */
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) .next {
    --hiMix: #fff 70%;
    --loMix: #000 15%;
    --accent: #435425; /* A */
    --accent-ink: #435425; /* A */
  }
}
```

Other surfaces: give the background and `--surf` the same token. If the surface is much darker or lighter than the page, retune `--hiMix` / `--loMix` in both themes. A dark surface takes a small white % and a large black %; a light surface takes the reverse.

---

## Appendix B · Page skeleton

The whole `index.html` in order; concatenated, the blocks **are** the file.

### B.1 How to use it

- Copy classes, ids, ARIA, `data-fly` values and inline properties **exactly**. Replace every `{{key}}` (§3) and `{…}` (a §3 fragment or figure microcopy, §3.9); none may remain. Comments inside markup are instructions, not output.
- **Banner shorthand.** Only the A layer rule is printed in full; every shortened banner **MUST** be expanded to its §2.2 form (80-character rules): layer rules like A's, B, C, D parts to three lines (in `<main>` 2 more indent, 2 fewer rule characters), A parts padded with `─`.
- Features are `C.07 … C.(6+N)`, then next issue, contact, footer (`C.(7+N)` … `C.(9+N)`); shown for N = 7. `#fN` = feature position, `figK` = figure number in order of appearance, `F` = `features[N-1]`. §3.7 drops whole elements.

### B.2 Head and styles (A, B)

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- masthead box, verbatim from §2.2 -->

<!-- ━━━━━━━━  A  ·  HEAD  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ -->

<!-- ── A.01 · identity ── -->
<title>{{fullName}} — {{role}}</title>
<!-- ── A.02 · crawling ── -->
<!-- ── A.03 · colour and display ── -->
<!-- ── A.04 · icons and installability ── -->
<!-- ── A.05 · Open Graph ── -->
<!-- ── A.06 · Twitter / X ── -->
<!-- ── A.07 · structured data ── -->
<!-- ── A.08 · early fetches ── -->
<!-- ── A.09 · script flag ── -->
<!-- every A part's tags, and the A.09 flag script, verbatim from §2.5 -->

<!-- ━━  B  ·  STYLES  ━━ -->
<style>
/* B.01  TYPEFACES */ /* see §5.2 */
/* B.02  TOKENS */ /* see §4, Appendix A */
/* B.03  DOCUMENT */ /* see §6 */
/* B.04  TYPE & MARKS */ /* see §5 */
/* B.05  LAYOUT */ /* see §6.3 */
/* B.06  TOP BAR */ /* see §7.1, §7.2 */
/* B.07  COVER */ /* see §7.3 */
/* B.08  OPENING LINE */ /* see §7.4 */
/* B.09  ORBIT */ /* see §8, §15.3.1 */
/* B.10  AT A GLANCE */ /* see §9.1 */
/* B.11  LETTER */ /* see §9.2 */
/* B.12  FEATURES */ /* see §10.3–§10.7 */
/* B.13  PHOTO SLOTS */ /* see §10.12 */
/* B.14  FIGURE TRAYS */ /* see §12.1, §12.4, §12.6 */
/* B.15  CONTROLS */ /* see §12.2, §12.3 */
/* B.16  3D STAGES */ /* see §14.1, §14.2 */
/* B.17  BROWSER TAB */ /* see §13.1–§13.3 */
/* B.18  COMPACT FIGURES */ /* see §13.4, §13.5 */
/* B.19  NEXT ISSUE */ /* see §11.1, Appendix A.7 */
/* B.20  CONTACT */ /* see §11.2 */
/* B.21  FOOTER */ /* see §11.3 */
/* B.22  MOTION & PRINT */ /* see §16, §17 */
/* B.23  SHARED FINISHING */ /* see §2.4, §4.4.3 */
</style>
</head>
<body>
```

### B.3 Bar, cover, opening line (C.01–C.03)

```html
<!-- ━━  C  ·  PAGE  ━━ -->

<!-- C.01  TOP BAR -->
<header class="bar">
  <div class="wrap">
    <a class="name" href="#top">{{nameParts[0].text}} <i>{{nameParts[1].text}}</i> {{nameParts[2].text}}</a>
    <nav aria-label="Sections">
      <a href="#glance">Glance</a><a href="#inside">Inside</a><a href="#letter">Letter</a><a href="#f1">Work</a><a href="#contact">Contact</a>
      <button class="tbtn" id="themeBtn" type="button" aria-label="Switch between light and dark mode">Dark</button>
    </nav>
  </div>
</header>

<main id="top">
  <!-- C.02  COVER  ·  the name across the photo -->
  <section class="cover" aria-label="Cover">
    <div class="cv-hero">
      <div class="strip"><span class="lab">{{role}}</span><span class="note">{city} &middot; {{coordinates}}</span></div>
      <h1 class="mast"><span class="m1">{{nameParts[0].text}}</span> <span class="m2">{{nameParts[1].text}}</span> <span class="m3">{{nameParts[2].text}}</span></h1>
    </div>
    <p class="mast mast-echo" aria-hidden="true"><!-- the h1's three spans, character for character --></p>
  </section>

  <!-- C.03  OPENING LINE -->
  <section class="opening wrap" aria-label="Opening line">
    <p class="hl" data-fly><span class="tt">{first word}</span> {words} <span class="ink">{{opening.inkWord}}</span> {words}&nbsp;<em class="brush">{{opening.brushWord}}</em></p>
  </section>
```

Name classes: §7.3.2. `{city}`: `{{location}}` before the comma.

### B.4 Orbit (C.04)

```html
  <!-- C.04  ORBIT  ·  the signature object and the contents -->
  <section class="orbit wrap" id="orbit" aria-label="The {{orbit.signatureObject}}, and the contents">
    <div class="orbit-pin">
      <figure class="sig" id="sigFig">
        <div class="sig-view" id="sigHost"><canvas id="sigCanvas" tabindex="0" role="img" aria-label="Interactive 3D {{orbit.signatureObject}}. Drag or use the arrow keys to turn it; Q and E roll it."></canvas></div>
        <div class="sig-ui" id="sigUi">
          <div class="sig-ctrls"><!-- the four range rows #rx #ry #rz #sup verbatim from §15.2; D.08 docks them, closed --></div>
          <div class="sig-foot"><span class="sig-status" id="sigStatus" aria-live="polite">{Measure}: starting.</span><button class="btn" id="sigReset" type="button">Reset</button></div>
        </div>
        <figcaption class="note"><span>{{orbit.aside[0]}}</span><span>{{orbit.aside[1]}}</span></figcaption>
      </figure>
    </div>
    <div class="orbit-flow">
      <p class="stand o-r" data-fly style="--mt:4svh">{lede before} <b>{{orbit.keyPhrase}}</b> {lede after}</p>
      <p class="o-hand o-l o-in" data-fly style="--mt:20svh"><span class="hand">{{orbit.aside[0]}} &mdash;<br>{{orbit.aside[1]}}</span><svg viewBox="0 0 160 64" aria-hidden="true" focusable="false"><path d="M6 5c8 19 30 35 62 41 27 5 55 1 79-10"/><path d="M131 27c6 3 12 6 17 9-5 4-11 8-16 12"/></svg></p>
      <p class="sig-note o-r o-in" data-fly style="--mt:12svh"><span class="note">fig. 0 &mdash; {{orbit.signatureObject}}</span>{{orbit.figNote}}</p>
      <nav class="inside" id="inside" aria-label="Contents">
        <span class="lab o-l" data-fly style="--mt:26svh">In this issue</span>
        <a class="o-l" data-fly href="#f1" style="--mt:8svh"><span class="n">01</span><span class="t">{headline 1, plain}</span></a>
        <!-- like 01: 02 o-r o-in 6svh · 03 o-l o-in 16svh · 04 o-r 12svh · 05 o-l 18svh · 06 o-r o-in 9svh · 07 o-l o-in 15svh -->
      </nav>
    </div>
  </section>
```

Stage first, then text in reading order (§8.3.3). `aside[0]`/`[1]`: `orbit.aside` split at its em dash. Other stop counts: §8.9.

### B.5 At a glance and letter (C.05–C.06)

```html
  <!-- C.05  AT A GLANCE  ·  the whole portfolio in one card -->
  <section class="glance g" id="glance" aria-labelledby="glH">
    <div class="gl-top" data-fly="rise"><span class="lab">{{glance.kicker}}</span><span class="gl-time">{{glance.aside}}</span></div>
    <div class="gl-card" data-fly="rise">
      <div class="gl-cover" role="img" aria-label="{{portrait.alt}}"></div>
      <div class="gl-body">
        <div class="gl-id">
          <h2 id="glH" class="gl-name">{given name} <span class="brush">{family name}</span></h2>
          <p class="gl-role">{{role}} <i aria-hidden="true">·</i> {{location}} <i aria-hidden="true">·</i> {{workMode}}</p>
          <p class="gl-line">{{pitch}}</p>
          <div class="gl-acts"><a class="btn solid" href="mailto:{{email}}">{{glance.actions.mail}}</a><a class="btn" href="{{links[0].url}}" rel="noopener">{{glance.actions.link}}</a><a class="btn gl-go" href="#f1">{{glance.actions.work}}</a></div>
        </div>
        <dl class="gl-facts">
          <div><dt>{{glance.facts[0].label}}</dt><dd><b>{{glance.facts[0].numeral}}</b> {{glance.facts[0].value}}</dd></div>
          <!-- facts[1…5] alike; <b> only before a numeral -->
        </dl>
        <div class="gl-stack" role="group" aria-label="Stack">
          <div class="gl-grp"><span class="lab" id="glS1">{{glance.stack[0].label}}</span><ul aria-labelledby="glS1"><li>{{glance.stack[0].items[0]}}</li></ul></div>
          <!-- one .gl-grp per group (glS2, glS3); the last adds .now if now is true -->
        </div>
      </div>
    </div>
  </section>

  <!-- C.06  LETTER  ·  before the work, a note -->
  <section class="letter g" id="letter" aria-labelledby="letH">
    <span class="lab" data-fly>{{letter.kicker}}</span>
    <h2 id="letH" data-fly>{words} <span class="ink wide">{{letter.stampWord}}</span> {words} <span class="loop">{{letter.loopWord}}</span> {words} {penultimate}&nbsp;{last}.</h2>
    <div class="snap snap-desk" data-fly=".6">
      <figure class="snap-in" style="--tilt:-2.4deg"><span class="snap-ph" aria-hidden="true"></span><figcaption>{{letter.photoSlot.caption}} &mdash; photo to come</figcaption></figure>
    </div>
    <div class="cols" data-fly=".55" data-fly-delay=".08">
      <p>{{letter.paragraphs[0]}}</p>
      <p>{… <strong>{{letter.strongPhrase}}</strong> …}</p>
    </div>
  </section>
```

**Filled photo slot** (any `.snap`): `span.snap-ph` → `<img src alt loading="lazy" decoding="async">`; drop "&mdash; photo to come" (§10.12).

### B.6 Feature articles (C.07 – C.(6+N))

Templates in canonical order (sequence: §10.8); L1 shows a complete tray (A1, §12.6.1).

```html
  <!-- C.07  FEATURE 01 -->
  <article class="feature g" id="f1" aria-labelledby="f1h">
    <div class="f-top" data-fly="rise"><span class="no">No. 01</span><span>{{F.field}}</span></div>
    <h2 id="f1h" data-fly>{Words} <em>{hinge}</em> <span class="ink sq">{noun}</span></h2>
    <p class="lede" data-fly=".7">{{F.lede}}</p>
    <div class="f-text" data-fly=".5" data-fly-delay=".06"><p>{{F.body[0]}}</p><p>{{F.body[1]}}</p></div>
    <aside class="side" data-fly=".5" data-fly-delay=".12">
      <dl><div class="row"><dt>{{F.details[0].label}}</dt><dd>{{F.details[0].value}}</dd></div><!-- 3–6 rows --></dl>
    </aside>
    <div class="insert" id="fig1" data-fly="rise">
      <div class="insert-h"><span>Fig. 1 &mdash; try it</span><span>{{F.figure.note}}</span></div>
      <h3>{{F.figure.title}}</h3>
      <p class="cap">{{F.figure.caption}}</p>
      <div class="ctrls">
        <label for="fig1Total"><span id="fig1TotalL">{Units}</span> <output id="fig1TotalO" for="fig1Total">1000</output>
          <select id="fig1Total" aria-labelledby="fig1TotalL"><option>500</option><option selected>1000</option><!-- … --><option>5000</option></select></label>
        <label for="fig1Cap"><span id="fig1CapL">{Limit, per second}</span> <output id="fig1CapO" for="fig1Cap">20</output>
          <input id="fig1Cap" type="range" min="5" max="100" value="20" aria-labelledby="fig1CapL"></label>
        <!-- #fig1Fail like #fig1Cap: 0–30, value 5, output "5%" -->
      </div>
      <div class="runrow"><button class="btn solid" id="fig1Run" type="button">{Run both}</button><button class="btn" id="fig1Reset" type="button">Reset</button></div>
      <div class="race">
        <div class="lane"><h5>{Strategy A}</h5><small>{its rule}</small>
          <div class="track" aria-hidden="true"><i class="d" id="fig1ad" style="width:0"></i><i class="f" id="fig1af" style="width:0"></i></div>
          <div class="nums"><span>{Outcome}<b id="fig1adn">0</b></span><span>{Loss}<b id="fig1afn">0</b></span><span>Time<b id="fig1at">0s</b></span></div></div>
        <div class="lane"><!-- as lane A, ids fig1bd fig1bf fig1bdn fig1bfn fig1bt --></div>
      </div>
      <div class="verdict" id="fig1Verdict" aria-live="polite"></div>
    </div>
  </article>
```

Tray order: `insert-h` → `h3` → `.cap` (not A3) → `.ctrls`/`.fields` → `.runrow` → body → `.verdict`; "&mdash; try it" on Fig. 1 only; labels per §12.2.1. Below, `<!-- base -->` = `.lede`, `.f-text`, `aside.side` as in L1; `{field}` `{note}` `{title}` `{caption}` are `F`'s keys.

```html
  <!-- C.08  FEATURE 02 --> <!-- L2 · A2 -->
  <article class="feature g" id="f2" aria-labelledby="f2h">
    <!-- .f-top as L1: No. 02 -->
    <h2 id="f2h" data-fly><span class="t1">{Word,}</span> {words}<br class="d"> {words} <span class="t2">{word}</span></h2>
    <!-- base -->
    <div class="insert" id="fig2" data-fly="rise">
      <div class="insert-h"><span>Fig. 2</span><span>{note}</span></div><h3>{title}</h3><p class="cap">{caption}</p>
      <div class="calc"><!-- .fields (labels as L1) + .slip + #fig2Pdf, markup §12.6.2 --></div>
    </div>
  </article>

  <!-- C.09  FEATURE 03 --> <!-- L3 · A4 -->
  <article class="feature g" id="f3" aria-labelledby="f3h">
    <div class="f-top" data-fly="rise"><span class="no">03</span><span class="say">{{F.aside}}</span><span>{field}</span></div>
    <h2 id="f3h" data-fly><span class="l1">{line one}</span> <span class="l2"><em>{line two}</em></span></h2>
    <!-- base -->
    <div class="insert" id="fig3" data-fly="rise">
      <div class="insert-h"><span>Fig. 3</span><span>{note}</span></div><h3>{title}</h3><p class="cap">{caption}</p>
      <div class="runrow"><!-- buttons as L1: #fig3Primary solid "{Primary}", #fig3Sim "Simulate {a user}", #fig3Reset --></div>
      <div class="scene3">
        <div class="stage" id="fig3Host"><!-- canvas#fig3Canvas, .tags#fig3Tags, .stage-hint: §14.1 --></div>
        <div class="scene-side">
          <div class="lab">{Groups}</div><div class="toggles" id="fig3Groups" role="group" aria-label="{Groups}"></div>
          <!-- the same pair for .toggles.pick#fig3Stations -->
          <div class="tab" id="fig3Tab"><!-- §13.1: #fig3Url, #fig3List (aria-live), #fig3Total, #fig3Go --></div>
        </div>
      </div>
      <div class="lab tk-h">{Records}<span class="tk-count" id="fig3TkCount"></span></div><div class="tk-scroll" id="fig3TkScroll"><ol class="tickets" id="fig3Tickets"></ol></div>
      <div class="verdict" id="fig3Verdict" aria-live="polite"></div>
    </div>
    <p class="quote" data-fly=".6">{Principle sentence}<br class="d"> <span class="up">{turn}</span></p>
  </article>

  <!-- C.10  FEATURE 04 --> <!-- L4 · A5 -->
  <article class="feature g" id="f4" aria-labelledby="f4h">
    <!-- .f-top as L1: No. 04 -->
    <h2 id="f4h" data-fly><span class="t1">{Word}</span><br> {words}</h2>
    <!-- base -->
    <div class="insert" id="fig4" data-fly="rise">
      <div class="insert-h"><span>Fig. 4</span><span>{note}</span></div><h3>{title}</h3><p class="cap">{caption}</p>
      <div class="scene3">
        <div class="stage sq" id="fig4Host"><!-- as Fig. 3 --></div>
        <div class="scene-side">
          <div class="lab">{User side}</div><div class="tab" id="fig4Tab"><!-- body #fig4Pick, not live --></div>
          <div class="lab">{Tracker}</div><ol class="steps" id="fig4Steps"></ol><p class="steps-now" id="fig4StepNow"></p><p class="steps-log" id="fig4Log">{First step.}</p>
        </div>
      </div>
      <div class="ctrls" style="margin-top:26px"><!-- ranges as L1: #fig4Rate 6–90 (30), #fig4Fail 0–40 (8%) --></div>
      <div class="lab">{Routes}</div><div class="toggles" id="fig4Routes" role="group" aria-label="{Routes}"></div>
      <div class="runrow" style="margin-top:18px"><!-- #fig4Run solid "Pause", #fig4Reset --></div>
      <div class="nums shnums"><span>{Readout}<b id="fig4N1">0</b></span><!-- six spans, #fig4N1 … #fig4N6 --></div>
      <div class="lab tk-h">{Recent records}</div><ol class="orders" id="fig4Orders"></ol>
      <div class="verdict" id="fig4Verdict" aria-live="polite"></div>
    </div>
    <p class="quote" data-fly=".6"><span class="hang" aria-hidden="true">&ldquo;</span>{Principle sentence} <span class="up">{turn}.</span><span aria-hidden="true">&rdquo;</span></p>
  </article>

  <!-- C.11  FEATURE 05 --> <!-- L5 · A0 -->
  <article class="feature g" id="f5" aria-labelledby="f5h">
    <span class="f-num" aria-hidden="true">05</span>
    <div class="f-top" data-fly="rise"><span class="say">{{F.aside}}</span><span>{field}</span></div>
    <h2 id="f5h" data-fly>{Words,}<br class="d"> {words} <span class="wide">{word}</span></h2>
    <!-- base; details run-in, §10.4 -->
    <p class="quote" data-fly=".6">{Principle sentence with one} <span class="up">{turn}</span> {word}.</p>
  </article>

  <!-- C.12  FEATURE 06 --> <!-- L6 · A3 -->
  <article class="feature g" id="f6" aria-labelledby="f6h">
    <!-- .f-top as L1: No. 06 -->
    <h2 id="f6h" data-fly>{Words}<br class="d"> {words} <span class="t2">{word}</span></h2>
    <!-- base -->
    <div class="snap snap-sketch" data-fly=".6">
      <figure class="snap-in" style="--tilt:1.8deg"><span class="snap-ph" aria-hidden="true"></span><figcaption>{{F.photoSlot.caption}}</figcaption></figure>
    </div>
    <div class="insert" id="fig5" data-fly="rise">
      <div class="insert-h"><span>Fig. 5</span><span>{note}</span></div><h3>{title}</h3>
      <div class="seg" role="group" aria-label="{Form} version"><button type="button" id="fig5Before" aria-pressed="false">Before</button><button type="button" id="fig5After" aria-pressed="true">After</button></div>
      <div class="uxwrap"><div id="fig5Host"></div><ul class="why" id="fig5Why"></ul></div>
    </div>
  </article>

  <!-- C.13  FEATURE 07 --> <!-- L7 · A0 -->
  <article class="feature g" id="f7" aria-labelledby="f7h">
    <div class="f-top" data-fly="rise"><span class="no">07</span><span class="say">{{F.aside}}</span><span>{field}</span></div>
    <h2 id="f7h" data-fly>{Words} <em>{hinge}</em> {words}</h2>
    <!-- base; details as claims, §10.4 -->
    <p class="quote" data-fly=".6"><span class="hang" aria-hidden="true">&ldquo;</span>{Principle sentence}<br class="d"> {words} <span class="up ink">{turn}</span> {words}.<span aria-hidden="true">&rdquo;</span></p>
  </article>
```

Part order and number forms: §10.2; hand-set classes: §5.5.

### B.7 Next issue, contact, footer (C.(7+N) – C.(9+N))

```html
  <!-- C.14  NEXT ISSUE -->
  <section class="g" id="next" aria-labelledby="nextH">
    <div class="next" data-fly="rise">
      <div><span class="lab">{{next.kicker}}</span><h2 id="nextH">{word one} <em>{word two.}</em></h2></div>
      <div>
        <p>{{next.body}}</p>
        <p class="built"><span class="note">{{next.built.note}}</span><br>{{next.built.items}}</p>
        <button class="btn soon" type="button" disabled aria-disabled="true">{{next.button}}</button>
      </div>
      <div class="snap snap-process">
        <figure class="snap-in" style="--tilt:3.2deg"><span class="snap-ph" aria-hidden="true"></span><figcaption>{{next.photoSlot.caption}}</figcaption></figure>
      </div>
    </div>
  </section>

  <!-- C.15  CONTACT -->
  <section class="g" id="contact" aria-labelledby="conH">
    <h2 class="con-h" id="conH" data-fly>{{contact.lead}}</h2>
    <a class="mail" href="mailto:{{email}}" data-fly=".6"><span class="m1">{local part}</span><wbr><span class="at">@</span><span class="m2">{domain}</span></a>
    <div class="links" data-fly=".5"><a href="{{links[0].url}}" rel="noopener">{{links[0].label}}</a><!-- one <a> per link --></div>
  </section>

</main>

<!-- C.16  FOOTER -->
<footer>
  <div class="wrap">
    <p>&copy; <span id="yr">{year}</span> {{fullName}}. {{colophon.note}}</p>
    <p class="colo">{the fixed colophon line, §3.2}</p>
  </div>
</footer>
```

The card arrives whole (no inner `data-fly`). `next.mode: "open"` → `<a class="btn" href="mailto:{{email}}">`. The `.mail` spans **MUST** touch the `<wbr>`. Why: whitespace prints inside the address.

### B.8 Scripts (D)

Each subtitle is the script's purpose; each banner is followed by one classic `<script>` holding one IIFE (§2.6). The order is load-bearing (§2.3); no `type="module"`, `defer`, `async`; the PDF library loads on intent (§2.7).

```html
<!-- ━━  D  ·  SCRIPTS  ━━ -->

<!-- D.01  ARRIVALS  ·  things travel into place as you scroll -->
<!-- D.02  three.js r149  ·  vendor library, MIT licence, in assets/vendor --> <script src="assets/vendor/three.min.js"></script>
<!-- D.03  THEME & FOOTER  ·  light and dark, the year -->
<!-- D.04  SIGNATURE OBJECT  ·  the hand-turned drawing in the orbit -->
<!-- D.05  FIGURE KIT  ·  the custom dropdown and the 2D figures -->
<!-- D.06  3D FIGURES  ·  the scenes on a shared Stage -->
<!-- D.07  PHOTO GUARD  ·  no context menu or drag on the portrait -->
<!-- D.08  CONTROL DRAWERS  ·  settings folded into one line -->
<!-- D.09  PINNED DRAWER  ·  the drawer opens over the pinned object -->
<!-- D.10  COMPACT FIGURES  ·  tab sheets and the ticket row, ≤ 900px -->
</body>
</html>
```

---

## Appendix C · Constants registry

Every number that tunes motion or interaction, by system. Values are normative: a build **MUST** use them unless the cited chapter allows otherwise. Units: s, ms, CSS px, world units (u), vh, vw. `^dt` decays are per second, so behaviour is the same at any refresh rate. Formulas live in the chapter cited.

### C.1 Arrivals (`[data-fly]`)

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| `TRAVEL` / `DRIFT` | 1100 / 760 | px (min-width) | mode thresholds; below 760 is `rise` | §16.2.2 |
| pull default · delay cap | 1 · 0.36 | factor · fraction | `data-fly` empty; `data-fly-delay` clamp | §16.2.1 |
| rise delay timer | `d0 × 900` | ms | rise-mode lag per delay unit | §16.2.1 |
| sideways clamp · dead zone | ±48 · 16 | px | drift or wide; \|dx\| below → 0 | §16.2.2 |
| wide · tilt-eligible | > 0.64 · < 0.45 vw ink, < 0.6 vh tall | — | wide never tilts | §16.2.2 |
| `dy` travel | `clamp(0.06·vh, 28, 72)`, ×1.2 wide | px | start offset below home | §16.2.2 |
| `dy` drift · rise | 40 · 22 | px | start offset | §16.2.2 |
| start line | `0.96 + jS` | vh | progress 0 | §16.2.2 |
| landing line | 0.58 travel · 0.66 drift | vh | progress 1 | §16.2.2 |
| min journey | 40 | px | `b ≤ a − 40` | §16.2.2 |
| rise arm line | 0.9 | vh | timer arms when top passes | §16.2.2 |
| stiffness `k` | `170 × (0.86 + 0.28·jit₁)` | 1/s² | 146.2–193.8 | §16.2.2 |
| damping ratio ζ | `0.64 + 0.15·jit₂` | — | `c = 2ζ√k` | §16.2.2 |
| tilt magnitude | `0.35 + 0.9·jit₄` | deg | 0.35–1.25 | §16.2.2 |
| start jitter `jS` | `(jit₅ − 0.5) × 0.06` | vh | ±0.03 | §16.2.2 |
| `jit` hash | 12.9898 · 78.233 · 43758.5453 | — | `fract(sin(i·a + salt·b)·c)` | §16.2.2 |
| substeps · dt cap · wake dt | `ceil(dt·120)` · 0.05 · 1/60 | — · s · s | integrator | §16.2.2 |
| rest | 0.001 · 0.004 | position · velocity | sleep thresholds | §16.2.2 |
| overshoot caps | 5% x · 10% y | of offset | then `14·tanh(x/14)` sideways | §16.2.2 |
| opacity ramp | `q / 0.45` | — | fully opaque at 45% | §16.2.2 |
| admission | `s + 1.2·vh` | px | sets `will-change` | §16.2.2 |
| idle settle | 180 · `vh − 24` | ms · px | after scroll stops, land what is visible | §16.2.2 |
| re-measure debounce | 140 | ms | resize, fonts, observer | §16.2.3 |
| first screen | 0.9 | vh | lands at load | §16.2.5 |
| hash landing | `target + 1.2·vh` | px | lands above this | §16.2.5 |
| `flyShow` failsafe | 2.5 | s | CSS reveal if no script | §16.2.1 |

### C.2 Signature object: physics, glow, input

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| camera | FOV 30, z 8.3, far 60; DPR ≤ 2 | deg · u | near-axonometric | §15.3.2 |
| grid · `U` · `H` · `ZL` | 400 · 100 · 0.07 · 0.078 | design · u | geometry scale, half-thickness, trace layer | §15.3.3 |
| `G` · `FR` · `V0` | 2.2 · 1.1 · 0.9 | u/s² · 1/s · u/s | gravity, friction, launch speed | §15.3.6 |
| supply `D` | 1.1 (0–4.4, step 0.1) | u/s² | readout `(D/G)` × gravity | §15.3.6 |
| substep | ≤ 0.004 | s | `n = ceil(dt/0.004)` | §15.3.6 |
| re-launch wait | arrive `0.2 + 0.5·r` · return `0.15 + 0.35·r` | s | parked time | §15.3.6 |
| initial wait | `0.15 + 0.13·line + 0.55·bead + 0.3·r` | s | cascade | §15.3.6 |
| `BEADS` · `TR` · `NPTS` | 3 · 7 · 212 | count | per line, per bead, buffer | §15.3.5 |
| trail gap | `0.012 + min(\|v\|·0.022, 0.05)` | u | stretches with speed | §15.3.5 |
| bead alpha · size | `0.95(1 − t/7)²` · `0.085(1 − 0.55t/7)` | — · u | moving; parked 0.4 · 0.06 | §15.3.5 |
| sink energy τ · ease | 0.9 · `min(1, 8dt)` | s · — | `bright` target `1 − e^(−E/3)` | §15.3.7 |
| status window · start | 1.4 · 1.6 | s | "recent" arrival/return; "starting" | §15.3.8 |
| flowing share | 0.8 | of N | full vs partial | §15.3.8 |
| `REST` | x 14, y −28, z 0 | deg | three-quarter view | §15.6 |
| hold threshold | 2 | px (Manhattan) | drag takes the object | §15.6 |
| drag gain · fling gain | 0.55 · 33 | deg/px · (deg/s)/px | 33 = 0.55 × 60 | §15.6 |
| fling decay · stop | `0.0006^dt` · 1.2 | /s · deg/s | coast | §15.6 |
| key step | 5 (Shift 15) | deg | ←/→ ↑/↓ Q/E | §15.6 |
| scroll yaw | ±12, `min(1, 4dt)` | deg · — | τ 0.25 s | §15.6 |
| `PIN` | min-width 1100, min-height 680 | px | pinned stage query | §8.5 |
| make room | gap 6, clearance 12, floor 8, min scale 0.6 | px · — | 0.55 s transition | §8.4 |

### C.3 Stage input, zoom, idle

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| turn gain | yaw 0.006 · pitch 0.004 | rad/px | drag | §14.5 |
| yaw range · pitch range | `yaw0 ± 0.9` · 0.3–1.3 | rad | never under the floor | §14.5 |
| fling decay · stop | `0.002^dt` · 0.0004 | /s · rad/frame | yaw only | §14.5 |
| drag start | 5 | px (Manhattan) | then `.grab` | §14.5 |
| zoom range | 0.8–4.5 (keys off at 4.49 / 0.81) | × | dolly | §14.5 |
| zoomed state | > 1.03 | × | one finger pans, caption hides | §14.1 |
| pan clamp · foreshortening floor | `±1.1·fit[0]` · 0.35 | u · — | `sin pitch` floor | §14.5 |
| wheel | `exp(−deltaY·0.01)` | × | Ctrl/⌘ only | §14.5 |
| key turn · key zoom | 14 · 15 px of drag · ×1.25 / ×0.8 | — | ←/→, ↑/↓, +/− | §14.5 |
| pill zoom | ×1.4 / ÷1.4 | × | about the centre | §14.5 |
| double tap | 320 | ms | reset | §14.5 |
| tiny host | < 300 | px | small keys, no caption | §14.1 |
| visibility | `[0, 0.35]` | ratio | rAF off; `onSeen` > 0.35 | §14.6 |
| idle return | 6 · `1 − e^(−0.8dt)` | s · — | τ 1.25 s | §14.6 |
| label cull | 1.3 | NDC | hide past the frame | §14.2 |

### C.4 Flows, flights, paths, scheduler

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| spring + swirl | k 10, c 5.2, swirl 5.5 over 1.6 | /s² · /s · u | ζ ≈ 0.82 | §14.11 |
| slerp | `1 − e^(−4.5dt)` | — | orientation | §14.11 |
| card trail | 7 pts, `0.03·v`, size 0.13, speed > 0.25 | — · u | | §14.11 |
| stagger | `0.05k + 0.12·group`, scatter 0.35 | s | sort | §14.11 |
| arc lift | 1.6 room · 0.35–0.8 short | u | apex `h/2` | §14.11 |
| comet | 9 pts, step 0.035, size 0.15; start −0.16·i | — · u · s | flights | §14.11 |
| pop-in | `s += 6dt`, `1 − (1 − s)³` | — | landing | §14.11 |
| walker · carrier | 2.1 · 2.3 | u/s | path speed | §14.11 |
| dwell | 0.7 | s | at each stop | §14.11 |
| legs · transfers | 0.7–1.0 · 0.5–0.55 (−0.12·k) | s | scheduler | §14.11 |
| fade | 0.6 to scale 0.001 | s | back to pool | §14.11 |
| queue compaction | `min(1, 6dt)` | — | lerp to slot | §14.11 |
| glow capacity | 900 · 1600 busy | points | per stage | §14.9 |
| spotlight hold | 4000 | ms | user's finished item | §14.12 |
| reduced motion | flights 0.01 s; waits ≤ 120 ms | — | | §14.13 |

### C.5 Sheet and drag (≤ 900px)

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| compact query | max-width 900 | px | launcher + sheet | §13.4 |
| open | 440 from launcher / 380 slide · scrim 300 | ms | opacity 1 at 30% | §13.4 |
| close | 340 into launcher / 260 drop · scrim 300 | ms | opacity held to 55% | §13.4 |
| dismiss | > 90, or > 16 at > 0.5 | px · px/ms | drag down | §13.4 |
| scrim while dragging | `max(0.2, 1 − dy/480)` | — | | §13.4 |
| spring-back | 240 | ms | | §13.4 |
| pick → open · go → close | 520 · 480 | ms | the scene reacts first | §13.4 |
| reduced motion | 160 fade in, instant close | ms | | §13.4 |
| launcher hint | 1.5 × 2 | s | first full view, URL change | §13.4 |

### C.6 Ticket row springs (≤ 900px)

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| `G` · card width | 12 · `min(236px, 76vw)` | px | gap | §13.5 |
| `K` · `C` | 240 · 23 | /s² · /s | position, ζ ≈ 0.74 | §13.5 |
| `SWIRL` | 520 fading over 180 | px/s² · px | curve, then land straight | §13.5 |
| height spring | 380 · 36 | /s² · /s | ζ ≈ 0.92 | §13.5 |
| stagger · wave gate | 0.045 · moved > 1, \|v\| < 20 | s · px · px/s | | §13.5 |
| new card | +48 · fade 320 | px · ms | ease-out | §13.5 |
| list reveal | 380, `[0, 0 @ 25%, 1]` | ms | open card | §13.5 |
| dt clamp | 0.001–0.032 | s | | §13.5 |
| snap | dist < 0.3, \|v\| < 3; height < 0.5, < 1 | px · px/s | | §13.5 |
| tilt · scale | `clamp(vx·0.0028, ±3.5)` · `1 + min(0.035, \|v\|/26000)` | deg · × | | §13.5 |

### C.7 Controls, drawers, demo, status, theme

| Constant | Value | Unit | Meaning | Ref |
|---|---|---|---|---|
| type-ahead buffer | 600 | ms | reset after silence | §12.2.3 |
| dropdown list | `min(264, n·44 + 14)`, margin 16, bar 54 | px | flips up if no room | §12.2.3 |
| drawers open | min-width 701 | px | until first manual toggle | §12.3 |
| closed-drawer summary | 1000 | ms | refresh interval | §12.3 |
| race tick `TICK_MS` | 45 | ms | one simulated second | §12.6.1 |
| groove transition | 0.05 | s | just exceeds the tick | §12.4 |
| demo prerequisite · lead-in | +2200 · 1300 | ms | Simulate | §12.5 |
| demo step gap | `clamp(2600 / steps, 170, 520)` | ms | | §12.5 |
| demo commit | +500 | ms | after last step | §12.5 |
| auto-play | 900 after > 35% visible | ms | once, untouched | §12.5 |
| reduced-motion wait cap | 120 | ms | per scripted wait | §12.5 |
| signature status | 250 (4 Hz) | ms | write on change | §15.3.8 |
| figure readouts | ≥ 0.25 apart (4 Hz) | s | DOM work rate | §14.6 |
| status fallback | 5000 | ms | silence → counter summary | §12.4 |
| tracker pulse | 1.4 infinite | s | ring 0→5px | §16.4 |
| theme probe | every 20 frames | frames | compare token key, re-tint | §14.6, §15.3.9 |
| theme reveal | 700 | ms | circle clip from the button | §7.2.6 |

A re-tuned value **SHOULD** keep its ratio to its neighbours (sheet close shorter than open; ζ ≥ 0.6). Why: otherwise the motion stops reading as one hand.
