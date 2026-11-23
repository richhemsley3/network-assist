# Network Assist: instructions for building the agent experience in Figma

You are building the **Network Assist** agent experience in Figma: foundations, components with every state, the screens of one end-to-end flow, and its prototype. This file is the single source of truth. Every value here was measured from the live reference build; use them exactly.

- **Reference build (open it, click through it):** https://richhemsley3.github.io/network-assist/experience/ (refresh to start over; narrow the window below 600px for mobile; add `?to=3` to open on the built answer, `?to=4` on the drill-down, `?to=5` on the retry)
- **Source:** https://github.com/richhemsley3/network-assist (`experience/index.html` is one self-contained file; its last `<style id="v5">` block is the ground truth if this file and the build ever disagree)

**Version 2, November 23, 2026.** The visual language is now the one built for Brightline Onboarding: the Design System Labs palette applied by its semantic tokens, Proxima Nova, nothing under 14px, an 8px grid, and surfaces only for what waits on the person or floats. The flow, the copy and the data are unchanged from version 1. If you built version 1, rebuild the foundations and the components; the screens and the prototype keep their structure.

**Version 2.1, the same day.** Cards come back where they belong: on home, the figures and each insight are cards; in a turn, the composed objects (breakdown, chart, payments table, receipt) are cards, while the words stay on the page. The page is warm grey 025 so cards read as objects. The AI's gradient border is drawn on top of its element's fill and must be visible all the way round, not just the glow.

**Version 2.2, the same day.** Home is a normal page, not the assistant's centred column: a page title, the briefing as a lead, and a full-width card grid. Every screen now follows reading order, **read, then comprehend, then act**: on home the ask field sits high, between the figures and the insights, in the page flow (it moves to the pinned bar only in assist mode); in a turn the evidence comes before the prepared action and the asks, which sit just above the pinned bar. This reverses version 2's "action before evidence".

Scope is the agent experience only. Do not build the preview frame, chapter controls, device switch, notes or the direction boards that sit around the prototype.

---

## 0. How to work

1. **Never invent a value.** Every colour, size, radius, shadow, duration and string is below. If something you need is missing, measure it in the reference build or ask; do not guess and do not "improve" it.
2. **Build in this order:** variables → text styles → effect styles → icons → primitives (buttons, chips, mark, AI edge) → components → screens → prototype. Nothing in a screen may use a raw value: every fill, stroke, text and effect must reference a variable or style.
3. **Auto layout everywhere.** Every component and every screen section is an auto-layout frame. Use the padding, gap and alignment given. Hug or fill as specified; fixed sizes only where a size is given.
4. **One component per concept, with variants for states.** Use component properties (variant, boolean, text, instance swap) as listed in each component's "Properties" line. Do not make separate components for states.
5. **Names are exact.** Use the names in this file for pages, collections, variables, styles, components, properties and layers.
6. **Two breakpoints only:** Desktop (design frame 1440 × 900) and Mobile (390 × 844). The layout switches at a container width of 600px. Desktop values first; mobile differences are listed under "Mobile".
7. **Everything sits on an 8px grid.** Paddings, gaps and line heights are multiples of 8 (4 only inside small controls).
8. **Report what Figma cannot do.** Some motion (word-by-word text, counting numbers, a gradient drifting forever) cannot be prototyped exactly in Figma. Build the closest Figma approximation given in §7 and also place the exact spec as an annotation on the Motion page.
9. **Finish with the checklist in §13.**

### Figma file structure

| Page | Contents |
|---|---|
| `00 Read me` | A frame with the rules in §1 and a link to the reference build |
| `01 Foundations` | Colour, type, effect and motion swatches, built from the variables and styles |
| `02 Icons and marks` | Every icon and the assistant mark |
| `03 Components` | All components with all variants, laid out in a grid with labels |
| `04 Screens · Desktop` | The desktop frames in §8, left to right in flow order |
| `05 Screens · Mobile` | The mobile frames in §8 |
| `06 Prototype` | Duplicated screens wired as one clickable flow (§9) |
| `07 Motion` | One annotated frame per motion in §7 with the exact spec |

---

## 1. The product and its rules

Network Assist is a language-first AI assistant inside a payments platform, used by a business's finance or operations lead (the fictional **Halden Supply Co.**). Home is a briefing the assistant writes. The person asks in plain language; the assistant shows its work, composes an answer from the payment records, prepares actions, and the person approves anything that moves money on a fixed platform sheet.

These rules decide every judgement call:

1. **The assistant speaks in the system's info colour** (`text/voice` #0C4A69): the briefing headline, answer sentences, insight sentences, the label before its asks, its suggested next question in the bar. **Facts and figures inside its sentences are ink** (`text/primary` #1C1A17, 600).
2. **Language on the page, objects in cards.** The assistant's words and the person's question sit directly on the page: question titles, the work trace and summary, answer sentences, the asks. Composed objects sit in cards: on home, the figures and each insight; in a turn, the breakdown, the chart, the payments table and the receipt. The assistant's own objects (the ask field, the assist bar, the prepared action) carry the AI edge instead of a card edge. The confirm sheet floats. Turns are divided by one 1px rule. Never put the assistant's sentences, the asks or the trace in a card.
3. **The AI flourish is the multicolour flow and nothing else:** a gradient of blue, periwinkle and orange that drifts along the border with a soft glow behind it, on the ask field, the assist bar and a prepared action that waits for approval; the same colours in the assistant's ring mark and the trace spinner. It is the one exception to the system palette and appears only on what the assistant owns.
4. **AI styling never touches money.** Figures, the payments table, the confirm sheet and the commit button stay plain. Once the person approves, the prepared action loses its edge and glow and turns to the subtle tone.
5. **Asks to the assistant are chips; acts on the platform are buttons.** A chip is white with a 1px blue-200 edge and navy text. The one commit in view is a navy button (48 high). Secondary acts are white buttons with a 1px dark edge. No text links, no arrows.
6. **Read, then comprehend, then act.** In a turn: the question, the work summary, the answer sentence (read); the breakdown, the chart, the table (comprehend); then the prepared action and the asks (act), which end the turn just above the pinned assist bar. On home the ask field is an exception, by an earlier decision: it sits high, between the figures card and the insights, so it is easy to see and reach. It scrolls with the page; nothing on home is pinned. Once the assist is active, the field travels to the bar pinned at the bottom. Actions that belong to one insight stay inside that insight's card.
7. **Charts are instruments, in the system's series** (the shared rules are `design/CHARTS.md` in the Brightline Onboarding project; their substance is here). Pick the form by the job: counts of items are cells, one ratio per row is a meter with its whole stated, change over time is columns from zero on a dated axis, dates against a plan are a timeline. A bar whose length is not on a stated scale is decoration and is removed. Every mark has a tooltip; columns are at most 40 wide; all chart text is 14px HTML. navy (chart-1) for the thing that matters, teal (chart-2) and light blue (chart-3) for the rest. Bars start from zero. Never a line through weekly or daily totals. Labels are real 14px text, never scaled.
8. **Status is a coloured word, never a pill:** success green, error red, warning amber.
9. **Nothing moves the flow on except the person.** The experience waits at the ask field, at send, at each ask (the chip or the bar) and at the approval.

---

## 2. Foundations

### 2.1 Colour variables

Create a collection **`Primitives`** (one mode) holding the Design System Labs colours below, and a collection **`Semantic`** (one mode, Light) whose variables alias the primitives with the system's own names. Components use only `Semantic`. If your team already has the Design System Labs library, alias to it instead of copying.

**Primitives** (the system's names)

| Variable | Value | | Variable | Value |
|---|---|---|---|---|
| `white` | #FFFFFF | | `blue/050` | #EBF4F5 |
| `warm-gray/025` | #FAF8F5 | | | |
| `warm-gray/050` | #F4F1EB | | `blue/100` | #D9EBED |
| `warm-gray/100` | #EBE6DD | | `blue/200` | #A6CBD6 |
| `warm-gray/150` | #E0DCD3 | | `blue/300` | #77B2C7 |
| `warm-gray/200` | #D0CBC3 | | `blue/450` | #4F8AA0 |
| `warm-gray/550` | #6B6760 | | `blue/700` | #0C4A69 |
| `warm-gray/650` | #54514D | | `blue/750` | #013D5B |
| `warm-gray/800` | #302D28 | | `blue/800` | #05314D |
| `warm-gray/900` | #1C1A17 | | `green/050` | #F4FAEB |
| `red/500` | #BF5547 | | `green/500` | #62800B |
| `yellow/600` | #8A7515 | | `green/600` | #7A9A01 |
| `flow/blue` | #3144B7 | | `flow/periwinkle` | #9EAAFC |
| `flow/orange` | #FE6200 | | `scrim` | #1C1A17 at 32% |

`flow/*` are the only values outside the system palette; they exist only for the AI flourish (§2.2).

**Semantic**

| Variable | Alias | Used for |
|---|---|---|
| `text/primary` | warm-gray/900 | titles, figures, data, facts inside the assistant's sentences |
| `text/secondary` | warm-gray/650 | supporting text, labels, cause amounts' context |
| `text/tertiary` | warm-gray/550 | captions, table headers, axis labels, placeholders |
| `text/voice` | blue/700 (`status-info-text`) | everything the assistant writes |
| `text/link` | blue/750 | chip text |
| `text/on-primary` | white | text on the navy button |
| `text/success` | green/500 | gains, "recoverable", the receipt label, "Approved by you" |
| `text/error` | red/500 | drops |
| `text/warning` | yellow/600 | the soonest deadline (reserved) |
| `bg/canvas` | warm-gray/025 | the page behind everything, and the assist bar's band |
| `bg/page` | white | cards, the app bar, the ask field, the bar, the prepared action, the sheet |
| `bg/subtle` | warm-gray/050 | the query block, the key cap, an approved action, hover on insights |
| `border/subtle` | warm-gray/100 | every rule: table rows, cause rows, the turn divider, the hero divider |
| `border/default` | warm-gray/150 | the app bar's bottom edge, the chart baseline, the sheet's edge |
| `border/strong` | warm-gray/200 | checkbox outline, send button with nothing to send |
| `interactive/primary` | blue/750 | the commit button, send when ready, checkbox on, the assistant's mark on the brand tile |
| `interactive/primary-hover` | blue/800 | commit button hover |
| `interactive/secondary` | white | secondary button fill |
| `interactive/secondary-border` | warm-gray/800 | secondary button edge (1px) |
| `chip/edge` | blue/200 | chip edge (1px) |
| `nav/item` | warm-gray/650 | unselected navigation and filter items |
| `nav/active-bg` | blue/100 | selected navigation item, selected table filter |
| `nav/active-text` | blue/750 | text on nav/active-bg |
| `chart/1` | blue/750 | the series that matters: this week, the recoverable cause |
| `chart/2` | blue/450 | second series, the 7-week average line, hover |
| `chart/3` | blue/200 | other bars, third cause |
| `chart/grid` | warm-gray/100 | dotted grid lines, bar and progress tracks |
| `chart/axis` | warm-gray/550 | axis labels |
| `chart/tooltip` | warm-gray/900 | tooltip fill (text white) |
| `status/success-strong` | green/600 | the done check, the progress fill, the recoverable outline |
| `status/success-bg` | green/050 | inside the recoverable block |
| `overlay/scrim` | scrim | behind the confirm sheet |

### 2.2 Gradients (paint styles)

Figma variables cannot hold gradients; make these **paint styles** in a folder `AI/`. They are the only gradients in the file.

| Style | Type | Stops |
|---|---|---|
| `AI/Flow` | Linear, horizontal | 0% #3144B7 · 18% #9EAAFC · 34% #FE6200 · 50% #9EAAFC · 66% #3144B7 · 82% #9EAAFC · 100% #FE6200. Apply to a layer **three times the width** of the element it decorates; only one third is visible at a time (see §4.3). |
| `AI/Mark` | Angular (conic) | 0% #3144B7 · 25% #9EAAFC · 50% #FE6200 · 75% #9EAAFC · 100% #3144B7 |
| `AI/Spinner` | Angular | 0% #3144B7 · 28% #9EAAFC · 57% #FE6200 · 85% #FE6200 at 0% |

The brand tile, the charts, the bars and every surface are flat colour.

### 2.3 Number variables

Collection **`Layout`**:

| Variable | Value | | Variable | Value |
|---|---|---|---|---|
| `space/4` | 4 | | `radius/surface` | 12 (cards, prepared action, ask field, assist bar) |
| `space/8` | 8 | | `radius/control` | 10 (buttons, chips, send) |
| `space/12` | 12 | | `radius/item` | 8 (nav items, filters, query block, tooltip, key cap) |
| `space/16` | 16 | | `radius/brand` | 6 |
| `space/24` | 24 | | `radius/bar` | 4 (bars, tracks, checkbox, chart bar tops) |
| `space/32` | 32 | | `radius/sheet` | 16 (desktop) · 16 / 16 / 0 / 0 (mobile) |
| `space/48` | 48 | | `width/content` | 896 in the conversation (960 with 32 side padding); home is full width, max 1440 with 40 side padding |
| `height/commit` | 48 | | `width/assist-bar` | 896 |
| `height/control` | 40 (chips, secondary buttons, send) | | `width/sheet` | 480 |
| `height/field` | 56 (ask field, assist bar) | | `breakpoint` | 600 |

### 2.4 Type

Typeface: **Proxima Nova**, weights **300 Light**, **400 Regular** and **600 Semibold** only. Numbers use **tabular figures** (OpenType `tnum`) everywhere. The only other face is a monospace for the query block (SF Mono; Menlo if SF Mono is unavailable). **No text is smaller than 14px.** Line heights are on the 8px grid.

| Style | Size / line height | Weight | Letter spacing | Default colour | Used for |
|---|---|---|---|---|---|
| `Figure/Large` | 48 / 48 | 300 | −3% | text/primary | Home "Paid out this week", the Payouts insight, the answer's "Paid out" |
| `Figure/Sheet` | 40 / 48 | 300 | −3% | text/primary | Amount on the confirm sheet |
| `Figure/Medium` | 28 / 32 | 300 | −2% | text/primary | Hero side figures, Disputes and Next payout insights |
| `Heading/Page` | 28 / 32 | 600 | −2% | text/primary | The home page title ("Home") |
| `Body/Briefing` | 22 / 32 | 400 | −1% | text/voice | The briefing sentence under the page title (max width 72ch); figures inside it are 600 text/primary |
| `Heading/Question` | 28 / 32 | 600 | −2% | text/primary | Every question title (first turn and follow-ups) |
| `Heading/Action` | 22 / 32 | 600 | −1.5% | text/primary | Prepared action title, sheet title |
| `Heading/Section` | 17 / 24 | 600 | 0 | text/primary | Chart title, receipt title |
| `Body/Lead` | 17 / 28 | 400 | 0 | text/voice | Answer sentences (max width 62ch, about 600) |
| `Body/Default` | 14 / 24 | 400 | 0 | text/primary | Everything else: trace, summary, cause rows, table, sheet rows, nav, bar, labels, captions, axis labels |
| `Body/Strong` | 14 / 24 | 600 | 0 | text/primary | Figures inside text, step results, amounts, chips, secondary buttons |
| `Body/Commit` | 15 / 24 | 600 | 0 | text/on-primary | The navy commit button only |
| `Mono/Query` | 14 / 24 | 400 | 0 | text/secondary | Query block |

Captions, labels, table headers and axis labels are `Body/Default` in text/tertiary or text/secondary; there is no smaller label style.

**Mobile** (container below 600): Page title 22 / 28 · Briefing 17 / 28 · Question 22 / 28 · Lead 16 / 24 · Large figure 40 / 48 · Hero side figure 20 / 28 · Sheet figure 32. Make these as separate styles with a `Mobile/` prefix.

### 2.5 Effect styles

| Style | Layers |
|---|---|
| `Surface/Card` | Drop shadow 0, 0, blur 0, spread 1, border/default (the 1px edge) · Drop shadow 0, 1, blur 2, #1C1A17 4%. Every card. |
| `Surface/Card hover` | Drop shadow 0, 0, blur 0, spread 1, border/strong · Drop shadow 0, 8, blur 24, spread −12, #1C1A17 18%. Insights on hover. |
| `Surface/Float` | Drop shadow 0, 0, blur 0, spread 1, border/default · Drop shadow 0, 16, blur 40, spread −16, #1C1A17 28%. Only on the confirm sheet. |
| `AI/Glow · rest` | Layer blur 12 on the glow layer, opacity 32% |
| `AI/Glow · working` | Layer blur 13, opacity 42% |
| `AI/Glow · landed` | Layer blur 15, opacity 55% |
| `AI/Glow · prepared` | Layer blur 16, opacity 24% |

Cards use `Surface/Card`; the AI's objects use the AI edge and no card shadow; an approved action sits on bg/subtle with no edge.

### 2.6 Motion tokens

Create a number collection **`Motion`** for durations (ms). Easing cannot be a variable; record the curves as text on the Motion page and use **Custom bezier** in prototype transitions.

| Easing | Curve | Feel |
|---|---|---|
| `ease/standard` | cubic-bezier(0.2, 0.8, 0.2, 1) | default for fades, slides, colour changes |
| `ease/out` | cubic-bezier(0.16, 1, 0.3, 1) | things arriving and settling: FLIP moves, bars growing, the sheet |
| `ease/pop` | cubic-bezier(0.34, 1.56, 0.64, 1) | small overshoot: checks |
| `ease/in-out` | ease-in-out | the AI edge drifting |
| `linear` | linear | spinners, the mark turning |

| Duration variable | ms | | Duration variable | ms |
|---|---|---|---|---|
| `dur/press` | 200 | | `dur/reveal` | 700 |
| `dur/hover` | 250 | | `dur/count` | 800 |
| `dur/toggle` | 300 | | `dur/bars` | 800 |
| `dur/scrim` | 350 | | `dur/glow` | 900 |
| `dur/step` | 420 | | `dur/figure-count` | 1100 |
| `dur/fade` | 450 | | `dur/bloom` | 2200 |
| `dur/dim` | 500 | | `dur/progress` | 2600 |
| `dur/sheet` | 550 | | `dur/drift` | 10000 |
| `dur/bar` | 600 | | `dur/title-rise` | 650 |

---

## 3. Icons and marks

All icons are strokes (round caps and joins) inside a square frame named as below. Put them on `02 Icons and marks` as components.

| Component | Frame | Drawing |
|---|---|---|
| `Icon/Arrow up` | 16 × 16 | Path M8 13.5 V2.5 · M3 7.5 L8 2.5 L13 7.5. Stroke 2, white. |
| `Icon/Stop` | 10 × 10 | Rectangle 10 × 10, radius 2, fill white. |
| `Icon/Check` | 14 × 14 | Path M2.5 7.5 L5.5 10.5 L11.5 3.5. Stroke 1.8, status/success-strong. |
| `Icon/Checkbox check` | 11 × 11 (in 12 grid) | Path M2.5 6.5 L5 9 L9.5 3. Stroke 2.2, white. |
| `Mark/Assistant` | 12 × 12 | A ring: ellipse 12 × 12 minus a centred ellipse 8 × 8 (Subtract). Fill `AI/Mark`. Variants `State=Rest` and `State=Working`; Working rotates continuously (360° per 1600 ms, linear) in the prototype. |
| `Mark/Spinner` | 14 × 14 | Ellipse 14 × 14 minus centred ellipse 10 × 10. Fill `AI/Spinner`. Rotates 360° per 900 ms, linear. |
| `Mark/Brand` | 24 × 24 | Rectangle, radius 6, fill interactive/primary. No effect. |
| `Mark/Legend` | 10 × 10 | Rectangle radius 3; fill chart/1, chart/2 or chart/3 per cause. |

---

## 4. Primitives

### 4.1 Buttons

**`Button/Commit`** (the one action in view that moves money)
- Auto layout horizontal, hug, height 48, padding 0 / 24, centred. Fill interactive/primary, radius 10, no effect.
- Text `Body/Commit`.
- Variants `State`: Default · Hover (fill interactive/primary-hover). When the action is done the button is removed, not disabled.
- Variant `Size`: Large (48, used in the prepared action and the sheet) · Compact (height 40, padding 0 / 16, text 14 / 600, used only on the Payouts insight on home).
- Property: `Label` (text). Labels used: "Review and retry", "Retry 19 payments".

**`Button/Secondary`**
- Hug, height 40, padding 0 / 16, radius 10. Fill interactive/secondary, inner stroke 1px interactive/secondary-border. Text `Body/Strong` text/primary.
- Variants `State`: Default · Hover (fill bg/subtle) · Disabled (fill bg/subtle, no stroke, text text/tertiary).
- Properties: `Label` (text), `Countdown` (boolean; when on, the label is followed by " · " and a mm:ss value, for example "Cancel the rest · 9:58").

**`Button/Steps`** (shows the assistant's query)
- Text only: `Body/Default` text/secondary "4 steps" with a 1px dotted underline in text/tertiary (offset 2). No fill, no icon.
- Variants `State`: Closed · Open (same look; the query block shows) · Hover (text text/primary). Boolean `Interactive`: off for the second turn ("2 steps", same look, not clickable).

**`Button/Send`**
- 40 × 40, radius 10, `Icon/Arrow up` centred.
- Variants `State`: Empty (fill border/strong) · Ready (fill interactive/primary) · Waiting for you (fill interactive/primary; Hover fill interactive/primary-hover) · Working (fill interactive/primary, `Icon/Stop`).

**`Key cap`**
- Hug, height 24, padding 0 / 8, radius 8, fill bg/subtle, no stroke. Text `Body/Default` text/tertiary "Tab".

**`Nav item`** (app bar navigation and the table filter)
- Hug, padding 4 / 12, radius 8, `Body/Default`. Unselected: no fill, text nav/item. Selected: fill nav/active-bg, text nav/active-text, 600.
- Items sit in a horizontal auto layout with gap 4, no track behind them.

**`Checkbox`**
- 18 × 18, radius 4. Variants `State`: Off (no fill, inner stroke 1.5 border/strong) · On (fill interactive/primary, `Icon/Checkbox check` centred).

### 4.2 `Chip` (an ask to the assistant)
- Auto layout horizontal, hug, height 40, padding 0 / 16, radius 10, centred. Fill bg/page, inner stroke 1px chip/edge. Text `Body/Strong` text/link.
- Variants `State`: Default · Hover (fill bg/subtle) · Answered (stroke border/subtle, text text/tertiary; not clickable): a chip in a turn the person has moved on from.
- Property: `Label`.

**`Asks row`** (component): auto layout horizontal, wrap, gap 12, align centre. A label (`Mark/Assistant` + text, gap 12, `Body/Default` text/voice, 4 right margin) then one or more `Chip`s.
- First turn: "Before you decide" + "Show me the declined ones". Second turn: "Next" + "Retry them".

### 4.3 The AI edge (`AI/Edge`)

This is the assistant's signature. It is not a stroke style; it is two layers placed **behind** the content of the element it decorates. Build it once as a component and place it as the bottom two layers inside the ask field, the assist bar and the prepared action.

Structure, inside a frame the same size and radius as its host (`Clip content` off):

1. **`Glow`** (bottom). A rectangle inset from the host edges by the glow inset below, same corner radius, fill `AI/Flow`, layer blur and opacity per state. It spills softly outside the host.
2. **`Edge`**. The ring of colour itself:
   - a frame the size of the host with `Clip content` on, containing a rectangle **300% of the host's width** filled with `AI/Flow`;
   - masked by a **ring shape**: the host's rounded rectangle minus the same rectangle inset by 1.5 (every host) with its radius reduced by the same amount. Use Subtract, then use as mask.
   - Under the mask, add the host's fill (bg/page) as a separate inner rectangle so the host stays white.
3. The host's own content sits above both.

**The edge is always visible.** The 1.5px gradient ring sits on top of the host's white fill, all the way round, at full strength; the glow only spills outside it. If the host's fill ends up above the edge (in Figma, a fill on the host frame itself; in code, a background without `isolation: isolate` on the host), the ring disappears and only the glow shows. That is a defect.

States (variant property `State` on `AI/Edge`):

| State | Glow inset (top/bottom, left/right) | Glow blur | Glow opacity | When |
|---|---|---|---|---|
| Rest | 3, 6 | 12 | 32% | Idle ask field and assist bar |
| Typing | 2, 4 | 13 | 42% | Home ask field while the person types |
| Working | 2, 4 | 13 | 42% | Assist bar while the assistant works |
| Landed | 0, 1 | 15 | 55% | Assist bar for 1.3 s when an answer finishes with nothing to suggest |
| Suggesting | 3, 6 | 12 | 45% | Assist bar while it offers the next question |
| Prepared | 6, 12 | 16 | 24% | Prepared action |
| Off | none | | 0% | Prepared action after approval (no edge, no glow) |

Motion: the 300%-wide gradient slides from x = 0 to x = −200% of the host width and back, 10 000 ms each way, ease-in-out, forever (the edge strip and the glow strip move together). In Figma, make two variants of the strip position (`Drift=A`: x 0, `Drift=B`: x −200%) and loop them with After delay → Smart animate, 10 000 ms, ease-in-out. Glow changes between states use Smart animate 900 ms ease/standard (Landed comes in at 350 ms).

### 4.4 The assistant mark in context

Wherever the assistant is the author, put `Mark/Assistant` (12 × 12) before the text with a gap of 12: the briefing byline, the work summary line, inside the ask field and the assist bar, the prepared action's label, and the asks row's label. While the assistant is working, every visible mark is `State=Working` (turning).

---

## 5. Components

Each entry: anatomy (outer to inner), layout, tokens, variants and properties, interactions, motion, mobile. "On the page" means no fill, no stroke, no shadow, no radius: the content sits directly on bg/canvas. **"Card" means fill bg/page, radius 12, padding 24 (16 on mobile), effect `Surface/Card`.**

### 5.1 `App bar`
- Frame fill width, height 56, auto layout horizontal, padding 0 / 24, gap 24, align centre. Fill bg/page, bottom stroke 1px border/default.
- Children: `Brand` (gap 12: `Mark/Brand` 24 + "Halden Supply Co." in `Body/Strong`) · `Nav item`s Home (selected), Payments, Payouts, Disputes · spacer (fill) · date "Fri 21 Nov" in `Body/Default` text/tertiary.
- Mobile: padding 0 / 16; the navigation and the date are hidden.

### 5.2 `Page header` (home)
- On the page, auto layout vertical, fill width: title "Home" in `Heading/Page` · 4 below it the byline (horizontal, gap 12: `Mark/Assistant` + "Your weekly briefing, 16–22 Nov" in `Body/Default` text/secondary) · 16 below that the briefing sentence in `Body/Briefing`, max width 72ch: "Sales were up this week. Payouts were $8,490.00 lower, and about half of that can be recovered today."
- Mobile: title 22 / 28, briefing 17 / 28.

### 5.3 `Hero figures`
- A card across home grid columns 1–2. Auto layout horizontal, gap 32, two columns at 1.3 : 1.
- Left column, vertical: "Paid out this week" `Body/Default` text/secondary · "$38,420.00" `Figure/Large`, 8 above and below · delta line `Body/Default` text/secondary: "−$8,490.00" in text/error, then " · 18.1% below last week" · **mini column chart**, 24 above, filling the rest of the column's height (at least 96): eight columns from zero on a $0–$50,000 scale, each centred in an equal slot (gap 8), at most 32 wide, top corners radius 4, chart/3, this week chart/1; a 1px border/default baseline; under it three axis labels in `Body/Default` chart/axis: "28 Sep" left, "8 weeks" centre (text/tertiary), "16 Nov" right. Each column has a hover tooltip (§5.10) with the week and the amount; the hovered column turns chart/2 (this week stays chart/1).
- Right column: a 1px border/subtle on its left edge and 32 padding left; three rows divided by 1px border/subtle, each padding 16 / 0 (the first has no top padding or rule): label `Body/Default` text/secondary · value `Figure/Medium` · note `Body/Default` text/tertiary with a coloured delta.
  - Gross volume / $214,860.40 / "+3.2%" (text/success) " on last week" · Payment success / 94.1% / "−2.3 pts" (text/error) " on last week" · Open disputes / 3 / "$2,640.00 held".
- Mobile: one column. The large figure is 40 and the mini chart 72 high with gap 4. The right column becomes three stacked rows, each with a 1px border/subtle top rule and padding 12 / 0: label and note on the left, the figure (20 / 28) right aligned.

### 5.4 `Ask field` (home)
- Frame fill width, min height 56, radius 12, auto layout horizontal, padding 8 / 8 / 8 / 20, gap 16, align centre. Fill bg/page with `AI/Edge` behind. No shadow.
- **Placement:** the row between the Hero figures card and the insights, full width, with 32 reserved below it for the scope line. It is part of the page and scrolls with it. Do not move it to the end of the page, and do not fix or stick it to the window: only the assist bar is pinned, and only once the assist is active.
- Children: `Mark/Assistant` · text (fill, single line, truncate with ellipsis) `Body/Default` · `Button/Send`.
- Under it, a **scope line** (24 below; reserve 24 of bottom padding in the section): `Body/Default` text/tertiary with the understood terms in 600 text/primary: "Looking at **payouts** for **16–22 Nov**, compared with **9–15 Nov**". Left inset 40 (aligns under the text).
- Variants `State`:
  | State | Text | Send | Edge | Scope line |
  |---|---|---|---|---|
  | Rest | "Reply to this briefing, or ask about anything" in text/tertiary | Empty | Rest | hidden |
  | Hover | same | Empty | Rest, glow 50% | hidden |
  | Typing | the typed text in text/primary with a 2 × 1.1em caret in interactive/primary | Empty | Typing | hidden |
  | Understood | "Why were payouts lower this week?" with "payouts" and "this week" underlined (1.5px, interactive/primary, at the baseline) | Waiting for you | Typing | shown |
- Interaction: click Rest → Typing (the rest of the briefing dims to 30%). Send is the only way on.
- Mobile: scope line inset 16.

### 5.5 `Insight card`
- A card, auto layout vertical, gap 4. The insight group aligns with the content column.
- Variants `Type`:
  - **Lead** (Payouts). Children in order:
    1. "Payouts" `Body/Default` text/secondary.
    2. "$4,212.00" `Figure/Large` in text/success, 8 above.
    3. Sentence `Body/Default` text/voice, 4 above, max width 44ch: "can be recovered today. 19 payments were declined after Westmoor Bank started requiring 3-D Secure on 17 Nov."
    4. Customer list, 16 above and below: four rows, each auto layout horizontal space-between, padding 12 / 0, top rule 1px border/subtle, `Body/Default` with the amount in 600: Larch Hotel Group $731.10 · Norrow Bakehouse $455.75 · Orchard Lane Café $312.40 · and a last row in text/tertiary with the amount in 400: "16 more customers" $2,712.75.
    5. `Button/Commit` Size=Compact "Review and retry", left aligned, 8 above.
  - **Standard** (Disputes, Next payout). Children: label · flexible spacer (min 16) · figure `Figure/Medium` · sentence `Body/Default` text/voice, max width 44ch.
    - Disputes: "Disputes" · "4 days" · "until evidence is due on the first of 3 disputes. $2,640.00 is held until they're decided."
    - Next payout: "Next payout" · "$41,276.40" · "arrives Monday 24 Nov, including $1,638.00 held over from the holiday."
- `State`: Default (`Surface/Card`) · Hover (`Surface/Card hover`: the edge darkens to border/strong and a soft shadow appears; nothing moves). The whole card is the target; there are no links inside it. Clicking the Lead asks "Why were payouts lower this week?"; its button goes straight to the prepared retry.
- Layout on home: part of the page grid in §6 (the Lead in the third column beside the figures, running down beside Disputes and Next payout).
- Mobile: one column, gap 16, cards pad 16.

### 5.6 `Question title`
- `Heading/Question` in every turn, text/primary, fill width. It is the person's words, so ink, not the voice colour.

### 5.7 `Work trace` and `Work summary`
**Trace step** (component `Trace step`): auto layout horizontal, padding 4 / 0, gap 12, fill width. Columns: icon 16 wide · text (fill) `Body/Default` · result `Body/Strong`.
- Variants `State`: Running (icon `Mark/Spinner`, text text/secondary, result hidden) · Done (icon `Icon/Check`, text text/tertiary, result shown in text/primary).
- First turn steps (results in brackets): Reading payouts for 16–22 Nov ($38,420.00) · Comparing with 9–15 Nov (−$8,490.00) · Matching 214 balance transactions to the gap (3 causes) · Checking which declines can be retried (19 of 19).
- Second turn: Filtering declined payments since 17 Nov (19 payments) · Checking each decline reason and bank (All Westmoor Bank).
- Third turn: Preparing a retry with authentication for 19 payments ($4,212.00) · Checking your limits and approval rules (Within limits).

**Work summary** (component `Work summary`): replaces the trace when the work is done; the folded trace leaves no space behind. Auto layout horizontal, gap 8 / 12, align centre. `Mark/Assistant` · `Body/Default` text/tertiary sentence (fill) · `Button/Steps`.
- First turn: "Read payouts, balance transactions, disputes and Westmoor Bank's notice of 17 Nov" + "4 steps" (interactive).
- Second turn: "Filtered the 19 declined payments and checked each card's bank" + "2 steps" (not interactive).
- Third turn has no summary; its trace stays visible.

**Query block** (component `Query block`), shown under the summary when Steps is open: fill bg/subtle, radius 8, padding 16, `Mono/Query` text/secondary, three lines:
```
payouts      where account = halden_supply and week in (2026-11-16, 2026-11-09)
balance_txn  where payout_week = 2026-11-16 group by cause
payments     where status = declined and decline_code = authentication_required
```

### 5.8 `Answer sentence`
- `Body/Lead`, text/voice, max width 62ch. Figures inside are 600 text/primary. First turn: "Payouts were **$38,420.00** this week, $8,490.00 less than last week. Three things explain all of it, and about half can be recovered today." Second: "These are the 19 payments Westmoor Bank declined with “authentication required”. Every one can be retried, so I've selected them all." Third: "It's prepared. Nothing is sent until you confirm."

### 5.9 `Breakdown card` (Paid out)
- A card, full content width, auto layout vertical, gap 8.
- "Paid out · 16–22 Nov" `Body/Default` text/tertiary · "$38,420.00" `Figure/Large` · "**−$8,490.00** · 18.1% below 9–15 Nov" `Body/Default` text/secondary with the amount 600 text/primary.
- Split bar, 8 above: height 16, auto layout horizontal gap 4, three segments radius 4: 49.6% chart/1 · 31.1% chart/2 · 19.3% chart/3.
- Cause rows (`Cause row` component): grid 10 | fill | hug, gap 12, padding 12 / 0, top rule 1px border/subtle. Marker `Mark/Legend` · title `Body/Default` with a sub-line `Body/Default` text/tertiary · amount `Body/Strong`.
  - Declined after Westmoor Bank's 3-D Secure change / "19 payments since 17 Nov · " + "recoverable" in text/success / $4,212.00 (chart/1)
  - Disputes holding funds / "3 disputes · decisions due by 2 Dec" / $2,640.00 (chart/2)
  - Holiday payout moves to Mon 24 Nov / "Timing only · nothing is lost" / $1,638.00 (chart/3)

### 5.10 `Chart card` (Payouts, last 8 weeks)
- A card, full content width, auto layout vertical, gap 16.
- Header: auto layout horizontal space-between, wrap: "Payouts, last 8 weeks" `Heading/Section` and "dashed line: the 7-week average, $44,096 · green: what retrying recovers" `Body/Default` text/tertiary.
- Plot, 24 above: a y-axis column 48 wide and a plot 200 high.
  - Y axis $0, $30k, $60k: `Body/Default` chart/axis, right aligned, centred on their lines. Grid lines at $30k and $60k: 1px chart/grid, dotted. Baseline at $0: 1px border/default, solid.
  - Eight columns from zero, each centred in an equal slot (gap 8), **at most 40 wide**, top corners radius 4: weeks 1 to 7 chart/3, this week (16 Nov) chart/1. Heights are value ÷ $60,000 of the plot (§11).
  - Recoverable: a block on top of this week's column from $38,420 to $42,632, the column's width, fill status/success-bg, 1.5px dashed status/success-strong on the top and sides, top corners radius 4. Its label "+$4,212" `Body/Strong` text/success, centred 4 above it.
  - The 7-week average: a 1.5px dashed chart/2 line across the plot at $44,096. No label in the plot; the header names it.
  - X labels under the plot, `Body/Default` chart/axis, centred under their columns: 5 Oct, 19 Oct, 2 Nov, 16 Nov. Under "16 Nov" (text/primary), this week's value "$38,420" in `Body/Strong`: the latest value is labelled at the axis, never inside a narrow column.
- Hover state (component `Bar tooltip`): the hovered bar turns chart/2; a tooltip above it, fill chart/tooltip, radius 8, padding 8 / 12, two lines (line height 20): "9 Nov" in white 400 and "$46,910.00" in white 600. Tooltips on the first two columns align left, on the last two align right; none on this week's bar.
- Mobile: y-axis column 40, plot 160 high, gap 4; "+$4,212" aligns right.

### 5.11 `Prepared action`
- The one surface in a turn: fill bg/page, radius 12, padding 24, no shadow, `AI/Edge` State=Prepared behind. A two-column grid: content (fill) and the button (hug), gap 8 / 32, aligned centre.
- Content, vertical: label row `Mark/Assistant` + "Waiting for your approval" `Body/Default` text/tertiary, gap 12 · title `Heading/Action`: "Retry the 19 declined payments" (first turn) or "Retry 19 payments from Westmoor Bank cardholders" (third turn) · meta `Body/Default` text/secondary ("**$4,212.00** · each customer gets a short 3-D Secure prompt", or "**$4,212.00** · settles to the payout on Mon 24 Nov").
- Button column: `Button/Commit` "Review and retry", spanning all three rows, vertically centred.
- Variants `State`: Waiting (as above) · Approved (AI/Edge Off, fill bg/subtle, label "Approved by you at 4:12 pm" in text/success with no mark, the button removed).
- When it appears, its glow blooms once (see §7).
- Mobile: one column, padding 16; the button goes under the meta, 8 above, left aligned.

### 5.12 `Turn`
- A follow-up turn: auto layout vertical, gap 24, padding top 48, 24 above, top rule 1px border/subtle, fill width. Contains: Question title · trace or summary · answer sentence · the prepared action and the asks row · the evidence.
- Mobile: padding top 32, 8 above, gap 16.

### 5.13 `Payments card`
- A card, auto layout vertical, gap 16.
- Filters: `Nav item`s All 19 (selected) · Over $200 · Repeat customers, gap 4.
- Table: header row `Body/Default` text/tertiary, padding bottom 8, bottom rule 1px border/subtle: (checkbox column 30) · Customer · Declined · Card · Amount (right aligned). Body rows height 48, bottom rule 1px border/subtle, `Body/Default`: checkbox · name text/primary · date text/secondary · "•••• 4471" text/secondary · amount text/primary right aligned. Cells have 16 right padding except the last.
- Rows (component `Payment row`, variants `Selected` On/Off): Orchard Lane Café · 17 Nov · 4471 · $312.40 / Pell & Daughters · 17 Nov · 0932 · $189.00 / Norrow Bakehouse · 18 Nov · 7710 · $455.75 / Kite Street Deli · 18 Nov · 2286 · $98.20 / Ashby Market · 19 Nov · 5103 · $260.00 / Larch Hotel Group · 19 Nov · 6648 · $731.10 / Fenwick Florists · 20 Nov · 3390 · $144.85 / Marlow Provisions · 21 Nov · 8821 · $205.50.
- Footer: space-between, `Body/Default`: "11 more · $1,815.20" text/tertiary and "19 selected · $4,212.00" in 600 text/primary.
- Mobile: hide the Declined and Card columns (headers and cells together).

### 5.14 `Confirm sheet` (platform-owned: no AI styling)
- Scrim: full screen overlay/scrim.
- Sheet: width 480 (desktop, centred) · fill bg/page · radius 16 · padding 24 · auto layout vertical, gap 16 · effect `Surface/Float`.
- Content: "Confirm · Halden Supply Co." `Body/Default` text/tertiary · "Retry 19 declined payments" `Heading/Action` · "$4,212.00" `Figure/Sheet` · three rows (grid fill | hug, gap 12, padding 12 / 0, top rule 1px border/subtle, `Body/Default`, left cell text/tertiary): Customers / 19 Westmoor Bank cardholders · What they see / A 3-D Secure prompt by email · Settles to / Payout on Mon 24 Nov · actions right aligned, gap 12: `Button/Secondary` "Not now", `Button/Commit` "Retry 19 payments" · note `Body/Default` text/tertiary: "This sheet is part of the platform, not generated. Nothing is sent until you press Retry."
- Mobile: full width, anchored to the bottom, radius 16 / 16 / 0 / 0; the figure is 32.

### 5.15 `Receipt card`
- A card, auto layout vertical, gap 8.
- "Retry started · today, 4:12 pm" `Body/Default` text/success · "19 customers have been sent a 3-D Secure prompt" `Heading/Section` · progress bar 8 above and below: height 8, radius 4, track chart/grid, fill status/success-strong · row space-between, wrap, gap 8 / 24, `Body/Default` text/secondary: "**4 of 19** authenticated · **$1,055.35** recovered so far" (bold parts 600 text/primary) and `Button/Secondary` with Countdown on: "Cancel the rest · 9:58".
- Variants `State`: Running (progress 21% = 4 of 19) · Cancelled (secondary button Disabled, label "Stopped · $1,055.35 kept").

### 5.16 `Assist bar` (bottom)
- Container: fill width, padding 16 / 24 / 24, centred content, fill bg/canvas (its own row under the conversation). Hidden on home until the person sends a question from the ask field.
- Bar: width 896 (fill on mobile), min height 56, radius 12, auto layout horizontal, padding 8 / 8 / 8 / 20, gap 16, align centre, fill bg/page, `AI/Edge` behind, `Body/Default`.
- Children: `Mark/Assistant` · text (fill) · `Key cap` (only when suggesting) · `Button/Send`.
- Variants `State`:
  | State | Text | Key cap | Send | Edge | Mark |
  |---|---|---|---|---|---|
  | Idle | "Ask a follow-up" text/secondary | off | Empty | Rest | Rest |
  | Working | "Ask a follow-up" text/secondary | off | Working (stop) | Working | Working |
  | Landed (after the last turn only) | as Idle | off | Empty | Landed | Rest |
  | Suggesting | the suggested question in text/voice ("Show me the declined ones", then "Retry them") | on | Empty | Suggesting | Rest |
  | Accepted | the same question in text/primary | off | Ready | Rest | Rest |
- Interaction: in Suggesting, clicking anywhere on the bar, pressing Tab, or clicking the matching chip in the asks row accepts the suggestion.
- Mobile: container padding 12 / 16 / 16, Key cap hidden.

---

## 6. Layout of the two screens

### Home (a normal page)
- App bar (bg/page, 1px bottom edge), then a scrolling view on bg/canvas. The page container is full width and left aligned: padding 40 left and right, max width 1440, top padding 32. Not the centred reading column; that belongs to the conversation.
- A three-column grid, gap 24:
  1. Page header across all three columns.
  2. Hero figures card across all three columns.
  3. Ask section across all three columns, in the page flow (§5.4).
  4. Lead insight card across columns 1–2, spanning two rows · Disputes card in column 3.
  5. Next payout card in column 3, beside the Lead.
- Container below 1100: two columns (header; hero; ask section; Lead on the left spanning two rows beside Disputes and Next payout).
- No assist bar on home.

### Answer (conversation)
- App bar, a scrolling view, and the Assist bar pinned to the bottom.
- Content column 960 wide centred (896 inside 32), top padding 32, bottom 40, **one column**, vertical gap 24.
- Order in the first turn, read → comprehend → act: Question title · Work summary (after the trace folds) · Query block (only when open) · Answer sentence · Breakdown card · Chart card · **Prepared action · Asks row**. Then Turn 2 (title · summary · sentence · Payments card · Asks row) and Turn 3 (title · trace · sentence · Prepared action · Receipt card). The turn ends with its action just above the pinned bar. Everything sits on bg/canvas; the cards and the AI objects are white.

### Mobile
- Content padding 16, one column throughout, the same order, gap 16. Home stacks: header, hero, ask section, Lead, Disputes, Next payout.

---

## 7. Motion

Every motion below is part of the design. Build what Figma can, and put the exact spec on `07 Motion` as an annotated frame per row. "FLIP" means the element is drawn in its new place and animated from where it was.

### 7.1 Always on
| What | Spec | Figma approximation |
|---|---|---|
| AI edge drift | Gradient strip 300% wide slides x 0 → −200%, 10 000 ms ease-in-out, then back; forever | Two strip variants, After delay 0 → Smart animate 10 000 ms ease-in-out, looping |
| Mark turning (only while working) | rotate 360°, 1600 ms linear, repeat | Variant loop with rotation 0 → 360, Smart animate linear |
| Spinner | rotate 360°, 900 ms linear, repeat | Same technique |
| Caret | blinks: visible 525 ms, hidden 525 ms | Two variants, Instant change after 525 ms |

### 7.2 Home
| Step | Spec |
|---|---|
| Briefing loads | Insights rise: opacity 0 → 1 and y +10 → 0, 500 ms ease/standard, staggered 0 / 80 / 160 ms |
| Insight hover | Effect to `Surface/Card hover`, 250 ms ease/standard. Nothing moves |
| Click the ask field | Everything on home except the ask section fades to 30% (500 ms ease/standard); the field's edge goes to Typing (900 ms); a caret appears |
| Typing | One character every 45–85 ms (spaces 90 ms). After the last character: "payouts" underline grows left to right 0 → 100% over 500 ms, then "this week" 220 ms later; the scope line fades up (opacity 0 → 1, y +4 → 0, 450 ms) 180 ms after that; Send becomes Waiting for you |
| Press send | The scope line fades out. At the same moment: the ask field travels to the assist bar's position at the bottom (FLIP, 700 ms ease/out, translating and scaling from its old box); the typed question travels into the Question title (FLIP, 650 ms ease/out, opacity 60% → 100%, scaling from the field's text size). The briefing is removed. 650 ms later the assist bar goes to Working |

### 7.3 The assistant working
| Step | Spec |
|---|---|
| Each trace step | Appears: opacity 0 → 1, y +6 → 0, 420 ms ease/standard. Runs for 700 ms (the third step of the first turn runs 950 ms). Then the spinner becomes the check, drawn along its path over 350 ms; 120 ms pause before the next step |
| Trace folds | 1400 ms after the last step: the trace collapses its height to 0 (550 ms ease/standard), the Work summary fades in (450 ms), then the trace is removed so it leaves no gap |
| Steps toggle | Click: the Query block appears below the summary; click again hides it. No animation on the label |

### 7.4 The answer composing (first turn)
The evidence arrives first, then the action; the view follows down so the turn ends on the action and the asks.

| Step | Spec |
|---|---|
| Answer sentence | Fades in word by word: each word opacity 0 → 1 over 700 ms, starting 28 ms after the previous word |
| Breakdown | 350 ms after the sentence: reveal (opacity and y, 700 ms). The figure counts from $46,910.00 down to $38,420.00 over 1100 ms, ease-out (quartic). The split bar's segments grow from 0 width to their share, 600 ms ease/out each, 180 ms apart. Cause rows rise (opacity, y +10 → 0, 420 ms), 110 ms apart |
| Payouts chart | 450 ms later: reveal; the average line fades in (500 ms, 800 ms delay). The bars grow from the baseline to their height, 800 ms ease/out each, 70 ms apart left to right. 1300 ms after they start, the recoverable block grows up from the top of this week's bar (scale y 0 → 1, 700 ms ease/out), this week's value appears inside its bar, and "+$4,212" counts from +$0 over 800 ms (starting 400 ms in) |
| Prepared action | 250 ms after the chart: opacity 0 → 1 and y +10 → 0, 700 ms; its glow blooms 24% → 60% → 24% over 2200 ms (peak at 35%), starting 300 ms after it appears |
| Asks row | 500 ms later: opacity 0 → 1 and y +10 → 0, 700 ms |
| Answer finished | The assist bar switches straight to Suggesting with "Show me the declined ones": text in the voice colour, Key cap shown, glow to 45% (900 ms ease/standard). Landed (glow 55% in 350 ms, back to Rest 1300 ms later) is used only when there is nothing to suggest, after the third turn |

### 7.5 Follow-ups
| Step | Spec |
|---|---|
| Accept a suggestion (the bar, Tab, or the chip) | The chips in the turn just left go to Answered. The bar's text turns from the voice colour to ink and Send becomes Ready; 450 ms later the question travels from the bar into the new turn's title (FLIP, 650 ms ease/out) and the bar goes to Working |
| Turn 2 | Trace runs and folds; sentence word by word; the payments card reveals. Rows appear: opacity 0 → 1, y +8 → 0, 420 ms, 70 ms apart. After 350 ms, each row selects in turn 90 ms apart: checkbox fill 250 ms ease/standard, check pops (scale 0.4 → 1, 300 ms ease/pop) while the footer total climbs; it ends on "19 selected · $4,212.00"; 400 ms later the asks row reveals under the table |
| Turn 3 | Trace runs and stays visible; sentence fades in word by word; Prepared action appears and blooms; the bar goes to Landed and offers nothing further |

### 7.6 Approval
| Step | Spec |
|---|---|
| Open the sheet | "Review and retry" opens it. Scrim fades in 350 ms ease/standard. Desktop: the sheet rises from y +24 and scale 0.98 to rest, 550 ms ease/out. Mobile: it slides up from fully below the screen, 550 ms ease/out. The sheet does not pulse or animate once open |
| Not now, or click the scrim | Reverse |
| Retry 19 payments | Sheet closes; the prepared action switches to Approved; the Receipt appears (700 ms reveal); the progress bar fills to 21% over 2600 ms ease/out; "authenticated" counts 1, 2, 3, 4 and "recovered" adds $312.40, $189.00, $455.75, $98.20, one every 650 ms; the countdown runs down from 10:00 once a second |
| Cancel the rest | The countdown stops; the button becomes Disabled with "Stopped · $1,055.35 kept" |

### 7.7 Reduced motion
With reduced motion on, nothing animates: every state appears in its final form, the edge does not drift, the mark and spinner do not turn.

---

## 8. Screens to build

Build each as a full frame using only components. Desktop frames 1440 × 900 on `04 Screens · Desktop`; mobile frames 390 × 844 on `05 Screens · Mobile`. Name them exactly.

| # | Frame name | What it shows |
|---|---|---|
| 01 | `Home / Rest` | Page header, Hero figures card, Ask section, the three insight cards; no assist bar |
| 02 | `Home / Insight hover` | 01 with the Lead insight card in Hover |
| 03 | `Home / Typing` | Briefing dimmed to 30% except the ask section; Ask field Typing ("Why were payo") |
| 04 | `Home / Understood` | Ask field Understood with the scope line; Send Waiting for you |
| 05 | `Answer / Working` | Question title; trace with steps 1–2 Done and step 3 Running; assist bar Working |
| 06 | `Answer / Built` | Work summary (Steps Closed), answer sentence, Breakdown card, Chart card, Prepared action Waiting, Asks row; assist bar Suggesting "Show me the declined ones" |
| 07 | `Answer / Steps open` | 06 with Steps Open and the Query block |
| 08 | `Answer / Chart hover` | 06 scrolled to the chart with the tooltip on 9 Nov |
| 09 | `Drill / Built` | Turn 1's chips Answered; Turn 2: title, summary, sentence, Payments card with all rows selected, Asks row; assist bar Suggesting "Retry them" |
| 10 | `Retry / Prepared` | Turn 3: title, trace (both Done), sentence, Prepared action Waiting; assist bar Idle |
| 11 | `Retry / Confirm` | 10 with the scrim and the Confirm sheet |
| 12 | `Retry / Receipt` | Prepared action Approved; Receipt Running (4 of 19, $1,055.35, 9:58) |
| 13 | `Retry / Stopped` | Receipt Cancelled |

Mobile: build 01, 03, 04, 06, 09, 11 (bottom sheet) and 12.

Scroll positions: in 05–13 show the part of the conversation the person is looking at, as in the reference build (the latest turn in view, the assist bar pinned at the bottom). Make the view frame scroll vertically in the prototype.

---

## 9. Prototype (page `06 Prototype`)

Wire the duplicated screens as one flow. Use Smart Animate with the curves in §2.6 so the matching layers travel.

| From | Trigger | To | Transition |
|---|---|---|---|
| 01 Home / Rest | Click Ask field | 03 Home / Typing | Smart animate 500 ms ease/standard |
| 01 | Click Lead insight | 05 Answer / Working | Smart animate 650 ms ease/out |
| 01 | Click "Review and retry" on the Lead insight | 10 Retry / Prepared | Dissolve 450 ms |
| 01 | While hovering the Lead insight | 02 | Smart animate 250 ms ease/standard |
| 03 | After delay 2500 ms | 04 Home / Understood | Smart animate 500 ms ease/standard |
| 04 | Click Send | 05 Answer / Working | Smart animate 700 ms ease/out (name the ask field and the assist bar the same layer, `Assist`, so they travel; name the field text and the question title the same, `Question`) |
| 05 | After delay 3000 ms | 06 Answer / Built | Smart animate 700 ms ease/standard |
| 06 | Click Steps | 07 | Instant; 07 back to 06 the same way |
| 06 | Hover a chart bar | 08 | Instant |
| 06 | Click the "Show me the declined ones" chip, the assist bar, or key Tab | 09 Drill / Built | Smart animate 650 ms ease/out |
| 09 | Click the "Retry them" chip, the assist bar, or key Tab | 10 Retry / Prepared | Smart animate 650 ms ease/out |
| 06 or 10 | Click "Review and retry" | 11 Retry / Confirm | Open overlay: the sheet as an overlay, centred (desktop) or bottom (mobile), background overlay/scrim, Smart animate 550 ms ease/out |
| 11 | Click "Not now" or outside | Close overlay | 350 ms |
| 11 | Click "Retry 19 payments" | 12 Retry / Receipt | Smart animate 700 ms ease/standard |
| 12 | Click "Cancel the rest" | 13 Retry / Stopped | Smart animate 250 ms ease/standard |

Set the starting frame to 01. Turn off "Show hotspot hints on click"; nothing in the design shows hints.

---

## 10. Copy deck

Use these strings exactly, including typographic characters (− minus, · middle dot, ’ and “ ” quotes are as shown).

**App bar:** Halden Supply Co. · Home · Payments · Payouts · Disputes · Fri 21 Nov

**Home:** Your weekly briefing, 16–22 Nov · Sales were up this week. Payouts were $8,490.00 lower, and about half of that can be recovered today. · Paid out this week · $38,420.00 · −$8,490.00 · 18.1% below last week · Gross volume · $214,860.40 · +3.2% on last week · Payment success · 94.1% · −2.3 pts on last week · Open disputes · 3 · $2,640.00 held · Reply to this briefing, or ask about anything · Looking at payouts for 16–22 Nov, compared with 9–15 Nov

**Insights:** Payouts · $4,212.00 · can be recovered today. 19 payments were declined after Westmoor Bank started requiring 3-D Secure on 17 Nov. · Larch Hotel Group $731.10 · Norrow Bakehouse $455.75 · Orchard Lane Café $312.40 · 16 more customers $2,712.75 · Review and retry · Disputes · 4 days · until evidence is due on the first of 3 disputes. $2,640.00 is held until they're decided. · Next payout · $41,276.40 · arrives Monday 24 Nov, including $1,638.00 held over from the holiday.

**Turn 1:** Why were payouts lower this week? · (trace steps in §5.7) · Read payouts, balance transactions, disputes and Westmoor Bank's notice of 17 Nov · 4 steps · (query in §5.7) · Payouts were $38,420.00 this week, $8,490.00 less than last week. Three things explain all of it, and about half can be recovered today. · Waiting for your approval · Retry the 19 declined payments · $4,212.00 · each customer gets a short 3-D Secure prompt · Review and retry · Before you decide · Show me the declined ones · Paid out · 16–22 Nov · $38,420.00 · −$8,490.00 · 18.1% below 9–15 Nov · (cause rows in §5.9) · Payouts, last 8 weeks · dashed line: the 7-week average, $44,096 · green: what retrying recovers · $60k · $30k · $0 · 5 Oct · 19 Oct · 2 Nov · 16 Nov · $38,420 · +$4,212

**Assist bar:** Ask a follow-up · Show me the declined ones · Retry them · Tab

**Turn 2:** Show me the declined ones · Filtered the 19 declined payments and checked each card's bank · 2 steps · These are the 19 payments Westmoor Bank declined with “authentication required”. Every one can be retried, so I've selected them all. · Next · Retry them · All 19 · Over $200 · Repeat customers · Customer · Declined · Card · Amount · 11 more · $1,815.20 · 19 selected · $4,212.00

**Turn 3:** Retry them · It's prepared. Nothing is sent until you confirm. · Retry 19 payments from Westmoor Bank cardholders · $4,212.00 · settles to the payout on Mon 24 Nov · Approved by you at 4:12 pm

**Confirm sheet:** Confirm · Halden Supply Co. · Retry 19 declined payments · $4,212.00 · Customers · 19 Westmoor Bank cardholders · What they see · A 3-D Secure prompt by email · Settles to · Payout on Mon 24 Nov · Not now · Retry 19 payments · This sheet is part of the platform, not generated. Nothing is sent until you press Retry.

**Receipt:** Retry started · today, 4:12 pm · 19 customers have been sent a 3-D Secure prompt · 4 of 19 authenticated · $1,055.35 recovered so far · Cancel the rest · 9:58 · Stopped · $1,055.35 kept

---

## 11. Data (it all adds up; keep it consistent)

- Business: Halden Supply Co. Bank: Westmoor Bank. Today: Fri 21 Nov. Week: 16–22 Nov; last week 9–15 Nov.
- Paid out this week $38,420.00; last week $46,910.00; difference −$8,490.00 (−18.1%).
- The $8,490.00 gap: $4,212.00 declined after 3-D Secure (19 payments, recoverable, 49.6%) · $2,640.00 held by 3 disputes (31.1%) · $1,638.00 holiday timing (19.3%).
- Eight weeks of payouts: 28 Sep $41,200 · 5 Oct $43,880 · 12 Oct $42,350 · 19 Oct $44,910 · 26 Oct $45,300 · 2 Nov $44,120 · 9 Nov $46,910 · 16 Nov $38,420. Seven-week average $44,096. With the retry: $42,632. Chart scale $0 to $60,000; the hero strip's scale is $0 to $50,000.
- Gross volume $214,860.40 (+3.2%). Payment success 94.1% (−2.3 pts). Open disputes 3, $2,640.00 held.
- The 19 payments: the 8 rows in §5.13 total $2,396.80; the other 11 total $1,815.20; all 19 total $4,212.00. The three largest (Larch $731.10, Norrow $455.75, Orchard $312.40) total $1,499.25; the other 16 total $2,712.75.
- Next payouts: Mon 24 Nov $41,276.40 (includes $1,638.00 from the holiday) · Tue 25 Nov $6,912.75 · Wed 26 Nov $7,388.10.
- Receipt: 4 of 19 authenticated, $312.40 + $189.00 + $455.75 + $98.20 = $1,055.35 recovered.

---

## 12. Never do this

These are the defaults that make it look generic. If you catch yourself reaching for one, stop.

- A card around the assistant's words, the question title, the trace or the asks; or a composed object (figures, insight, breakdown, chart, table, receipt) left loose on the page without its card.
- The AI edge reduced to a glow: the 1.5px gradient border must show all the way round.
- Text smaller than 14px, anywhere, including axis labels, captions and table headers.
- A colour that is not a Design System Labs token, except the three flow colours on the AI flourish.
- A gradient anywhere but the AI edge, the mark and the spinner: no gradient backgrounds, washes, chart fills or brand tiles.
- Blue text links, underlined words as actions, "→" or "↗" arrows, "See more", "Learn more". Asks are chips; acts are buttons.
- A line or area chart through weekly or daily totals; bars that do not start at zero; labels drawn inside a scaled SVG; a progress-style bar whose length means nothing; chunky columns that fill their slot; a chart with no hover.
- An action or an ask placed above the evidence it depends on.
- The home ask field moved to the end of the page, or pinned or sticky: it sits between the figures and the insights, and the bar is pinned only in assist mode.
- The assistant's centred reading column on home.
- A sparkle or star icon for AI; a filled gradient dot; the word "Assistant" repeated beside every mark.
- The AI edge or a glow on anything that moves money: figures, the table, the sheet, the commit button.
- Pills or badges for status, coloured alert boxes, icons in rounded squares, avatars.
- All-caps labels, numbered eyebrows (01, 02), more than one label style per screen.
- Animated attention-seekers: pulsing buttons, bouncing hints, cards that lift or move on hover, anything that moves without a reason.
- A second commit button in view.
- Changing the copy, the numbers or the order of the flow.

---

## 13. Done checklist

- [ ] Collections `Primitives`, `Semantic`, `Layout`, `Motion` exist; no component uses a raw colour, size, radius or duration.
- [ ] All text uses the styles in §2.4; Proxima Nova 300/400/600 only; tabular figures on; nothing under 14.
- [ ] The effect styles are `Surface/Card`, `Surface/Card hover`, `Surface/Float` and the four AI glows; nothing has a one-off shadow.
- [ ] `AI/Edge` is one component with seven states and drifts in the prototype; it appears only on the ask field, the assist bar and the prepared action, and its gradient border is visible all the way round in every state but Off.
- [ ] Every component in §5 exists with every variant listed, built with auto layout; every "card" uses `Surface/Card`, and words never sit in a card.
- [ ] Every screen reads, then explains, then acts: on home the ask section sits between the figures and the insights and scrolls with the page; in every turn the prepared action and the asks row come after the evidence, just above the bar.
- [ ] Home is full width and left aligned on the three-column grid; only the conversation is centred.
- [ ] All 13 desktop and 7 mobile frames exist with the exact names in §8.
- [ ] The prototype in §9 runs from 01 to 13 using only the person's clicks (except 03 → 04 and 05 → 06, which stand in for typing and the assistant working), and the chips advance it as well as the bar.
- [ ] The Motion page has an annotated frame for every row in §7.
- [ ] Copy matches §10 character for character; numbers match §11.
- [ ] Nothing in §12 appears anywhere.
- [ ] Compare each frame side by side with the reference build at the same size; list any difference you could not resolve.
