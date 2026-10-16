# Folio · Build rules

> Companion to **`A-design-language.md`** (File A). File A says *what* Folio looks like and how each
> part works. This file says *how you work*: what to ask, in what order to build, what you may change,
> what you may never do, and how you prove the result is right before you hand it over.
> Read this file completely before you read File A.

---

## Contents

0. [Precedence and vocabulary](#0--precedence-and-vocabulary)
1. [The job](#1--the-job)
2. [The working loop](#2--the-working-loop)
3. [Phase 1 · The interview](#3--phase-1--the-interview)
4. [Phase 2 · The content plan](#4--phase-2--the-content-plan)
5. [Phase 3 · The build order](#5--phase-3--the-build-order)
6. [Non-negotiables](#6--non-negotiables)
7. [Forbidden](#7--forbidden)
8. [What you may adapt](#8--what-you-may-adapt)
9. [Decision tables](#9--decision-tables)
10. [Writing the words](#10--writing-the-words)
11. [Fallback ladders](#11--fallback-ladders)
12. [Working in small steps](#12--working-in-small-steps)
13. [Phase 4 · Proof before delivery](#13--phase-4--proof-before-delivery)
14. [Delivery](#14--delivery)

---

## 0 · Precedence and vocabulary

| Situation | Rule |
|---|---|
| File A gives a value (colour, size, clamp, duration, easing, constant) | Use it **verbatim**. Do not round, "tidy" or modernise it. |
| File A gives no value for something you need | Reuse the nearest value File A already uses for a similar job (same role, same surface). Never introduce a new colour, font, radius, shadow or easing. |
| File A and this file disagree about **process or scope** | This file wins. |
| File A and this file disagree about a **value or a component's anatomy** | File A wins. |
| The person asks for something that breaks a rule in §6 or §7 | Explain in one sentence which rule it breaks and why the rule exists, offer the closest Folio-compatible alternative, and do what they decide. Their site, their call; record the deviation in the delivery note (§14). |
| You are unsure whether something is allowed | Choose the more restrained option. Folio is built by leaving things out. |

Words: **MUST / MUST NOT / SHOULD / MAY** carry their RFC 2119 meanings. "§A4.2" means File A,
section 4.2. "§B6" means this file, section 6. `{{key}}` is a field of the content model (§A3).
"The person" is the one whose portfolio you are building.

---

## 1 · The job

Build a one-page portfolio for the person, in the Folio design language, delivered as one
`index.html` plus an `assets/` folder, with no framework, no build step and no third-party requests at
runtime. The page reads like an issue of a small printed magazine about the person's work: a cover
with their name set across their photo, an opening line, a contents list that travels past a
hand-turnable 3D object, a one-card summary for the thirty-second reader, a letter, three to seven
feature articles with working figures, a "next issue" card, a contact block and a colophon.

Success is judged on four things, in this order:

1. **Truth.** Every claim on the page came from the person. Every simulated figure says it is simulated.
2. **Fidelity.** Someone who knows Folio recognises it at a glance, in both themes, at every width.
3. **Craft.** Accessible, fast, readable without JavaScript or motion, no layout shift, no console errors.
4. **Fit.** The page sounds like the person and shows *their* work at its most convincing.

---

## 2 · The working loop

Work in five phases. Do not skip a phase and do not start one before the previous gate is passed.

| Phase | You do | Gate to pass before moving on |
|---|---|---|
| **0 · Read** | Read this file, then File A §0–§6 and Appendix A completely. Skim §7–§18 headings so you know where things are. | You can state, without looking, the five typefaces and their jobs, the paper and ink colours, what `--surf` does, and the section order of the page. |
| **1 · Interview** | Ask the questions in §B3, in rounds. | You have the minimum set (§B3.4) or the person has said "use what you have". |
| **2 · Plan** | Fill the content model (§A3), choose layouts, figures and the signature object (§B9), draft all copy (§B10). Show the plan. | The person has confirmed the plan, or corrected it and confirmed the correction. |
| **3 · Build** | Build in the fixed order of §B5, one step at a time, running each step's check. | Every step's check passes. |
| **4 · Prove** | Run the acceptance tests (§A18 and §B13). Fix and re-run until all pass. | All tests pass, or each remaining failure is listed with its reason in the delivery note. |

If you can run code and open a browser, open the page after every build step at 1440px and 390px wide,
in light and dark. If you cannot, re-read the step's part of File A against your code line by line
instead. Never claim a check passed if you did not perform it.

---

## 3 · Phase 1 · The interview

### 3.1 How to ask

- Ask in **three rounds**. At most **seven questions per message**. Number them so answers can be short.
- Say why you are asking when it is not obvious ("so the name fits on one line at every width").
- Offer a **default** for every optional question, so "default" is a valid answer.
- Accept partial answers and follow up only on what is missing.
- Never ask for anything the page will not use.
- If the person uploads a CV, a LinkedIn export or an old site, read it first and ask only for what is
  missing or unclear. Confirm facts you extracted instead of re-asking them.

### 3.2 The three rounds

**Round 1 · You**

1. Your name exactly as it should be printed on the cover. Should one word be in italics? (default: the
   second word of three, or none for a two-word name)
2. What you do, in two to four words (for example "Product designer", "Backend engineer").
3. Where you are based. May the cover show the city and coordinates rounded to one decimal? (default:
   city only)
4. How you work: remote, on-site, hybrid, open to relocation.
5. The e-mail address for the contact block, and up to three links (for example LinkedIn, GitHub, Dribbble).
6. A landscape photo of you, at least 1500 × 900 px, with you off-centre and plenty of surroundings.
   (No photo is fine: the cover then uses the variant in §A7.)
7. One sentence on what you make and why it is good. I will tighten it with you.

**Round 2 · The work**

8. List **three to seven** pieces of work you want to show: products, client projects, roles, research,
   open source, or significant coursework. For each one, tell me:
   a. its field in one or two words; b. what it does, in a sentence; c. the hardest constraint you
   worked under; d. what *you* did; e. one result with a number if you have one; f. the tools;
   g. whether a visitor could **try** a small piece of it in the browser: a simulation, a calculator,
   a before/after comparison, or a 3D model of how something flows.
9. Which of them is the one you most want a stranger to see? (It becomes Feature 01.)

**Round 3 · The rest**

10. Six short facts for the summary card: for example years of experience, core tools, what else you
    do, your specialty, how you work, the languages you speak. (default: I draft them from your answers)
11. Your tools in up to three groups; the third group is what you are learning or building with now.
12. For the letter: why do you do this work, what do you care about that others skip, and what should
    a reader expect from you? Two or three sentences each are enough; I will draft and you edit.
13. What are you working on next? (It becomes the "next issue" card. default: "Open to new work" variant.)
14. Accent colour: sampled from your photo (default), or a colour you prefer?
15. The 3D object in the orbit represents your craft. I suggest **[the archetype you picked from the §A15 table, by §B9.3]**. Keep it or choose another?

### 3.3 People with little work to show

Never invent work. If the person has fewer than three pieces:

- Use what exists: coursework, a hackathon, a volunteer or internal tool, a process they improved at work,
  a written teardown or redesign of an existing product (labelled as a self-initiated study).
- Build **at least three** features. Two strong features plus one study is better than four thin ones.
- If even that is not possible, build three features anyway from the strongest material and tell the
  person which ones are thin and what would make them stronger.

### 3.4 The minimum set

You may start Phase 2 once you have: the name, the role, the e-mail, and **at least three** pieces of
work with a–f answered. Everything else has a default or a documented variant. If the person says
"just build it" before that, build with the minimum set and list every default you used in the plan.

---

## 4 · Phase 2 · The content plan

Before writing any code, present the plan as one message with these parts, in this order:

1. **The content model** (§A3), filled in, as JSON or a table. Unknown optional fields are `null` and
   say which variant applies.
2. **The headlines**: every headline with its one italic hinge word marked, its ink word (if any) and
   brush word (if any) marked (§A5, §B10.2).
3. **The features table**: number, field, headline, layout template (L1–L7, §A10), figure archetype
   (A0–A5, §A12) with a one-line description of what the visitor can do, and the honesty note.
4. **The signature object**: archetype, what the beads represent, the physical rule, and the status
   sentence (§A15).
5. **The accent**: the two hex values (light, dark) and where they came from (§A4).
6. **Defaults used** and **questions left**.

If you were told the plan is already confirmed (for example, answers handed to you in advance), skip the wait, keep the inferred-copy list in the delivery note instead of `[check]` marks, and build.

Otherwise ask the person to confirm or correct. Apply every correction, then show only what changed. Start Phase 3
only after a clear confirmation.

---

## 5 · Phase 3 · The build order

Build in this order. Each step lists its File A sections and the check you run before the next step.
Do not reorder: later steps depend on earlier tokens, grid and contracts.

| # | Step | File A | Check before moving on |
|---|---|---|---|
| 1 | File anatomy: masthead comment, the four layers with banners, empty parts | §2, App. B | The file opens without errors; banners are in order A → B → C → D. |
| 2 | Head: identity, colour metas, icons, social cards, JSON-LD, preloads, the head flag script | §2 | View source: every `{{…}}` in the head is filled; the flag script sets `html.js` and the saved theme. |
| 3 | Fonts and tokens, pasted from Appendix A; accent replaced with the derived values | §4, §5, App. A | Body is `--paper` with grain; text is `--ink`; switching `data-theme` re-inks everything. |
| 4 | Document base, labels and marks, the `.g` grid | §5, §6 | At 1440 and 390, a test element on `col 3 / col-end` lands where §A6 says. |
| 5 | Bar and theme switch, including the circular reveal | §7 | Toggle works with mouse and keyboard, persists across reload, no flash of the wrong theme. |
| 6 | Cover with the cut and the echo; K re-derived for this name | §7 | At 300, 390, 820, 1440 and 2560px the name is on one line, cut by the photo edge, echo aligned. |
| 7 | Opening line | §7 | Two lines at ≥ 1100px; poster size on phones. |
| 8 | At a glance card | §9 | Bleeds right; facts reflow 3 → 2 → 1; "now" crescents visible. |
| 9 | Letter | §9 | Drop cap three lines tall; two columns only at ≥ 1100px. |
| 10 | Features, one at a time, text parts first (no figures yet) | §10 | Each feature matches its layout template at ≥ 1100px and the shared layouts below. |
| 11 | Next issue, contact, footer | §11 | Inverse card re-lights its controls; e-mail never overflows at 300px. |
| 12 | Arrivals | §16 | Scroll down and up: things land once and stay; reduced motion shows everything still. |
| 13 | Figure trays and controls, then each figure's behaviour | §12, §13 | Every control works by keyboard; drawers follow the width rule; readouts never shift layout. |
| 14 | 3D stages, then the 3D figures | §14 | Plain wheel scrolls the page; drag turns; loops stop off screen. |
| 15 | Orbit and the signature object | §8, §15 | The object pins at ≥ 1100 × 680, turns with scroll until touched, words pass in the lanes. |
| 16 | Compact figures (≤ 900px): launchers, sheets, ticket rows | §13 | Sheet traps focus, Escape closes it, focus returns, page does not scroll under it. |
| 17 | Dark mode pass, print pass | §17 | Every surface, mark and 3D colour correct in dark; print preview is ink on paper. |
| 18 | Accessibility and performance pass | §17 | §B13 all green. |

**Text before behaviour.** For every section, build the markup and the CSS first and check the layout with
static content. Add JavaScript last. A section must look right with JavaScript off.

---

## 6 · Non-negotiables

Each rule is checkable. Numbers in brackets point to the part of File A that defines it.

### 6.1 Paper, ink and light

- **R1** Paste the token sheet (App. A) verbatim. Only the accent tokens change, and only to the values
  derived by the method in §A4.
- **R2** No `#fff`, `#000`, `white` or `black` as a surface or text colour anywhere. The page is `--paper`
  with its grain; text is `--ink` or `--muted`. (Exceptions are the literal values File A itself uses
  inside `color-mix()` recipes, shade gradients, print colours, and the deliberately bad "before" form of archetype A3, §A12.)
- **R3** One accent. It appears **only** where §A4 lists it. Anything that is "on" or chosen fills with
  `--ink`, not with the accent.
- **R4** Small text in the accent colour uses `--accent-ink`, never `--accent`.
- **R5** Sand appears only in hand marks, "now" crescents, dot separators, hanging quote marks and emphasis
  underlines (§A4, §A5).
- **R6** Only controls are moulded. Plates, trays, cards, photos, text and drawings are flat: no
  `box-shadow` on them. The exceptions are exactly the ones File A specifies: taped prints keep their print
  shadow (§A10), stack pills are raised (§A9) and a figure's result card is raised (§A12). The bar has no shadow.
- **R7** Soft shadows come only from the `--surf` light model. To put controls on a new surface, set
  `--surf` (and, for the inverse card, `--hiMix` / `--loMix`) on the container. Never hand-write a
  shadow colour.
- **R8** Every dark-mode rule exists twice: under `:root[data-theme="dark"]` and inside
  `@media (prefers-color-scheme: dark)` under `:root:not([data-theme="light"])`.

### 6.2 Type

- **R9** Exactly five families, each in its job (§A5): Instrument Serif for headlines and numerals, Jost
  300 for reading text, DM Mono for notes, captions, labels and details, Nunito for everything you press,
  Caveat once on the whole page. `font-synthesis: none`.
- **R10** Headlines are Instrument Serif 400. Never bold. Weight is faked only with `.ink` (§A5).
- **R11** Each headline has **at most one** italic run, the hinge word or phrase (§A5, §B10.2).
  Pull quotes invert this: the quote is italic and one word is upright.
- **R12** Reading text is never bolder than 400. Emphasis in running text is the sand underline or an ink
  colour shift (§A5).
- **R13** Type sizes, line heights and tracking come from the §A5 scale. Headline tracking is set by
  hand per word with the tracking classes, then re-checked at 1100, 1440 and 1920px.

### 6.3 Layout

- **R14** Every section below the orbit is a `.g` grid; parts are placed with named lines only (§A6).
- **R15** Bleeds alternate down the page: the glance card bleeds right, one feature's tray bleeds left,
  the next-issue card bleeds right. The bleeding side has square corners; the content inside returns to
  the frame (§A6).
- **R16** Section spacing uses the long/short pair rhythm and the clamps in §A6. There is no uniform
  spacing scale.
- **R17** Text is left-aligned and ragged-right. No centred sections, no centred headlines, no justified
  text.
- **R18** The cover name stays on one line from 300px to 2560px, using the §A7 formula with K
  re-derived for this name.
- **R19** No horizontal scrolling at any width from 300px to 2560px.

### 6.4 Sections and components

- **R20** Sections appear in the §A1 order with the ids in §A1.2 and Appendix B. Sections may be omitted only as §B8
  allows; they are never reordered.
- **R21** Every feature uses the shared anatomy (§A10) and one layout template L1–L7. Two consecutive
  features never use the same template.
- **R22** Every interactive figure sits in a tray with the `Fig. N — try it` header line and a frank
  note saying what is simulated and what is real (§A12).
- **R23** Every control is a Folio control from §A12. No browser-default selects, ranges or checkboxes
  are visible.
- **R24** A figure's settings live in a `.dock` drawer whose closed state shows the current values
  in one line (§A12).
- **R25** Photo slots are taped prints. An empty slot shows the hatching placeholder and an honest caption.

### 6.5 Motion

- **R26** Scroll arrivals use the `data-fly` system of §A16 exactly: deterministic springs, forward-only,
  settle on scroll stop.
- **R27** Animate `transform` and `opacity` only, except where §A16's inventory names another property.
- **R28** Every animation has the reduced-motion behaviour listed in §A16. With
  `prefers-reduced-motion: reduce`, nothing translates or rotates on its own.
- **R29** With JavaScript disabled, every section is visible and readable (the 2.5s CSS fallback).
- **R30** Every `requestAnimationFrame` loop stops when its element is off screen and when nothing moves.

### 6.6 3D

- **R31** 3D is drafting, not rendering: hairline edges, no lights, no shading, no textures except tiny
  canvas labels, no shadows, ink lines, the accent only on live parts, glow as one `Points` cloud
  with normal blending (§A14, §A15).
- **R32** Canvases never trap scrolling: a plain wheel scrolls the page, `touch-action: pan-y`, zoom only
  by pinch, Ctrl/Cmd + wheel, keys or the zoom keys (§A14).
- **R33** Device pixel ratio is capped at 2. Without WebGL, a sentence replaces the canvas and the page
  stays whole.

### 6.7 Access

- **R34** Everything is operable by keyboard. The focus ring is the §A6 ring and appears only for keyboard focus.
- **R35** Every control has an accessible name. Decorative duplicates and marks are `aria-hidden="true"`.
- **R36** Live regions are `polite`, reserve their height and do not chatter (§A17).
- **R37** All text meets WCAG AA contrast in both themes. Colour is never the only signal.
- **R38** Touch targets are at least 44 × 44 px.

### 6.8 Code and delivery

- **R39** One `index.html` and an `assets/` folder. No framework, no build tool, no CSS framework, no icon
  font, no CDN or third-party request at runtime, no analytics.
- **R40** The file follows §A2's layer and banner grammar. One IIFE per script, nothing global.
- **R41** Fonts and three.js are self-hosted in `assets/`. Heavy libraries (PDF) load on demand.
- **R42** The portrait and the first-screen fonts are preloaded exactly as §A2 shows.

---

## 7 · Forbidden

If you notice yourself reaching for one of these, stop and re-read §A1.

- **F1** Gradient fills, gradient text, mesh or "aurora" backgrounds. The only gradients are the ones File A writes out: the photo shades and fades, and the small CSS drawings made with gradients (theme-switch dot, slider dot, switch knobs, select chevron, drawer glyph, "now" crescent, print hatching).
- **F2** Glassmorphism, `backdrop-filter`, frosted panels, translucent overlays over content.
- **F3** Neon, glows, bloom, coloured shadows (the 3D glow beads are the one exception, §A14).
- **F4** Drop shadows on cards, sections, images or text. Neumorphic cards or panels.
- **F5** Pure white or pure black surfaces or text; grey-on-white "clean minimal" palettes.
- **F6** A second accent colour, colour-coded sections, or tinted section backgrounds.
- **F7** Bold or black-weight headings, sans-serif headlines, all-caps headlines. (Small tracked uppercase labels are fine.)
- **F8** Any typeface outside the five, any icon font or icon library, emoji in the page.
- **F9** Centred hero layouts, big "Hi, I'm …" greetings, card grids of projects, logo walls, skill
  percentage bars, star ratings, testimonial carousels, "Hire me" badges, sticky call-to-action buttons.
- **F10** Hover effects that scale, lift or tilt cards and images (the photo-slot straighten in §A10 is the one exception).
- **F11** Parallax backgrounds, scroll-jacking, smooth-scroll libraries, cursor followers, magnetic
  buttons, typewriter text, marquees, text scramble, particle fields, confetti, Lottie, page-load spinners.
- **F12** Red error text (outside the A3 "before" form, which is bad on purpose), toast pop-ups, modal pop-ups on load, cookie banners, chat widgets, newsletter prompts.
- **F13** Lorem ipsum, invented metrics, invented clients, invented testimonials, stock photos, AI-generated portraits.
- **F14** React, Vue, Svelte, jQuery, Tailwind, Bootstrap, GSAP, any npm build. Google Fonts or any CDN at runtime.
- **F15** Dark mode made with `filter: invert()`, or dark mode that dims photos.
- **F16** `Math.random()` in page motion (arrivals, layout). Randomness belongs only inside simulations.
- **F17** Animating `width`, `height`, `top`, `left`, `margin` or `box-shadow` for movement (the documented
  exceptions in §A16 aside).
- **F18** Hiding content behind JavaScript: no section may depend on a script to become visible, beyond the 2.5s fallback.

---

## 8 · What you may adapt

Everything not listed here stays as File A specifies.

| You may change | Within these bounds |
|---|---|
| Accent hue | Derived by §A4; one hue, two tones per theme. |
| Number of features | 3 to 7 (§A10.8). |
| Which features have figures | At least one figure on the page; at most one figure per feature; a feature without a figure uses archetype A0 (§A10, §A12). |
| Figure archetypes | From A0–A5 only (§A12), chosen by §B9.2. |
| Signature object | From the §A15 archetypes, or a new one that keeps every §A15 invariant. |
| Cover corner captions | Role on the left; place, with or without coordinates, on the right. |
| Bar links | Up to five section links; on phones only the glance link and the toggle remain (§A7). |
| Letter | 2 to 5 paragraphs; one stamped word and one ringed word. |
| Photo slots | 0 to 3 taped prints, in the positions §A10 / §A11 allow. |
| Next issue | The person's next project, or the "open to new work" variant (§A3). |
| Stack groups | 1 to 3 groups; the "now" crescent only on the last group. |
| Words in labels and notes | Any wording that follows §B10 and fits the space File A gives. |

Sections that may be **omitted**: the letter (if the person declines), photo slots, the next-issue card
(replace with nothing rather than filler). Sections that may **never** be omitted: bar, cover, opening
line, orbit (falls back per §B11 but keeps its contents list), glance card, at least three features, contact, footer.

---

## 9 · Decision tables

### 9.1 Layout template for each feature

Use the rules and the lookup table in §A10.8. Count N (features) and k (features without a figure),
read the sequence from the table, and reorder features 02…N if the table needs it; feature 01 never moves.
Then check:

- no two adjacent features share a template, and each template is used at most once;
- details tables never sit on the same side three times in a row (§A10.8 rule 6);
- whenever a feature after 01 has a figure, L4 is present and its tray is the page's one left bleed (§B R15);
- never two neighbours that have neither a figure nor a quote.

### 9.2 Figure archetype for each piece of work

Ask "what would a visitor understand in thirty seconds by touching it?" and pick the first row that fits.

| The work is mainly about… | Archetype (§A12) |
|---|---|
| Choosing between two strategies under a limit (throughput, cost, latency, risk) | **A1 · Race** |
| Turning inputs into a document, a quote, an estimate or a report | **A2 · Calculator → document** |
| Design quality, usability, clarity, accessibility, a redesign | **A3 · Before / after** |
| A place, a layout, a physical or spatial system people move through and act in | **A4 · 3D scene + browser tab** |
| A flow with stages, hand-offs, failures and retries | **A5 · 3D flow + tracker** |
| Writing, research, strategy, leadership, or anything better told than touched | **A0 · No figure** |

Limits: at most **two** 3D figures on the page (each costs a WebGL context and the visitor's attention);
the signature object does not count toward the limit. If two features want the same archetype, the
weaker one becomes A0. Every figure models the *idea* of the work with invented, labelled data, never
the person's confidential data.

### 9.3 Signature object for the person's craft

Use the §A15 archetype table. Match on the person's role first, their strongest work second. If nothing
fits, keep the default archetype from §A15 and say why in the plan. The beads always mean something true
about the craft (flow, attention, load, signal), and the status sentence says it in plain words.

### 9.4 When content is missing

| Missing | Do this |
|---|---|
| Portrait | Cover variant without photo (§A7), glance card without its band (§A9), as §A3.7 lists. Do not use a stock photo or an illustration. |
| Accent preference and portrait | Use File A's default accent pair. |
| Numbers for results | Write the result without a number. Never estimate one for the person. |
| Letter material | Ask once more with the three prompts in §B3.2 Q12; if still nothing, omit the letter. |
| Next project | "Open to new work" variant (§A3). |
| Facts (fewer than six) | Ask for the missing ones with suggestions; if still short, use 4 or 2 facts (§A3.7), never filler facts. |
| Links | Show the e-mail only. |

---

## 10 · Writing the words

### 10.1 Rules

- Write in the person's first person, in plain short sentences. Present tense for what they do, past for what they did.
- Every sentence carries information. No "passionate", "innovative", "cutting-edge", "seamless",
  "leverage", "synergy", "world-class", "rockstar", "ninja", no exclamation marks, no emoji.
- Prefer concrete nouns and numbers the person gave you. If a sentence would need a number you do not
  have, rewrite the sentence.
- Buttons are verbs ("Run both", "Reset", "Download PDF"), never "Click here" or "Learn more".
- Figure notes are frank: "simulated, not real traffic", "made-up data", "the download is real".
- Keep every text inside the length limits of §A3. If it does not fit, cut words; never shrink type.
- Never invent facts. Draft from the person's answers, mark anything you inferred with `[check]` in the
  plan, and remove the marks only after they confirm.

### 10.2 Headline grammar

- 2 to 6 words. One **hinge** word or short phrase in italics: the word that turns the meaning ("Shipping
  *under* pressure", "Data that *stays* honest"). Never italicise the first word unless it is the hinge.
- At most one `.ink` word per headline, on the noun that carries the weight. At most one brush mark on the
  page's opening line and one on the glance name (§A5 rarity budget).
- Feature top lines carry a spoken aside in italic serif: a half-sentence the person might say out loud
  about the work ("the one that had to work offline").
- Pull quotes are one sentence the person would stand behind, 4 to 12 words, with one upright word.

---

## 11 · Fallback ladders

When a part is beyond what you can build reliably, step down one rung at a time. Never skip to removal.
Every rung keeps the layout, the tokens and the reduced-motion behaviour intact. Tell the person which
rung you used.

| Part | Rung 1 (full) | Rung 2 | Rung 3 | Rung 4 |
|---|---|---|---|---|
| Signature object (§A15) | Full archetype with bead physics, scroll turn, controls | Geometry + beads on fixed speeds, drag only, no physics | Static hairline drawing (SVG in the same line weights), no beads | Orbit keeps lanes and contents; the stage shows the `fig. 0` caption and an empty well |
| 3D figure (§A14) | Stage + scene + side panel + sheet | Stage + scene, simplified actions | 2D SVG diagram in the tray with the same controls | Archetype A0 for that feature |
| Arrivals (§A16) | Full spring system, three modes | Rise mode only, at every width | CSS-only fade-in on scroll via `IntersectionObserver` class toggle, same timings | None; content is simply there |
| Compact sheet (§A13) | Popover sheet with drag-to-dismiss | Sheet without drag (× and Escape only) | Tab stays inline at ≤ 900px, scrolls in its tray | — |
| Theme reveal (§A7) | Circular View Transition | Instant swap | — | — |
| Custom dropdown (§A12) | Full combobox | Native `<select>` styled as a pill field with the §A12 chevron | — | — |

---

## 12 · Working in small steps

These rules exist because long single passes drift. Follow them even if you feel confident.

1. **One section per pass.** Write the section's HTML, then its CSS, then check it, then move on.
   Do not write the whole page in one go.
2. **Copy, then fill.** Start every section from its skeleton in Appendix B and its CSS in the section's
   chapter. Replace placeholders; do not re-type values from memory.
3. **Re-read before you write.** Before each build step, re-read the File A section it depends on. Before
   each check, re-read §B6.
4. **Name things the File A way.** Use File A's class names, ids, custom properties and data attributes
   exactly. Other parts of File A depend on them.
5. **No improvements.** Do not "improve" a value, rename a token, merge two rules or swap a technique for a
   "simpler" one unless §B11 tells you to step down a rung.
6. **Keep a ledger.** Keep a short running list: steps done, checks passed, defaults used, deviations
   the person approved. Put it in the delivery note.
7. **When stuck, shrink.** If a step fails twice, build the smallest version that passes its check, mark
   it, finish the page, and come back to it.

---

## 13 · Phase 4 · Proof before delivery

**Measure, do not trust estimates.** Where File A gives an estimate rule (em widths per character, line
lengths in `ch`, breakpoint values tuned to a reference name), measure the real text in the browser and use
the measured value. Record each re-derived value in the delivery note.

Run File A §18's acceptance tests, then this checklist. Every line must be true, or listed in the
delivery note with the reason.

**Truth**
- [ ] Every fact, number, employer and project on the page came from the person.
- [ ] Every figure's header note says what is simulated and what is real.
- [ ] No placeholder `{{…}}`, `[check]` or TODO remains in the file.

**Fidelity**
- [ ] Paper, ink, accent and sand appear only where §A4 allows; "on" states are ink.
- [ ] Five typefaces, each in its job; no bold headline anywhere; one italic run per headline.
- [ ] The cover name is on one line at 300, 390, 820, 1440, 1920 and 2560px, cut by the photo edge, echo aligned.
- [ ] Bleeds alternate right / left / right with square corners on the bleeding side.
- [ ] Hand marks: about ten on the page, never more than two in one viewport.
- [ ] Dark mode is re-inked, not inverted; photos are not dimmed.

**Craft**
- [ ] With JavaScript off, everything is visible within 2.5s and readable.
- [ ] With reduced motion, nothing moves on its own; the page is complete.
- [ ] Keyboard alone reaches and operates every control; the focus ring shows; sheets trap and return focus.
- [ ] No horizontal scroll from 300 to 2560px; no layout shift when readouts update.
- [ ] No console errors or warnings; no third-party requests in the network panel.
- [ ] WebGL loops stop off screen (rAF callbacks drop to near zero when no canvas is visible).

**Fit**
- [ ] Feature 01 is the work the person most wants seen.
- [ ] Reading the page aloud sounds like the person, not like a template.

---

## 14 · Delivery

Hand over:

```text
index.html
assets/
  fonts/        nine WOFF2 files named as in §A5
  portrait.webp the cover photo (1500 × 900 or larger, WebP)
  vendor/       three.min.js (three@0.149.0) and, if a figure needs it, the PDF library
  og-card.jpg   1200 × 630 social card
favicon.svg, apple-touch-icon.png, icon-192.png, icon-512.png, site.webmanifest, robots.txt, sitemap.xml
```

If you cannot download files in your environment, still reference them by these paths and tell the
person exactly where to get each one: the five font families are free under the SIL Open Font License
(download WOFF2 files, for example from the Fontsource packages `@fontsource/instrument-serif`,
`@fontsource/jost`, `@fontsource/dm-mono`, `@fontsource/nunito`, `@fontsource/caveat`), and three.js r149
is `three@0.149.0/build/three.min.js` from npm.

With the files, write a short delivery note:

1. What was built, section by section, in one line each.
2. Every default you used and every deviation the person approved.
3. Which fallback rung (§B11) each hard part is on, if any is below rung 1.
4. How to replace a photo slot with a real photo, change the accent, add a feature, and publish on GitHub
   Pages (push to `main`, enable Pages for the root).
5. Any acceptance test that does not pass, with the reason.
