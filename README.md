# Arman Afridi Rahman · Portfolio

> **Software that is built to run *well*.**
> A single page at **[armanafridirahman.github.io](https://armanafridirahman.github.io/)**:
> editorial type, a 3D chip you can turn in your hands, and five working demos that show how the
> systems behind the case studies behave.

| | |
|---|---|
| **Entry point** | `index.html`: markup, styles and the site's own scripts. Binary assets live in `assets/`. |
| **Build step** | None. Open the file in a browser, or push to `main` and GitHub Pages serves it. |
| **Network at runtime** | Same-origin only. The portrait and the seven fonts on the first screen are preloaded; three.js loads with the page; jsPDF loads only when the payslip is downloaded. |
| **Size** | `index.html` ≈ 350 KB unminified. `assets/`: three.js 608 KB, jsPDF 364 KB (on demand), portrait 201 KB, fonts 137 KB. |
| **Browsers** | Evergreen browsers. `color-mix()` is required; View Transitions, popovers and `CloseWatcher` are used where they exist. |

---

## Contents

1. [How the file is layered](#1--how-the-file-is-layered)
2. [Map of the file](#2--map-of-the-file)
3. [Design system](#3--design-system)
4. [The page, section by section](#4--the-page-section-by-section)
5. [Behaviour, script by script](#5--behaviour-script-by-script)
6. [Contracts between the layers](#6--contracts-between-the-layers)
7. [Decisions worth knowing before you change anything](#7--decisions-worth-knowing-before-you-change-anything)
8. [Common edits](#8--common-edits)
9. [Other files in the repository](#9--other-files-in-the-repository)

---

## 1 · How the file is layered

`index.html` reads top to bottom in four layers. Each layer opens with a heavy rule, each part of a
layer with a double-ruled banner that carries a short code, and each group inside a part with a light
rule. The codes are the stable anchors: search the file for `B.15` or `D.06` and you land on that part.

```
<!-- ━━━━━━━━  B  ·  STYLES  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ -->      layer
/* ═════════════════════════════════════════════════════════════════════════════
   B.09  ORBIT  ·  the chip holds the middle while words pass                        part
   ══════════════════════════════════════════════════════════════════════════ */
/* ── chip ────────────────────────────────────────────────────────────────── */      group
```

| Layer | What it holds | Codes |
|---|---|---|
| **A · Head** | identity, crawling, colour, icons, social cards, structured data | `A.01` – `A.09` |
| **B · Styles** | one `<style>` element, one part per component | `B.01` – `B.23` |
| **C · Page** | the markup of the bar, `<main>` and the footer | `C.01` – `C.16` |
| **D · Scripts** | ten `<script>` elements, in load order (`D.02` from `assets/vendor`) | `D.01` – `D.10` |

**Styles are organised by component.** Each component's part holds everything about it: its base
rules, its states, its pressed-and-raised soft look, and its adjustments for every screen size, with
`@media` blocks placed right after the rules they adjust. A selector is written once per context
wherever the cascade allows; the few places where a rule repeats are deliberate, because a rule in
between must win for some state (for example `.btn:hover` and `.btn:disabled`). Rules that serve
several components at once live together in `B.23 Shared finishing`, near the end, because they
must override the components before them.

**Formatting** is standard: one declaration per line in the CSS, and the scripts formatted
consistently (two-space indent, double quotes, 100 columns). Each script is a single IIFE in ES5
style (`var`, `function`), and nothing is global.

---

## 2 · Map of the file

Line numbers are approximate and drift with edits; the codes do not.

### A · Head

| Code | Line | Part |
|---|---:|---|
| — | 6 | Masthead: a boxed contents card |
| `A.01` | 24 | Identity: title, description, keywords, author, canonical, `image_src` |
| `A.02` | 45 | Crawling: robots, Googlebot, Bingbot, search-console verification |
| `A.03` | 52 | Colour and display: `color-scheme`, two `theme-color` metas (`#tcLight`, `#tcDark`) |
| `A.04` | 59 | Icons and installability: favicon, touch icon, manifest, app titles |
| `A.05` | 71 | Open Graph, profile fields |
| `A.06` | 88 | Twitter / X card |
| `A.07` | 97 | JSON-LD graph: `WebSite`, `ProfilePage`, `Person` |
| `A.08` | 200 | Early fetches: preloads the portrait and the seven first-screen fonts |
| `A.09` | 210 | Adds the `js` class to `<html>` before first paint |

### B · Styles

| Code | Line | Part | Groups inside |
|---|---:|---|---|
| `B.01` | 216 | Typefaces | nine `@font-face` rules pointing at `assets/fonts/*.woff2` |
| `B.02` | 283 | Tokens | one `:root` block for light, one for manual dark, one for system dark, `--max` at 1920px · portrait (`--me`, `assets/portrait.webp`) · per-element light (`--hi`, `--lo`, `--raise`, `--sink`) |
| `B.03` | 402 | Document | reset and body (including body size at 300 / 360 / 1600 / 2200px) · theme transition · scrollbars · taps and focus · utilities (`.hidden`, `.nw`) |
| `B.04` | 579 | Type & marks | labels (`.lab`, `.note`, `.hand`) · ink · marks (`.brush`, `.loop`) |
| `B.05` | 636 | Layout | the `.g` grid and its named lines |
| `B.06` | 662 | Top bar | bar · theme switch |
| `B.07` | 790 | Cover | `.mast`, `.cv-hero`, the shade, `.mast-echo`, every width |
| `B.08` | 922 | Opening line | `.hl` |
| `B.09` | 954 | Orbit | frame and lanes · contents · chip · one column |
| `B.10` | 1385 | At a glance | placement · card · ≤ 999 / 640 / 380 / 300px · short landscape · facts as a list on hairlines |
| `B.11` | 1815 | Letter | heading, columns, drop cap, desk photo |
| `B.12` | 1908 | Features | parts · detail tables · spacing and type scale · hand-set words · phones and tablets · ≥ 1100px with F1 – F7 |
| `B.13` | 2520 | Photo slots | `.snap` prints, tape, hatching, dark variants |
| `B.14` | 2602 | Figure trays | tray · Fig. 1 dispatch race · Fig. 2 payslip · Fig. 5 form |
| `B.15` | 3037 | Controls | labels, fields and dropdowns · sliders · buttons · quiet links · segmented control · chips · drawers |
| `B.16` | 3620 | 3D stages | stage and labels · zoom keys |
| `B.17` | 3843 | Browser tab | tab · staff tickets · Fig. 4 checkout and readouts · Fig. 4 tracker · Fig. 4 recent orders |
| `B.18` | 4396 | Compact figures | launcher · sheet · ticket row (≤ 900px) |
| `B.19` | 4839 | Next issue | the dark `.next` card and its buttons |
| `B.20` | 4966 | Contact | `.con-h`, `.mail`, `.links` |
| `B.21` | 5067 | Footer | |
| `B.22` | 5102 | Motion & print | arrivals · print |
| `B.23` | 5205 | Shared finishing | rounded small labels · sand underlines · accent ink |

### C · Page

| Code | Line | Section | Anchor |
|---|---:|---|---|
| `C.01` | 5272 | Top bar | `header.bar` |
| `C.02` | 5286 | Cover | `section.cover` |
| `C.03` | 5297 | Opening line | `section.opening` |
| `C.04` | 5304 | Orbit | `#orbit`, `#inside` |
| `C.05` | 5340 | At a glance | `#glance` |
| `C.06` | 5375 | Letter | `#letter` |
| `C.07` | 5392 | Feature 01 · Dispatching under limits | `#f1`, Fig. 1 `#try-dispatch` |
| `C.08` | 5444 | Feature 02 · Time, pay and the payslip | `#f2`, Fig. 2 `#try-payslip` |
| `C.09` | 5488 | Feature 03 · Tables, menus and the order | `#f3`, Fig. 3 `#try-floor` |
| `C.10` | 5544 | Feature 04 · Checkout to the shelf | `#f4`, Fig. 4 `#try-shop` |
| `C.11` | 5610 | Feature 05 · WordPress, done properly | `#f5` (no demo) |
| `C.12` | 5633 | Feature 06 · Interface decisions that matter | `#f6`, Fig. 5 `#try-ux` |
| `C.13` | 5670 | Feature 07 · Agents, plugins and patterns | `#f7` |
| `C.14` | 5693 | Next issue | `#next` |
| `C.15` | 5713 | Contact | `#contact` |
| `C.16` | 5728 | Footer | `footer` |

### D · Scripts

| Code | Line | Script | Groups inside |
|---|---:|---|---|
| `D.01` | 5739 | Arrivals | collect · measure · one element · the loop |
| `D.02` | 6128 | three.js r149 | `<script src="assets/vendor/three.min.js">`, the official build |
| `D.03` | 6133 | Theme & footer | footer year and GitHub link · theme |
| `D.04` | 6221 | Cover chip | theme colours · scene · power lines · beads · physics · colours · orientation · size and loop |
| `D.05` | 6857 | Demos | custom dropdown · Fig. 1 dispatch simulator · Fig. 2 payslip · Fig. 2 receipt PDF · Fig. 5 form |
| `D.06` | 7501 | 3D figures | helpers · theme colours · glow points · arc and path · Stage · turn, pan, zoom · zoom keys · materials · Fig. 3 · Fig. 4 |
| `D.07` | 10327 | Photo guard | — |
| `D.08` | 10343 | Control drawers | — |
| `D.09` | 10468 | Pinned chip drawer | — |
| `D.10` | 10515 | Compact figures | browser tabs → sheet · staff tickets |

---

## 3 · Design system

### Idea

Ink on paper, one leaf-green accent used sparingly, and controls that look moulded out of the page
itself. The reading parts stay editorial: serif headlines, light sans text, mono notes, a pencil hand.

### Colour tokens (`B.02`)

| Token | Light | Dark | Role |
|---|---|---|---|
| `--paper` | `#e9e6de` | `#1c1b18` | page |
| `--paper2` | `#e0ddd3` | `#25231f` | trays, cards, a shade deeper than the page |
| `--ink` | `#151412` | `#eeebe3` | text, rules |
| `--muted` | `#54514a` | `#a29e95` | secondary text |
| `--hair` / `--rule` | ink at 14% / 32% | ink at 15% / 34% | hairlines, field underlines |
| `--brand` / `--accent` | `#546634` | `#a9b77c` | the one accent: leaf green sampled from the photo's foliage |
| `--accent-ink` | `#435425` | `#a9b77c` | accent for small text, deeper so it reads on the grain |
| `--on-accent` | `#f6f4ec` | `#161a0e` | text on an accent fill |
| `--sand` / `--sand-ink` | `#9a7650` / `#7a5b3b` | `#c9a77c` | the warm streak in the granite: brush marks, "now" tags |
| `--grain` | SVG noise | SVG noise | paper tooth; averages within 1% of `--paper` |

Dark mode is declared twice on purpose: once for `:root[data-theme="dark"]` (a manual choice) and once
inside `prefers-color-scheme: dark` for `:root:not([data-theme="light"])` (system choice, no override).
Any new dark rule needs both.

### Type (`B.01`)

| Family | Weights | Variable | Used for |
|---|---|---|---|
| Instrument Serif | 400, 400 italic | `--serif` | headlines, ledes, quotes, numerals |
| Jost | 300, 400, 500 | `--sans` | body text, labels |
| DM Mono | 400 | `--mono` | notes, figure captions, detail tables |
| Nunito | 600, 700 | `--ui` | everything you press (`B.15`) |
| Caveat | 500 | `--hand` | the pencilled aside in the orbit |

Headlines are spaced by hand: words carry classes such as `.t1`, `.t2`, `.sq`, `.wide`, `.l1`, `.l2`
with their own `letter-spacing` (`B.12 › hand-set words`). `.ink` thickens a word with a hairline
text shadow, like a stamp that spread.

### Soft UI (`B.02`, applied in each component)

Every element computes its own light from `--surf`, the colour of whatever it sits on:

| Variable | Meaning |
|---|---|
| `--surf` | the surface under a control (`--paper`, `--paper2` in trays, `--ink` in the Next card) |
| `--hi` / `--lo` | `--surf` mixed with `--hiMix` / `--loMix` (white / warm brown in light, inverted in dark) |
| `--raise` / `--raise-s` | outer shadows: a control standing out of the surface |
| `--sink` / `--sink-s` | inner shadows: a groove pressed into it |
| `--face`, `--gface`, `--on-gface` | a control's face, the filled "on" face, and text on it |

Things that are on or chosen fill with green; nothing glows. To put controls on a new surface, set
`--surf` on the container and the shadows follow.

### Grid (`B.05`)

Every section below the orbit is its own grid, `.g`, with twelve columns inside the frame and a bleed
track to each screen edge. Named lines: `col N` (left edge of column N), `col-end`, `bleed-start`,
`bleed-end`. `--max` is 1560px (1680px from 1920px wide), `--gut` is the side gutter, `--cg` the column
gap. Bleeds alternate down the page: the glance card runs off the right, the Fig. 4 tray off the
left, the Next card off the right.

Sections are spaced by eye, not by a scale: long pauses before the work and before the end, short
ones between pairs. Each section's top margin sits with the section's own styles (`.glance` in
`B.10`, `.letter` in `B.11`, `#f1` – `#f7` in `B.12 › spacing and type scale`, `#next` in `B.19`,
`#contact` in `B.20`).

### Breakpoints

| Width | What changes |
|---|---|
| ≥ 2200 / ≥ 1600 | body text 19px / 18px |
| ≥ 1280 | glance label beside the card; F5 details stacked |
| ≥ 1100 (and height ≥ 680) | orbit pins the chip; features get their own wide compositions; arrivals travel |
| 761 – 1099 | tablet compositions; arrivals drift |
| ≤ 1099 or height ≤ 679 | orbit falls back to one column |
| ≤ 900 | compact figures: browser tabs become launchers and sheets, tickets become a row |
| ≤ 760 | bar keeps only "Glance"; phone compositions; arrivals rise |
| ≤ 700 | control drawers start closed |
| ≤ 620 / 460 / 360 / 300 | progressively compact panels, down to watches |
| height ≤ 520, landscape | bar stops sticking, figures fit the viewport |

---

## 4 · The page, section by section

**C.01 Top bar.** Name, five section links (only "Glance" survives on phones), and the theme switch
`#themeBtn`. The bar is sticky.

**C.02 Cover.** The portrait (`--me`) fills a rounded card; the name is set in light ink across it.
The name runs off the bottom of the photo: the photo's edge cuts the letters a little above the
baseline and a second, ink copy (`.mast-echo`, `aria-hidden`) carries them on below. Both share
`--cut`. The name stays on one line at every width.

**C.03 Opening line.** "Software that is built to run well." The largest statement on the page.

**C.04 Orbit.** On wide screens the chip sits in a sticky layer the height of the screen
(`.orbit-pin`) and the words travel past in a layer above it (`.orbit-flow`), in a lane on each side,
leaving the middle to the chip. That layer only takes clicks on the words, so the chip stays
draggable everywhere else. Contents (`#inside`, links 01 – 07) are set as stops along the way.
Narrower or shorter screens get one column with the chip in its place in the text.

**C.05 At a glance.** One card for anyone who will not scroll: the portrait as a cover band, name,
role, a one-line pitch, three actions (email, LinkedIn, the work), six facts and the stack in three
groups. The "Building with now" tags carry a sand crescent on their left curve.

**C.06 Letter.** A note before the work, in two columns with a drop cap, plus an empty photo slot
for the desk.

**C.07 – C.13 Features.** Seven case studies with the same parts, composed differently at each width:

| Part | Class |
|---|---|
| top line (number, a spoken aside, the field) | `.f-top` |
| headline | `h2#fNh` |
| lede | `.lede` |
| body | `.f-text` |
| details table | `aside.side dl` |
| interactive figure | `.insert` |
| pull quote | `.quote` |

| No. | Topic | Figure |
|---|---|---|
| 01 | Bulk email, SMS and WhatsApp from a weak server | Fig. 1 · Blast it, or pace it? |
| 02 | Per-employee time and payslips | Fig. 2 · From hours to payslip, with a real PDF |
| 03 | Restaurant SaaS with a QR code per table | Fig. 3 · Sort, scan, order (three.js) |
| 04 | Ecommerce: gateways with failover, warehouse picking | Fig. 4 · Checkout to the doorstep (three.js) |
| 05 | WordPress plugins and themes | none |
| 06 | Interface decisions | Fig. 5 · The same form, twice |
| 07 | Sub-agents, plugins and patterns for AI | none |

**C.14 Next issue.** A dark card bleeding to the right: what is being built now, a disabled "Coming
soon", and a taped photo slot.

**C.15 Contact.** A large email link with a pencil underline on hover, LinkedIn, and a GitHub link
that appears when `GITHUB_USER` is set (it is).

**C.16 Footer.** Copyright year (filled by script) and a colophon.

---

## 5 · Behaviour, script by script

### D.01 · Arrivals

Elements marked `data-fly` start a little below and toward the middle of the screen, then travel to
where they live as you scroll.

| Attribute | Effect |
|---|---|
| `data-fly` | full pull toward the middle (`1`), or a fraction such as `.5` |
| `data-fly="rise"` | no sideways travel, a short rise only (wide panels and figures) |
| `data-fly-delay` | leaves later than its neighbours, `0` to `.36` of the journey |

How it works:

- **Collect once.** Nested `data-fly` elements are ignored; the outer one moves. Each element gets a
  spring (stiffness ≈ 146 – 194, damping ratio 0.64 – 0.79, so a small overshoot) and a tilt, from a
  hash of its index rather than `Math.random`, so every visit looks the same.
- **Measure rarely.** Resting positions are read with transforms removed, on load, resize, font load
  and body size changes, never per frame, in one write → read → write batch.
- **Target from scroll.** Progress runs from 0 (top of the element at the bottom of the screen) to 1
  (a little above the middle) and only ever moves forward, so what you have read stays put on the way
  back up. When scrolling stops for 180 ms, anything on screen finishes landing.
- **Integrate.** Semi-implicit Euler at 120 Hz steps. The loop sleeps when nothing moves.
- **Modes.** `travel` from 1100px (sideways travel and tilt), `drift` from 760px (sideways capped at
  48px), `rise` below (a short rise on a timer as things come into view).
- **Always readable.** Reduced motion, print, a jump to a `#section`, keyboard focus and pressing on
  an element all land things immediately. If the script never runs, CSS shows everything after 2.5 s.

Sets `html.fly-ready` once running.

### D.03 – D.05 · Theme, cover chip and demos

**Theme.** `data-theme` on `<html>` holds a manual choice, saved in `localStorage["theme"]`; without
it the system preference rules. The switch keeps both `theme-color` metas in step. Where View
Transitions exist, the new theme grows out of the button as a circle over 700 ms.

**Cover chip (three.js).** A package of hairline edges with 13 pins per side, five power lines from
pins on the top edge to the core, and three glowing beads per line, drawn as one `Points` cloud with
a small shader. Each bead feels three forces along its trace:

```
acceleration = supply − gravity × (how much this segment points up in the world) − friction × speed
```

with gravity `2.2`, friction `1.1`, and supply from the slider (default `1.1`, shown as a multiple of
gravity). Turn the chip so a trace climbs and its beads slow, stall and slide back to the source. The
core brightens with arrivals. The status line reports how many of five lines are flowing.

Turning: drag, sliders, arrow keys (Shift for bigger steps), Q and E to roll. Rotation order is
`ZXY`. While the chip is pinned and untouched, scrolling turns it ±12° across the orbit; the first
touch folds that turn into the chip's own angle so nothing jumps. Reset hands it back to the scroll.
Colours are read live from the CSS tokens every 20 frames, so the theme switch recolours the chip.

**Custom dropdown.** Replaces a native `<select>` with an ARIA combobox and listbox that match the
page: arrow keys, Home, End, Enter, Space, Escape, type-ahead. The native select stays in the DOM,
hidden, and still fires `input` and `change`. Used for `#total`, `#pCur` and the Fig. 5 payment
field.

**Fig. 1 · Dispatch simulator.** One tick every 45 ms stands for one simulated second.

| Strategy | Per tick | Retries | Ends |
|---|---|---|---|
| Blast | sends everything pending; the server accepts up to its capacity | immediately | after three rounds; the rest are lost |
| Paced | sends 90% of capacity from a queue | re-queued | after five attempts per message |

Random failures apply to both. The verdict compares delivered percentages and times.

**Fig. 2 · Payslip.** `basic = regular × rate`, `overtime = overtime hours × rate × multiplier`,
`gross = basic + overtime + bonus`, `tax = gross × rate%` (capped at 60%), `net = gross − tax −
deductions`. Negative or empty inputs count as zero.

**Fig. 2 · Receipt PDF.** jsPDF draws an 80 mm till roll in Courier, marked as sample data. It is drawn
twice: once on a tall page to measure its height, then on a page cut to fit. Saved as
`payslip-<name>-<YYYY-MM>.pdf`. Nothing leaves the browser. jsPDF (`assets/vendor/jspdf.umd.min.js`)
is fetched on the first hover, focus or click of the button; if it cannot load, the button falls
back to `window.print()`.

**Fig. 5 · The same form, twice.** "Before" has cryptic labels, seven equal fields, three equal
buttons and one useless error. "After" has plain labels with examples, errors beside the field on
blur, fewer fields, one action, autofill hints, and a Bangladeshi mobile number check
(`01[3-9]` plus eight digits, optional `+88`).

### D.06 · 3D figures

**Stage.** One factory sets up each figure: renderer, a camera orbiting a target, HTML labels pinned to
3D points, picking, and input.

```
Stage({ host, canvas, tags, yaw, pitch, target, fit, view(aspect) })  →  S
S.scene  S.camera  S.glows  S.pick  S.tag(html, point, cls)  S.resetView()  S.start()
S.onFrame(dt, t)  S.onPick(object)  S.onSeen()  S.onTheme(palette)
```

- `fit` is the half width and half height that must stay in view; `view(aspect)` swaps camera
  settings between a wide and a tall framing.
- Drag turns (yaw within ±0.9 rad, pitch 0.3 – 1.3); right-drag, Shift-drag or any drag while zoomed
  pans; pinch, Ctrl + wheel, the corner keys or `+` `−` zoom between 0.8× and 4.5× about the point
  under the cursor; `0` or a double tap resets. A plain wheel still scrolls the page.
- After six idle seconds the view drifts back to its resting angle.
- The loop pauses off screen. While a sheet covers the page (`sheetchange`), the figure keeps its clock
  running but draws nothing.
- Shared pieces: `Glow` (soft accent points sized in world units), `Path` (a polyline walked at
  constant speed), `arc` (a lifted curve between two points), `Mats` (line and fill materials
  recoloured with the theme).

**Fig. 3 · Restaurant floor.** A menu wall of 20 dishes in 5 categories, and six tables (2, 4, 6, 4,
8 and 2 seats; round and rectangular) each with chairs, place settings and a QR code.

- *Sort the menu*: cards fly into category columns on a spring with a swirling flow field that fades
  on arrival. Switching a category off removes it from every table's menu at once.
- *Scan a table*: a beam and a ring rise from its QR code and the browser tab opens that table's
  menu (`menu.example/t/N`).
- *Order*: each dish flies to a place setting in front of a chair, drinks beside it; shared dishes go
  to the middle. Staff tickets group orders by table; *Close bill* frees the table.
- *Simulate a guest*: sorts the menu if needed, picks a free table and orders for a party that fits.

**Fig. 4 · Checkout to the doorstep.** Storefront → one of four gateways → a warehouse bin → packing →
van → one of four homes. One order is one object the whole way, so it can be followed.

| Gateway | Weight |
|---|---|
| Card | 0.40 |
| Mobile wallet | 0.35 |
| Bank transfer | 0.10 |
| Cash on delivery | 0.15 |

- Stock is reserved when the order is placed; a bin at two or fewer triggers a restock.
- A failing gateway moves the order to another; an idempotency key means one charge however many
  times a gateway calls back, and duplicate callbacks are counted as ignored.
- The van loads at the depot, visits the nearest door first and returns.
- The tracker follows your own order, otherwise the next one once the last has arrived. Steps:
  Placed, Paid, Picked, Packed, Out for delivery, Delivered.
- Six products, orders per minute 6 – 90, gateway failure 0 – 40%, gateways switchable.

### D.07 – D.08 · Photo guard & control drawers

The cover and glance photos get no context menu and no drag. Each figure's settings fold into a
`<details class="dock">` whose summary shows the current values in one line. Drawers open from
701px and close below, until someone opens or closes one themselves. The chip, the Fig. 3 admin and
the Fig. 4 admin drawers always start closed.

### D.09 · Pinned chip drawer

When the chip is pinned (≥ 1100px wide and ≥ 680px tall) its drawer opens down over the chip; the chip
moves down and, if it has to, shrinks to as little as 60% to stay in sight. Escape or a click outside
closes it.

### D.10 · Compact figures (≤ 900px)

**Browser tabs → sheets.** The tabs of Figs. 3 and 4 fold into a launcher (a small browser icon with
a cursor that taps it when the address changes). Opening it raises the same tab as a sheet over a
dimmed page.

- It is a full-screen layer, a manual popover in the top layer where supported, not a modal
  `<dialog>`. The dialog's manners are done by hand: focus moves in and is kept there, Escape and
  Android's back gesture (`CloseWatcher`) close it, focus returns to the launcher.
- The sheet grows out of the launcher; only `transform` and `opacity` animate.
- Drag the handle or the address bar down to put it away; the dimming lifts as it goes.
- The page under it does not scroll (`html.sheet-on`), and the 3D figures stop drawing.
- Opened before a table is picked, Fig. 3's sheet offers the tables inside, like a QR landing page.
  Placing an order closes it so the order can be seen landing.

**Staff tickets in a row.** Fig. 3's tickets become one sideways row of short cards. Opening a card
stacks the cards to its right under one another, as many as its height leaves room for, and they
spring into place one after another. A new table slides in from the right.

---

## 6 · Contracts between the layers

| Name | Kind | Set by | Read by |
|---|---|---|---|
| `html.js` | class | `A.09` | `B.22` (hide `[data-fly]` until placed) |
| `html.fly-ready` | class | `D.01` | `B.22` |
| `html.sheet-on` | class | `D.10` | `B.17`, `B.18` (lock page scroll, pause the tracker pulse) |
| `html[data-theme]` | attribute | `D.03` | `B.02`, `B.06`, `B.13`, `B.19`, the 3D colour readers |
| `localStorage.theme` | storage | `D.03` | `D.03` |
| `sheetchange` | `CustomEvent` on `document`, `detail.open` | `D.10` | `D.06` Stage |
| `--surf` | CSS variable | `B.02`, containers | every soft control |
| `#flTickets` contents | markup | `D.06` Fig. 3 rewrites it per order | `D.10` re-lays the row |
| `.tab`, `.tab-body`, `.url` | markup | `C.09`, `C.10` | `D.10` moves the tab into a sheet |
| `.chip-ctrls`, `.ctrls`, `.fields`, `#flCats`, `#shGws` | markup | `C` | `D.08` moves them into drawers |

Element IDs the scripts depend on:

| Area | IDs |
|---|---|
| Bar, footer | `themeBtn`, `tcLight`, `tcDark`, `yr`, `ghLink`, `ghSoon` |
| Chip | `orbit`, `chipFig`, `chipHost`, `chipCanvas`, `chipUi`, `chipStatus`, `chipReset`, `rx`, `ry`, `rz`, `sup`, `oRx`, `oRy`, `oRz`, `oSup` |
| Fig. 1 | `total`, `cap`, `fail`, `oTotal`, `oCap`, `oFail`, `runBtn`, `resetBtn`, `nd`, `nf`, `ndn`, `nfn`, `nt`, `pd`, `pf`, `pdn`, `pfn`, `pt`, `verdict` |
| Fig. 2 | `pName`, `pCur`, `pReg`, `pOt`, `pRate`, `pMult`, `pBonus`, `pTax`, `pDed`, `slip`, `printBtn` |
| Fig. 3 | `flHost`, `flCanvas`, `flTags`, `flSort`, `flGuest`, `flReset`, `flCats`, `flTables`, `flTab`, `flUrl`, `flMenu`, `flCart`, `flOrder`, `flTkCount`, `flTkScroll`, `flTickets`, `flStatus` |
| Fig. 4 | `shHost`, `shCanvas`, `shTags`, `shTab`, `shShop`, `shTotal`, `shPay`, `shSteps`, `shStepNow`, `shLog`, `shRate`, `oRate`, `shFail`, `oGfail`, `shGws`, `shRun`, `shReset`, `sOrd`, `sPaid`, `sRe`, `sDup`, `sTwice`, `sShip`, `shOrders`, `shStatus` |
| Fig. 5 | `uxBefore`, `uxAfter`, `uxHost`, `uxWhy` |

---

## 7 · Decisions worth knowing before you change anything

- **The top bar is 99% opaque, not 100%.** When it counts as fully opaque, Chrome stops drawing the
  part of the pinned WebGL chip it thinks is behind the bar, and the chip vanishes early as the orbit
  ends. (`B.06`)
- **Canvases never trap a scrolling finger.** On touch screens, up and down scrolls the page and
  sideways turns the figure (`touch-action: pan-y`). Only when a 3D figure is zoomed in does a
  one-finger drag pan it instead.
- **Small accent text uses `--accent-ink`,** a deeper leaf, so it stays readable on the grain. The
  lighter leaf stays on marks, the chip's glow and big numerals. (`B.23`)
- **The photo slot's tilt lives on the inner card** (`.snap-in`), so the arrival motion on `.snap` can
  never undo it. (`B.13`)
- **The sheet is a popover, not a `<dialog>`.** Opening a modal dialog makes the rest of the page
  inert, and the browser restyles every element to do it, on the way in and out. (`D.10`)
- **The Fig. 4 checkout tab is built once and updated in place,** so keyboard focus is never lost.
- **Resizing clears a WebGL canvas.** While a sheet covers a figure its loop does not draw, so the
  Stage redraws once on resize. (`D.06`)
- **Theme colours reach WebGL by probing CSS.** A hidden span takes `color: var(--token)` and a 1×1
  canvas converts whatever the browser returns into RGB; the key of the current tokens is checked
  every 20 frames.
- **Randomness.** The chip's beads, the 3D figures and the dispatch race use `Math.random`; arrivals
  do not, so the page's motion is the same on every visit.
- **Everything works without motion.** `prefers-reduced-motion` turns off arrivals, smooth scroll,
  the theme bubble, card flights, idle drift and the launcher hint, and starts Fig. 4 paused.

---

## 8 · Common edits

**Add a real photo to a slot.** Put the image next to `index.html`, then inside the slot's
`figure.snap-in` replace `<span class="snap-ph" aria-hidden="true"></span>` with, for example,
`<img src="desk.jpg" alt="My desk in Dhaka" width="800" height="1000" loading="lazy">`, and change
the `figcaption`. Slots: the desk in `C.06`, sketches in `C.12`, a process shot in `C.14`.

**Change the GitHub link.** `GITHUB_USER` at the top of `D.03`. An empty string hides the link and
shows "GitHub · Coming soon".

**Change the accent.** `--brand` and `--accent-ink` in `B.02`, the dark `--accent` twice in `B.02`,
and the chip's starting colour (`0x546634`) in `D.04`, which the live colour read replaces on the
first frame.

**Add a feature.** Copy a `C.07`-style section with a new `id` (`f8`, heading `f8h`), add a link to
`#inside` in `C.04`, give it a top margin and a type scale in `B.12 › spacing and type scale`.
Without a wide composition it uses the shared phone and tablet layout; add one inside
`B.12 › ≥ 1100px` if it deserves its own.

**Make something arrive.** Add `data-fly` (or `data-fly="rise"`, `data-fly=".5"`,
`data-fly-delay=".1"`). Not inside another `data-fly` element.

**Add a dark-mode rule.** Write it for `:root[data-theme="dark"]` and again inside
`@media (prefers-color-scheme: dark)` for `:root:not([data-theme="light"])`.

**Update a vendor library.** Replace `assets/vendor/three.min.js` or `assets/vendor/jspdf.umd.min.js`
with the official minified build (both are byte-identical to npm `three@0.149.0` and
`jspdf@2.5.1` today). The own code uses
three.js r149 APIs (`BufferGeometry`, `EdgesGeometry`, `ShaderMaterial`, `Raycaster`) and jsPDF's
`window.jspdf.jsPDF`.

---

## 9 · Other files in the repository

| File | Purpose |
|---|---|
| `assets/fonts/*.woff2` | the nine typefaces of `B.01` |
| `assets/portrait.webp` | the 1500 × 900 cover photo behind `--me` |
| `assets/vendor/` | three.js r149 and jsPDF 2.5.1, untouched official builds |
| `arman-afridi-rahman.jpg` | portrait referenced by the JSON-LD `Person.image` and the sitemap |
| `og-card.jpg` | 1200 × 630 social card for Open Graph and X |
| `og-image.png` | an earlier social image, not referenced by the page |
| `favicon.svg`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | icons and the mask icon |
| `site.webmanifest` | installable app metadata, paper-coloured |
| `robots.txt`, `sitemap.xml` | crawling: allow all, one URL with two images |
