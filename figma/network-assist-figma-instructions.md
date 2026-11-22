# Network Assist: instructions for building the agent experience in Figma

You are building the **Network Assist** agent experience in Figma: foundations, components with every state, the screens of one end-to-end flow, and its prototype. This file is the single source of truth. Every value here was measured from the live reference build; use them exactly.

- **Reference build (open it, click through it):** https://richhemsley3.github.io/network-assist/experience/ (refresh to start over; narrow the window below 600px for mobile)
- **Source:** https://github.com/richhemsley3/network-assist (`experience/index.html` is one self-contained file; its CSS is the ground truth if this file and the build ever disagree)

Scope is the agent experience only. Do not build the preview frame, chapter controls, device switch, notes or the direction boards that sit around the prototype.

---

## 0. How to work

1. **Never invent a value.** Every colour, size, radius, shadow, duration and string is below. If something you need is missing, measure it in the reference build or ask; do not guess and do not "improve" it.
2. **Build in this order:** variables → text styles → effect styles → icons → primitives (buttons, mark, AI edge) → components → screens → prototype. Nothing in a screen may use a raw value: every fill, stroke, text and effect must reference a variable or style.
3. **Auto layout everywhere.** Every component and every screen section is an auto-layout frame. Use the padding, gap and alignment given. Hug or fill as specified; fixed sizes only where a size is given.
4. **One component per concept, with variants for states.** Use component properties (variant, boolean, text, instance swap) as listed in each component's "Properties" line. Do not make separate components for states.
5. **Names are exact.** Use the names in this file for pages, collections, variables, styles, components, properties and layers.
6. **Two breakpoints only:** Desktop (design frame 1440 × 900) and Mobile (390 × 844). The layout switches at a container width of 600px. Desktop values first; mobile differences are listed under "Mobile".
7. **Report what Figma cannot do.** Some motion (word-by-word text, counting numbers, a line drawing itself, a gradient drifting forever) cannot be prototyped exactly in Figma. Build the closest Figma approximation given in §7 and also place the exact spec as an annotation on the Motion page.
8. **Finish with the checklist in §10.**

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

1. **The assistant's own words are violet** (`text/assistant` #230762): the briefing headline, answer sentences, insight sentences, its suggested next question. **Facts and figures are ink** (`text/primary` #151738), even inside its sentences.
2. **The assistant's mark is a small gradient ring** (12 × 12). It turns only while the assistant works. The word "Assistant" is not repeated next to it.
3. **The AI edge** (a gradient that drifts along the border, with a soft glow behind it) marks only what the assistant owns or is doing: the ask field, the assist bar, and an action it has prepared that waits for approval. Nothing else gets it.
4. **AI styling never touches money.** Figures, the payments table, the confirm sheet and the orange Retry button stay plain. Once the person approves, the prepared card loses its edge and glow.
5. **No text links and no arrows.** Next questions are offered inside the assist bar; secondary actions are tonal buttons; the query is a "steps" toggle.
6. **One orange button in view at a time**, and only on an action that moves money.
7. **Surfaces are material:** white, a 1px inner top highlight, a hairline, a soft two-step shadow, on a flat cool ground. No gradients in backgrounds.
8. **Status is a coloured word, never a pill:** good green, at-risk red, watch amber.
9. **Nothing moves the flow on except the person.** The experience waits at the ask field, at send, at each suggested next question and at the approval.

---

## 2. Foundations

### 2.1 Colour variables

Create a collection **`Primitives`** (one mode) and a collection **`Semantic`** (one mode, Light) whose variables alias the primitives. Components use only `Semantic`.

**Primitives**

| Variable | Value |
|---|---|
| `ink/900` | #151738 |
| `ink/650` | #525362 |
| `ink/550` | #696A75 |
| `ink/400` | #919199 |
| `grey/200` | #CCCCD3 |
| `grey/150` | #DDDDE3 |
| `grey/100` | #E6E7ED |
| `tone/050` | #F2F2F9 |
| `ground` | #F6F6FB |
| `white` | #FFFFFF |
| `violet` | #230762 |
| `burst/dark` | #3144B7 |
| `burst/light` | #9EAAFC |
| `blue/100` | #E5E6F9 |
| `heritage` | #FE6200 |
| `heritage/press` | #E55800 |
| `good` | #3F7A33 |
| `risk` | #C0392B |
| `watch` | #8A6A15 |
| `base/dark` | #000226 |
| `ink/a04` | #151738 at 4% |
| `ink/a05` | #151738 at 5% |
| `ink/a06` | #151738 at 6% |
| `ink/a07` | #151738 at 7% |
| `ink/a09` | #151738 at 9% |
| `ink/a10` | #151738 at 10% |
| `ink/a14` | #151738 at 14% |
| `scrim` | #000226 at 32% |

**Semantic**

| Variable | Alias | Used for |
|---|---|---|
| `text/primary` | ink/900 | figures, titles, data |
| `text/secondary` | ink/650 | supporting text, labels on cards |
| `text/caption` | ink/550 | captions, table headers, axis-adjacent text |
| `text/placeholder` | ink/400 | empty field text |
| `text/assistant` | violet | everything the assistant writes |
| `text/on-action` | white | text on the orange button |
| `text/positive` | good | gains, "recoverable" |
| `text/negative` | risk | drops |
| `text/watch` | watch | soonest deadline |
| `surface/ground` | ground | page background |
| `surface/card` | white | cards, fields, sheet |
| `surface/tint` | tone/050 | query block |
| `surface/selected-tint` | blue/100 | (reserved: selected day, active state) |
| `control/segment-track` | ink/a04 (app bar) · ink/a05 (tabs) | segmented control track |
| `control/tonal` | ink/a06 | tonal button |
| `control/tonal-hover` | ink/a10 | tonal button hover |
| `control/tonal-disabled` | ink/a04 | tonal button disabled |
| `control/steps` | ink/a05 | steps toggle |
| `control/steps-hover` | ink/a09 | steps toggle hover |
| `control/send-disabled` | grey/200 | send button with nothing to send |
| `control/send-ready` | ink/900 | send button ready, stop button |
| `control/send-hover` | burst/dark | send button hover while it is the next step |
| `action/primary` | heritage | orange button |
| `action/primary-hover` | heritage/press | orange button hover |
| `select/on` | burst/dark | checkbox selected |
| `line/hairline` | grey/100 | table rules, turn divider |
| `line/card-divider` | ink/a06 | rules inside cards (hero, cause rows) |
| `line/list-divider` | ink/a07 | customer list rows |
| `line/checkbox` | grey/200 | checkbox outline |
| `chart/line` | burst/dark | chart line, points, sparkline |
| `chart/line-start` | burst/light | start of the chart line gradient |
| `chart/grid` | grey/100 | chart grid (dotted) |
| `chart/average` | grey/200 | average line (dashed) |
| `bar/recoverable` | good | split segment 1, cause marker 1 |
| `bar/disputes` | grey/200 | split segment 2, cause marker 2 |
| `bar/holiday` | grey/150 | split segment 3, cause marker 3, progress track |
| `overlay/scrim` | scrim | behind the confirm sheet |

### 2.2 Gradients (paint styles)

Figma variables cannot hold gradients; make these **paint styles** in a folder `AI/` and `Chart/`.

| Style | Type | Stops |
|---|---|---|
| `AI/Flow` | Linear, horizontal | 0% #3144B7 · 18% #9EAAFC · 34% #FE6200 · 50% #9EAAFC · 66% #3144B7 · 82% #9EAAFC · 100% #FE6200. Apply to a layer **three times the width** of the element it decorates; only one third is visible at a time (see §4.2). |
| `AI/Mark` | Angular (conic) | 0% #3144B7 · 25% #9EAAFC · 50% #FE6200 · 75% #9EAAFC · 100% #3144B7 |
| `AI/Spinner` | Angular | 0% #3144B7 · 28% #9EAAFC · 57% #FE6200 · 85% #FE6200 at 0% |
| `Brand/Mark` | Linear 135° | 0% #3144B7 · 100% #230762 |
| `Chart/Area` | Linear, top to bottom | #3144B7 at 20% → #3144B7 at 0% |
| `Chart/Sparkline area` | Linear, top to bottom | #3144B7 at 22% → #3144B7 at 0% |
| `Chart/Line` | Linear, left to right | 0% #9EAAFC · 55% #3144B7 · 100% #3144B7 |
| `Chart/Recoverable band` | Linear, bottom to top | #3F7A33 at 5% → #3F7A33 at 30% |

### 2.3 Number variables

Collection **`Layout`**:

| Variable | Value | | Variable | Value |
|---|---|---|---|---|
| `space/2` | 2 | | `radius/card` | 16 |
| `space/4` | 4 | | `radius/field` | 14 |
| `space/6` | 6 | | `radius/query` | 12 |
| `space/8` | 8 | | `radius/segment-track` | 12 |
| `space/10` | 10 | | `radius/button` | 10 |
| `space/12` | 12 | | `radius/segment` | 9 |
| `space/14` | 14 | | `radius/tonal` | 9 |
| `space/16` | 16 | | `radius/steps` | 8 |
| `space/18` | 18 | | `radius/keycap` | 6 |
| `space/20` | 20 | | `radius/checkbox` | 5 |
| `space/22` | 22 | | `radius/sheet` | 24 |
| `space/24` | 24 | | `radius/bar` | 5 (split bar), 3 (progress) |
| `space/32` | 32 | | `radius/brand` | 7 |
| `width/content` | 936 (1000 with 32 side padding) | | `width/assist-bar` | 760 |
| `width/sheet` | 480 | | `breakpoint` | 600 |

### 2.4 Type

Typeface: **Figtree** (Google Fonts), weights **400 Regular** and **600 SemiBold** only. In production it is Proxima Nova; Figtree stands in. Numbers use **tabular figures** (OpenType `tnum`) everywhere. The only other face is a monospace for the query block (SF Mono, or Roboto Mono if SF Mono is unavailable).

Create these **text styles**. Letter spacing is given in px for the size shown (Figma: use percentage where noted).

| Style | Size / line height | Weight | Letter spacing | Default colour | Used for |
|---|---|---|---|---|---|
| `Display/Hero figure` | 48 / 48 | 600 | −1.92 (−4%) | text/primary | Home "Paid out this week" figure |
| `Display/Insight lead figure` | 40 / 40 | 600 | −1.4 (−3.5%) | text/positive | Payouts insight figure |
| `Display/Answer figure` | 40 / 42 | 600 | −1.4 (−3.5%) | text/primary | Answer "Paid out" figure |
| `Heading/Briefing` | 32 / 36 | 600 | −0.8 (−2.5%) | text/assistant | Briefing headline |
| `Heading/Sheet figure` | 32 / 34 | 600 | −0.96 (−3%) | text/primary | Amount on confirm sheet |
| `Heading/Question` | 28 / 32 | 600 | −0.62 (−2.2%) | text/primary | First question title |
| `Heading/Insight figure` | 28 / 31 | 600 | −0.7 (−2.5%) | text/primary | Disputes and Next payout figures |
| `Heading/Follow-up` | 22 / 25 | 600 | −0.48 (−2.2%) | text/primary | Follow-up question titles |
| `Heading/Side figure` | 20 / 26 | 600 | −0.4 (−2%) | text/primary | Hero side figures |
| `Heading/Sheet title` | 19 / 28.5 | 600 | −0.19 (−1%) | text/primary | Confirm sheet title |
| `Heading/Card title` | 17 / 21.25 | 600 | −0.17 (−1%) | text/primary | Prepared card and receipt titles |
| `Body/Lead` | 16 / 24.8 | 400 | 0 | text/assistant | Answer sentences (max width 636) |
| `Body/Default` | 15 / 22.5 | 400 | 0 | text/primary | Ask field, trace steps, delta lines |
| `Body/Default strong` | 15 / 22.5 | 600 | 0 | text/primary | App bar business name, step results, chart card title |
| `Body/Insight lead sentence` | 15 / 21.75 | 400 | 0 | text/assistant | Payouts insight sentence |
| `Body/Compact` | 14 / 21 | 400 | 0 | text/primary | Cause rows, table, sheet rows, receipt row, assist bar, app bar nav |
| `Body/Compact strong` | 14 / 21 | 600 | 0 | text/primary | Amounts, primary button, selected nav item |
| `Body/Insight sentence` | 14 / 20.3 | 400 | 0 | text/assistant | Disputes and Next payout sentences |
| `Label/Default` | 13 / 19.5 | 400 | 0 | text/caption | Labels, captions, table headers, scope line |
| `Label/Strong` | 13 / 19.5 | 600 | 0 | text/primary | Steps toggle, tonal button, segmented tab selected |
| `Label/Micro` | 12 / 18 | 400 | 0 | text/caption | Chart axes, tooltip |
| `Label/Micro strong` | 12 / 18 | 600 | 0 | text/secondary | Key cap, chart end labels |
| `Mono/Query` | 12.5 / 21.25 | 400 | 0 | text/secondary | Query block |

**Mobile** (container below 600): Briefing 24, Hero figure 36, Side figure 16, Insight lead figure 34, Insight figure 24, Question 22, Answer figure 32. Make these as separate styles with a `Mobile/` prefix (for example `Mobile/Heading/Briefing`).

### 2.5 Effect styles

| Style | Layers (top to bottom in Figma's list) |
|---|---|
| `Surface/Material` | Inner shadow 0, 1, blur 0, #FFFFFF 100% · Drop shadow 0, 0, blur 0, spread 1, ink 5% · Drop shadow 0, 1, blur 2, ink 5% · Drop shadow 0, 18, blur 40, spread −20, ink 18% |
| `Surface/Lift` (AI-owned surfaces) | Drop shadow 0, 1, blur 2, ink 5% · Drop shadow 0, 18, blur 40, spread −20, ink 22% |
| `Surface/Card hover` | Inner shadow 0, 1, blur 0, #FFFFFF · Drop shadow 0, 0, blur 0, spread 1, ink 6% · Drop shadow 0, 2, blur 4, ink 5% · Drop shadow 0, 24, blur 48, spread −20, ink 26% |
| `Surface/Sheet` | Drop shadow 0, 30, blur 80, spread −20, base/dark 45% |
| `Control/Segment selected` | Drop shadow 0, 1, blur 2, ink 8% · Drop shadow 0, 0, blur 0, spread 1, ink 4% |
| `Control/Primary button` | Inner shadow 0, 1, blur 0, #FFFFFF 25% · Drop shadow 0, 6, blur 14, spread −6, #FE6200 60% |
| `Control/Send` | Inner shadow 0, 1, blur 0, #FFFFFF 20% |
| `Control/Key cap` | Inner shadow 0, 0, blur 0, spread 1, ink 14% · Drop shadow 0, 1, blur 0, ink 8% |
| `Brand/Mark` | Inner shadow 0, 1, blur 0, #FFFFFF 35% |
| `AI/Glow · rest` | Layer blur 12 (on the glow layer, opacity 32%) |
| `AI/Glow · working` | Layer blur 13 (glow layer opacity 42%) |
| `AI/Glow · landed` | Layer blur 15 (glow layer opacity 55%) |
| `AI/Glow · prepared` | Layer blur 18 (glow layer opacity 18%) |

"ink" means #151738.

### 2.6 Motion tokens

Create a number collection **`Motion`** for durations (ms). Easing cannot be a variable; record the curves as text on the Motion page and use **Custom bezier** in prototype transitions.

| Easing | Curve | Feel |
|---|---|---|
| `ease/standard` | cubic-bezier(0.2, 0.8, 0.2, 1) | default for fades, slides, colour changes |
| `ease/out` | cubic-bezier(0.16, 1, 0.3, 1) | things arriving and settling: FLIP moves, bars growing, the sheet |
| `ease/pop` | cubic-bezier(0.34, 1.56, 0.64, 1) | small overshoot: dots, checks |
| `ease/pop-strong` | cubic-bezier(0.34, 1.7, 0.64, 1) | chart points popping |
| `ease/in-out` | ease-in-out | the AI edge drifting |
| `linear` | linear | spinners, the mark turning |

| Duration variable | ms | | Duration variable | ms |
|---|---|---|---|---|
| `dur/press` | 200 | | `dur/reveal` | 700 |
| `dur/hover` | 250 | | `dur/count` | 800 |
| `dur/toggle` | 300 | | `dur/glow` | 900 |
| `dur/scrim` | 350 | | `dur/figure-count` | 1100 |
| `dur/step` | 420 | | `dur/area` | 1500 |
| `dur/fade` | 450 | | `dur/line-draw` | 1900 |
| `dur/dim` | 500 | | `dur/bloom` | 2200 |
| `dur/sheet` | 550 | | `dur/halo` | 2400 |
| `dur/bar` | 600 | | `dur/progress` | 2600 |
| `dur/title-rise` | 650 | | `dur/drift` | 10000 |

---

## 3. Icons and marks

All icons are strokes (round caps and joins) inside a square frame named as below. Put them on `02 Icons and marks` as components.

| Component | Frame | Drawing |
|---|---|---|
| `Icon/Arrow up` | 14 × 14 | Path M7 12 V2 · M2.5 6.5 L7 2 L11.5 6.5. Stroke 2, white. |
| `Icon/Stop` | 10 × 10 | Rectangle 10 × 10, radius 2, fill white. |
| `Icon/Check` | 14 × 14 | Path M2.5 7.5 L5.5 10.5 L11.5 3.5. Stroke 1.8, text/caption. |
| `Icon/Checkbox check` | 11 × 11 (in 12 grid) | Path M2.5 6.5 L5 9 L9.5 3. Stroke 2.2, white. |
| `Icon/Plus` | 8 × 8 | Two bars 8 × 1.5, radius 1, crossing at the centre, fill current colour. Open state rotates the icon 45° to read as ×. |
| `Mark/Assistant` | 12 × 12 | A ring: ellipse 12 × 12 minus a centred ellipse 8 × 8 (Subtract). Fill `AI/Mark`. Variants `State=Rest` and `State=Working`; Working is identical but is rotated continuously (360° per 1600 ms, linear) in the prototype. |
| `Mark/Spinner` | 14 × 14 | Ellipse 14 × 14 minus centred ellipse 10 × 10. Fill `AI/Spinner`. Rotates 360° per 900 ms, linear. |
| `Mark/Brand` | 22 × 22 | Rectangle, radius 7, fill `Brand/Mark`, effect `Brand/Mark`. |
| `Mark/Legend` | 8 × 8 | Rectangle radius 2; fill set per use (bar/recoverable, bar/disputes, bar/holiday). |

---

## 4. Primitives

### 4.1 Buttons

**`Button/Primary`** (the orange money action)
- Auto layout horizontal, hug, height 36, padding 0 / 14, centred.
- Fill action/primary, radius 10, effect `Control/Primary button`.
- Text `Body/Compact strong`, text/on-action.
- Variants `State`: Default · Hover (fill action/primary-hover) · Disabled (not used; when the action is done the button is removed, not disabled).
- Property: `Label` (text). Labels used: "Review and retry", "Retry 19 payments".

**`Button/Tonal`** (secondary actions)
- Auto layout horizontal, hug, height 32, padding 0 / 12. Fill control/tonal, radius 9, no effect.
- Text `Label/Strong`, text/primary. No icon.
- Variants `State`: Default · Hover (fill control/tonal-hover) · Disabled (fill control/tonal-disabled, text text/caption).
- Properties: `Label` (text), `Countdown` (boolean; when on, the label is followed by " · " and a mm:ss value, for example "Cancel the rest · 9:58").

**`Button/Steps`** (shows the assistant's query)
- Auto layout horizontal, hug, height 26, padding 0 / 9, gap 7, centred. Fill control/steps, radius 8.
- Text `Label/Strong` ("4 steps"), then `Icon/Plus` (8 × 8).
- Variants `State`: Closed · Open (icon rotated 45°) · Hover (fill control/steps-hover). Boolean `Interactive`: off for the second turn, where it is plain text with no icon ("2 steps").

**`Button/Send`**
- 32 × 32, radius 10, centred content, effect `Control/Send`.
- Variants `State`: Empty (fill control/send-disabled, `Icon/Arrow up`) · Ready (fill control/send-ready, `Icon/Arrow up`) · Waiting for you (fill control/send-ready; Hover fill control/send-hover) · Working (fill control/send-ready, `Icon/Stop`).

**`Key cap`**
- Hug, height 22, padding 0 / 7, radius 6, no fill, effect `Control/Key cap`. Text `Label/Micro strong` "Tab".

**`Segmented`** (two uses: app bar navigation, table filter)
- Track: auto layout horizontal, padding 4, fill control/segment-track, radius 12. Gap 2 in the app bar, 3 in the table.
- Item: padding 6 / 12 (app bar) or 6 / 14 (table), radius 9. Unselected text `Body/Compact` (14) or `Label/Default` (13) in text/secondary. Selected: fill surface/card, effect `Control/Segment selected`, text 600 text/primary.
- Property `Selected` on each item instance.

**`Checkbox`**
- 17 × 17, radius 5. Variants `State`: Off (no fill, inner stroke 1.5 line/checkbox) · On (fill select/on, `Icon/Checkbox check` centred).

### 4.2 The AI edge (`AI/Edge`)

This is the assistant's signature. It is not a stroke style; it is two layers placed **behind** the content of the element it decorates. Build it once as a component and place it as the bottom two layers inside the ask field, the assist bar and the prepared card.

Structure, inside a frame the same size and radius as its host (`Clip content` off):

1. **`Glow`** (bottom). A rectangle inset from the host edges by the glow inset below, same corner radius, fill `AI/Flow`, layer blur and opacity per state. It spills softly outside the host.
2. **`Edge`**. The ring of colour itself:
   - a frame the size of the host with `Clip content` on, containing a rectangle **300% of the host's width** filled with `AI/Flow`;
   - masked by a **ring shape**: the host's rounded rectangle minus the same rectangle inset by the edge width (1.5 for fields and the bar, 1 for the prepared card) with its radius reduced by the same amount. Use Subtract, then use as mask.
   - Under the mask, add the host's fill (surface/card) as a separate inner rectangle so the field stays white.
3. The host's own content sits above both.

States (variant property `State` on `AI/Edge`):

| State | Glow inset (top/bottom, left/right) | Glow blur | Glow opacity | When |
|---|---|---|---|---|
| Rest | 3, 6 | 12 | 32% | Idle ask field and assist bar |
| Typing | 2, 4 | 13 | 42% | Home ask field while the person types |
| Working | 2, 4 | 13 | 42% | Assist bar while the assistant works |
| Landed | 0, 1 | 15 | 55% | Assist bar for 1.3 s when an answer finishes |
| Suggesting | 3, 6 | 12 | 45% | Assist bar while it offers the next question |
| Prepared | 8, 16 | 18 | 18% | Prepared action card; edge width 1 |
| Off | none | | 0% | Prepared card after approval (no edge, no glow) |

Motion: the 300%-wide gradient slides from x = 0 to x = −200% of the host width and back, 10 000 ms each way, ease-in-out, forever (both the edge strip and the glow strip move together). In Figma, make two variants of the strip position (`Drift=A`: x 0, `Drift=B`: x −200%) and loop them with After delay → Smart animate, 10 000 ms, ease-in-out. Glow changes between states use Smart animate 900 ms ease/standard (Landed comes in at 350 ms).

### 4.3 The assistant mark in context

Wherever the assistant is the author, put `Mark/Assistant` (12 × 12) before the text with a gap of 8: the briefing byline, the work summary line, inside the ask field and the assist bar, and the prepared card's label. While the assistant is working, every visible mark is `State=Working` (turning).

---

## 5. Components

Each entry: anatomy (outer to inner), layout, tokens, variants and properties, interactions, motion, mobile.

### 5.1 `App bar`
- Frame fill width, height 64, auto layout horizontal, padding 0 / 32, gap 24, align centre. No fill (sits on surface/ground). Not sticky-styled; it is the top row of the app.
- Children: `Brand` (auto layout, gap 10: `Mark/Brand` 22 + "Halden Supply Co." in `Body/Default strong`) · `Segmented` with items Home (selected), Payments, Payouts, Disputes · spacer (fill) · date "Fri 21 Nov" in `Label/Default`.
- Mobile: height 56, padding 6 / 20 / 0 / 20, the segmented navigation and the date are hidden.

### 5.2 `Briefing header`
- Auto layout vertical, gap 8, fill width.
- Byline: horizontal, gap 8: `Mark/Assistant` + "Your weekly briefing, 16–22 Nov" in `Label/Default` text/secondary.
- Headline: `Heading/Briefing` (text/assistant), max width 620 (fixed width, wraps to three lines): "Sales were up this week. Payouts were $8,490.00 lower, and about half of that can be recovered today."
- Mobile: headline `Mobile/Heading/Briefing` (24), fill width.

### 5.3 `Hero figure card`
- Card: fill surface/card, radius 16, effect `Surface/Material`, clip content on. Auto layout horizontal, no gap, fill width (936). Two columns at 1.3 : 1 (529 and 407).
- Left column: auto layout vertical, padding 18 / 20 / 0 / 20, gap 2.
  - "Paid out this week" `Label/Default` text/secondary.
  - "$38,420.00" `Display/Hero figure`, margins 8 above and 6 below (use spacers or padding).
  - Delta line `Body/Compact` text/secondary: "−$8,490.00" in text/negative, then " · 18.1% below last week".
  - Sparkline: fill width plus 20 each side (bleeds to the card's left edge and the column divider), height 72, top margin 12. Eight points from the payout series (§11), a 2px line in chart/line, area fill `Chart/Sparkline area`, no axes, no dots.
- Right column: vertical stack of three rows divided by 1px line/card-divider; a 1px line/card-divider on its left edge. Each row: auto layout vertical, padding 12 / 20, gap 2, centred vertically:
  - Label `Label/Default` text/secondary · Value `Heading/Side figure` · Note `Label/Default` text/caption with a coloured delta.
  - Rows: Gross volume / $214,860.40 / "+3.2%" (text/positive) " on last week" · Payment success / 94.1% / "−2.3 pts" (text/negative) " on last week" · Open disputes / 3 / "$2,640.00 held".
- Mobile: single column. Left column padding 20 / 20 / 0; figure `Mobile` 36. The three rows become one horizontal row of three equal cells divided by 1px line/card-divider, each padding 10 / 12, value 16.

### 5.4 `Ask field` (home)
- Frame fill width, height 56, radius 14, auto layout horizontal, padding 12 / 12 / 12 / 16, gap 12, align centre. Contains `AI/Edge` behind, effect `Surface/Lift`.
- Children: `Mark/Assistant` · text (fill, single line, truncate with ellipsis) · `Button/Send`.
- Under it, a **scope line** (absolute, 22px below the field's bottom edge area; reserve 22 of bottom padding in the section): `Label/Default` text/caption with the understood terms in 600 text/primary: "Looking at **payouts** for **16–22 Nov**, compared with **9–15 Nov**". Left inset 40 (aligns under the text).
- Variants `State`:
  | State | Text | Send | Edge | Scope line |
  |---|---|---|---|---|
  | Rest | "Reply to this briefing, or ask about anything" in text/placeholder | Empty | Rest | hidden |
  | Hover | same | Empty | Rest, glow 50% | hidden |
  | Typing | the typed text in text/primary with a 2 × 1.1em caret in text/assistant | Empty | Typing | hidden |
  | Understood | "Why were payouts lower this week?" with "payouts" and "this week" underlined (1.5px, text/assistant, offset at the baseline) | Waiting for you | Typing | shown |
- Interaction: click Rest → Typing (the rest of the briefing dims to 30%). Send is the only way on.
- Mobile: font 15, padding 12 / 10 / 12 / 16, scope line inset 16.

### 5.5 `Insight card`
- Card: fill surface/card, radius 16, effect `Surface/Material`. Auto layout vertical, gap 4.
- Variants `Type`:
  - **Lead** (Payouts): padding 20 / 22. Children in order:
    1. "Payouts" `Label/Default` text/secondary.
    2. "$4,212.00" `Display/Insight lead figure` (text/positive), 6 above.
    3. Sentence `Body/Insight lead sentence` (text/assistant), 4 above, max width 385: "can be recovered today. 19 payments were declined after Westmoor Bank started requiring 3-D Secure on 17 Nov."
    4. Customer list, 14 above and 16 below: four rows, each auto layout horizontal space-between, padding 8 / 0, top border 1px line/list-divider, `Body/Compact` text/primary with the amount in 600: Larch Hotel Group $731.10 · Norrow Bakehouse $455.75 · Orchard Lane Café $312.40 · and a last row in text/caption with the amount in 400: "16 more customers" $2,712.75.
    5. `Button/Primary` "Review and retry", left aligned, pushed to the bottom of the card.
  - **Standard** (Disputes, Next payout): padding 18 / 20. Children: label · flexible spacer (min 18) · figure `Heading/Insight figure` · sentence `Body/Insight sentence` (text/assistant), max width 360. The spacer pushes figure and sentence to the bottom so the card's space reads as deliberate.
    - Disputes: "Disputes" · "4 days" · "until evidence is due on the first of 3 disputes. $2,640.00 is held until they're decided."
    - Next payout: "Next payout" · "$41,276.40" · "arrives Monday 24 Nov, including $1,638.00 held over from the holiday."
- `State`: Default · Hover (moves up 2, effect `Surface/Card hover`). The whole card is the target; there are no links inside it. Clicking the Lead card asks "Why were payouts lower this week?"; its button goes straight to the prepared retry.
- Layout on home: a two-column grid, 531 and 393, gap 12. Lead spans both rows on the left; Disputes top right, Next payout bottom right; right cards stretch to the Lead's height.
- Mobile: one column, gap 12; Lead padding 22, figures 34 (lead) and 24.

### 5.6 `Question title`
- `Heading/Question` (first turn) or `Heading/Follow-up` (later turns), text/primary, fill width. It is the person's words, so ink, not violet.

### 5.7 `Work trace` and `Work summary`
**Trace step** (component `Trace step`): auto layout horizontal, padding 5 / 0, gap 10, fill width. Columns: icon 18 wide · text (fill) `Body/Default` · result `Body/Default strong`.
- Variants `State`: Running (icon `Mark/Spinner`, text text/secondary, result hidden) · Done (icon `Icon/Check`, text text/caption, result shown in text/primary).
- First turn steps (results in brackets): Reading payouts for 16–22 Nov ($38,420.00) · Comparing with 9–15 Nov (−$8,490.00) · Matching 214 balance transactions to the gap (3 causes) · Checking which declines can be retried (19 of 19).
- Second turn: Filtering declined payments since 17 Nov (19 payments) · Checking each decline reason and bank (All Westmoor Bank).
- Third turn: Preparing a retry with authentication for 19 payments ($4,212.00) · Checking your limits and approval rules (Within limits).

**Work summary** (component `Work summary`): replaces the trace when the work is done. Auto layout horizontal, wrap, gap 8 / 10, align centre. `Mark/Assistant` · `Label/Default` text/secondary sentence · `Button/Steps`.
- First turn: "Read payouts, balance transactions, disputes and Westmoor Bank's notice of 17 Nov" + "4 steps" (interactive).
- Second turn: "Filtered the 19 declined payments and checked each card's bank" + "2 steps" (not interactive).
- Third turn has no summary; its trace stays visible.

**Query block** (component `Query block`), shown under the summary when Steps is open: fill surface/tint, radius 12, padding 12 / 16, `Mono/Query` text/secondary, three lines:
```
payouts      where account = halden_supply and week in (2026-11-16, 2026-11-09)
balance_txn  where payout_week = 2026-11-16 group by cause
payments     where status = declined and decline_code = authentication_required
```

### 5.8 `Answer sentence`
- `Body/Lead`, text/assistant, max width 636. Figures inside are 600 text/primary. First turn: "Payouts were **$38,420.00** this week, $8,490.00 less than last week. Three things explain all of it, and about half can be recovered today." Second: "These are the 19 payments Westmoor Bank declined with "authentication required". Every one can be retried, so I've selected them all." Third: "It's prepared. Nothing is sent until you confirm."

### 5.9 `Breakdown card` (Paid out)
- Card: surface/card, radius 16, `Surface/Material`, padding 18 / 20, auto layout vertical, gap 6. Width 430 in the answer grid.
- "Paid out · 16–22 Nov" `Label/Default` text/caption · "$38,420.00" `Display/Answer figure` · "**−$8,490.00** · 18.1% below 9–15 Nov" `Body/Default` text/secondary with the amount 600 text/primary.
- Split bar, 12 above: height 10, auto layout horizontal gap 3, three segments radius 5: 49.6% bar/recoverable · 31.1% bar/disputes · 19.3% bar/holiday.
- Cause rows (`Cause row` component), 8 above the first: grid 10 | fill | hug, gap 12, padding 9 / 0, top border 1px line/card-divider. Marker `Mark/Legend` · title `Body/Compact` with a sub-line `Label/Default` text/caption · amount `Body/Compact strong`.
  - Declined after Westmoor Bank's 3-D Secure change / "19 payments since 17 Nov · " + "recoverable" in text/positive / $4,212.00 (marker bar/recoverable)
  - Disputes holding funds / "3 disputes · decisions due by 2 Dec" / $2,640.00 (bar/disputes)
  - Holiday payout moves to Mon 24 Nov / "Timing only · nothing is lost" / $1,638.00 (bar/holiday)

### 5.10 `Chart card` (Payouts, last 8 weeks)
- Card: as Breakdown card, width 494, gap 12. Header: auto layout space-between: "Payouts, last 8 weeks" `Body/Default strong` and "Dashed: what retrying would recover" `Label/Default` text/caption.
- Plot (build as vectors in a frame 454 × 218 for desktop; scale the drawing from a 520 × 250 design with plot margins left 40, right 112, top 16, bottom 28):
  - Y axis 30k to 50k. Grid lines at $30k, $40k, $50k: 1px chart/grid, dotted (dash 1, gap 3), extending 14 past the plot's right edge; labels `Label/Micro` right-aligned 10 left of the plot.
  - X labels at 5 Oct, 19 Oct, 2 Nov, 16 Nov (`Label/Micro`, centred under their points, baseline 4 above the bottom).
  - Area under the line: `Chart/Area`.
  - Average line at $44,096: 1.2px chart/average, dash 3 gap 4, across the plot; label "7-week average $44,096" `Label/Micro` 4 right of the start, 8 above the line.
  - The line: 2.25px, `Chart/Line`, round joins and caps, through the eight points in §11.
  - Points 1 to 7: circles r 3.25, fill white, 1.5px chart/line.
  - This week (point 8): circle r 5 chart/line, with a halo circle r 5 chart/line behind it (see motion).
  - Recoverable: a band 20 wide, radius 4, `Chart/Recoverable band`, from $38,420 up to $42,632, centred on point 8; a dashed line 1.5px text/positive (dash 2 gap 3) inside it; a circle r 4 white with 1.5px text/positive at $42,632.
  - End labels 14 right of point 8: "+$4,212 recoverable" `Label/Micro strong` text/positive at the top of the band; "$38,420" `Label/Micro strong` text/primary at the point.
- Hover state (component `Chart tooltip`): a vertical 1px text/primary dashed line (2/3) at the nearest week, and a tooltip: fill text/primary, radius 6, padding 10 horizontal, two lines `Label/Micro` white: "Week of 9 Nov" and "$46,910.00" in 600. Placed 54 above the point, kept inside the plot.

### 5.11 `Prepared action card`
- Card: surface/card, radius 16, padding 16 / 18, auto layout vertical, gap 10, effect `Surface/Lift`, with `AI/Edge` State=Prepared behind (edge width 1).
- Label row: `Mark/Assistant` + "Waiting for your approval" `Label/Default` text/caption, gap 8.
- Title `Heading/Card title`: "Retry the 19 declined payments" (first turn) or "Retry 19 payments from Westmoor Bank cardholders" (third turn).
- Row: auto layout horizontal space-between, wrap, gap 12 / 20: meta `Body/Compact` text/secondary ("**$4,212.00** · each customer gets a short 3-D Secure prompt", or "**$4,212.00** · settles to the payout on Mon 24 Nov") and `Button/Primary` "Review and retry".
- Variants `State`: Waiting (as above) · Approved (AI/Edge Off, effect `Surface/Material`, label "Approved by you at 4:12 pm" with no mark, the button removed).
- When it appears, its glow blooms once (see §7).

### 5.12 `Turn`
- A follow-up turn: auto layout vertical, gap 16, padding top 24, 14 above, top border 1px line/hairline, fill width. Contains: Question title (Follow-up) · trace or summary · answer sentence · the composed content.

### 5.13 `Payments card`
- Card: surface/card, radius 16, `Surface/Material`, padding 16 / 18 / 14, auto layout vertical, gap 14.
- `Segmented` (table variant): All 19 (selected) · Over $200 · Repeat customers.
- Table: header row `Label/Default` text/caption, padding bottom 8, bottom border 1px line/hairline: (checkbox column 30) · Customer · Declined · Card · Amount (right aligned). Body rows height 40, bottom border 1px line/hairline, `Body/Compact`: checkbox · name text/primary · date text/caption · "•••• 4471" text/caption · amount text/primary right-aligned. Cells have 12 right padding except the last.
- Rows (component `Payment row`, variants `Selected` On/Off): Orchard Lane Café · 17 Nov · 4471 · $312.40 / Pell & Daughters · 17 Nov · 0932 · $189.00 / Norrow Bakehouse · 18 Nov · 7710 · $455.75 / Kite Street Deli · 18 Nov · 2286 · $98.20 / Ashby Market · 19 Nov · 5103 · $260.00 / Larch Hotel Group · 19 Nov · 6648 · $731.10 / Fenwick Florists · 20 Nov · 3390 · $144.85 / Marlow Provisions · 21 Nov · 8821 · $205.50.
- Footer: space-between, `Body/Compact`: "11 more · $1,815.20" text/caption and "19 selected · $4,212.00" in 600 text/primary.
- Mobile: hide the Declined and Card columns.

### 5.14 `Confirm sheet` (platform-owned: no AI styling)
- Scrim: full screen overlay/scrim.
- Sheet: width 480 (desktop, centred) · fill surface/card · radius 24 · padding 22 / 24 / 24 · auto layout vertical, gap 12 · effect `Surface/Sheet`.
- Content: "Confirm · Halden Supply Co." `Label/Default` text/caption · "Retry 19 declined payments" `Heading/Sheet title` · "$4,212.00" `Heading/Sheet figure` · three rows (grid fill | hug, gap 12, padding 8 / 0, top border 1px line/hairline, `Body/Compact`, left cell text/caption): Customers / 19 Westmoor Bank cardholders · What they see / A 3-D Secure prompt by email · Settles to / Payout on Mon 24 Nov · actions right-aligned, gap 10: `Button/Tonal` "Not now", `Button/Primary` "Retry 19 payments" · note `Label/Default` text/caption: "This sheet is part of the platform, not generated. Nothing is sent until you press Retry."
- Mobile: full width, anchored to the bottom, radius 24 / 24 / 0 / 0.

### 5.15 `Receipt card`
- Card: surface/card, radius 16, `Surface/Material`, padding 16 / 18, auto layout vertical, gap 10.
- "Retry started · today, 4:12 pm" `Label/Default` text/caption · "19 customers have been sent a 3-D Secure prompt" `Heading/Card title` · progress bar height 6, radius 3, track bar/holiday, fill text/positive · row space-between, wrap, `Body/Compact` text/secondary: "**4 of 19** authenticated · **$1,055.35** recovered so far" (bold parts 600 text/primary) and `Button/Tonal` with Countdown on: "Cancel the rest · 9:58".
- Variants `State`: Running (progress 21% = 4 of 19) · Cancelled (tonal button Disabled, label "Stopped · $1,055.35 kept").

### 5.16 `Assist bar` (bottom)
- Container: fill width, padding 8 / 24 / 18, centred content. Hidden on home until the person sends a question from the ask field.
- Bar: width 760, radius 14, auto layout horizontal, padding 10 / 10 / 10 / 16, gap 12, align centre, `AI/Edge` behind, effect `Surface/Lift`, `Body/Compact`.
- Children: `Mark/Assistant` (2 left margin) · text (fill) · `Key cap` (only when suggesting) · `Button/Send`.
- Variants `State`:
  | State | Text | Key cap | Send | Edge | Mark |
  |---|---|---|---|---|---|
  | Idle | "Ask a follow-up" text/placeholder | off | Empty | Rest | Rest |
  | Working | "Ask a follow-up" text/placeholder | off | Working (stop) | Working | Working |
  | Landed (after the last turn only) | as Idle | off | Empty | Landed | Rest |
  | Suggesting | the suggested question in text/assistant ("Show me the declined ones", then "Retry them") | on | Empty | Suggesting | Rest |
  | Accepted | the same question in text/primary | off | Ready | Rest | Rest |
- Interaction: in Suggesting, clicking anywhere on the bar or pressing Tab accepts the suggestion.
- Mobile: container padding 8 / 12 / 26, bar fill width, Key cap hidden.

---

## 6. Layout of the two screens

### Home (briefing)
- App bar, then a scrolling view. Content column 1000 wide centred (936 inside 32 padding), top padding 20, bottom 32, vertical gap 16 between: Briefing header · Hero figure card · Ask section (Ask field + 22 reserved below for the scope line) · Insights grid.
- No assist bar on home. The briefing scrolls under where the bar will later appear.

### Answer (conversation)
- App bar, a scrolling view, and the Assist bar pinned to the bottom.
- Content column 1000 wide centred (936 inside 32), top padding 16, bottom 32, a grid of two columns 430 and 494 with gaps 16 (rows) and 12 (columns). Everything spans both columns except the Breakdown card (left) and the Chart card (right), which sit side by side.
- Order in the first turn: Question title · Work summary (after the trace folds) · Query block (only when open) · Answer sentence · Breakdown card + Chart card · Prepared action card. Then Turn 2 · Turn 3.

### Mobile
- Content padding 18, one column throughout, the same order.

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
| This week's chart point halo | scale 1 → 3.4 and opacity 35% → 0, 2400 ms ease/out, repeat, starts 300 ms after the chart finishes | Variant loop |

### 7.2 Home
| Step | Spec |
|---|---|
| Briefing loads | Insight cards rise: opacity 0 → 1 and y +10 → 0, 500 ms ease/standard, staggered 0 / 80 / 160 ms |
| Card hover | y −2 and effect `Surface/Card hover`, 250 ms ease/standard |
| Click the ask field | Everything on home except the ask section fades to 30% (500 ms ease/standard); the field's edge goes to Typing (900 ms); a caret appears |
| Typing | One character every 45–85 ms (spaces 90 ms). After the last character: "payouts" underline grows left to right 0 → 100% over 500 ms, then "this week" 220 ms later; the scope line fades up (opacity 0 → 1, y +4 → 0, 450 ms) 180 ms after that; Send becomes Waiting for you |
| Press send | The scope line fades out. At the same moment: the ask field travels to the assist bar's position at the bottom (FLIP, 700 ms ease/out, translating and scaling from its old box); the typed question travels into the Question title (FLIP, 650 ms ease/out, opacity 60% → 100%, scaling from the field's text size). The briefing is removed. 650 ms later the assist bar goes to Working |

### 7.3 The assistant working
| Step | Spec |
|---|---|
| Each trace step | Appears: opacity 0 → 1, y +6 → 0, 420 ms ease/standard. Runs for 700 ms (the third step of the first turn runs 950 ms). Then the spinner becomes the check, drawn along its path over 350 ms; 120 ms pause before the next step |
| Trace folds | 1400 ms after the last step: the trace collapses its height to 0 (550 ms ease/standard) and the Work summary fades in (450 ms) |
| Steps toggle | Click: the plus rotates 45° (300 ms ease/standard) and the Query block appears below; click again reverses |

### 7.4 The answer composing (first turn)
| Step | Spec |
|---|---|
| Answer sentence | Fades in word by word: each word opacity 0 → 1 over 700 ms, starting 28 ms after the previous word |
| Breakdown and Chart cards | Each card: opacity 0 → 1 and y +10 → 0, 700 ms ease/standard |
| Figure | Counts from $46,910.00 down to $38,420.00 over 1100 ms, ease-out (quartic) |
| Split bar | Segments grow from 0 width to their share, 600 ms ease/out each, starting 180 ms apart |
| Cause rows | Opacity 0 → 1 and y +10 → 0, 420 ms ease/standard, 110 ms apart |
| Chart | Grid lines and axis labels fade in (500 ms, 80 ms apart). Area grows up from the baseline (scale y 0 → 1, 1500 ms ease/out, 250 ms delay). The line draws left to right over 1900 ms (ease-out cubic) with a 5px chart/line dot leading it, glowing (0 0 6px chart/line 75%). Each weekly point pops (scale 0 → 1, 400 ms ease/pop-strong) as the line reaches it. The average line and its label fade in (600 ms, 900 ms delay). At the end: the leading dot fades, this week's point pops (450 ms ease/pop), the halo starts. 350 ms later the recoverable band grows up from $38,420 (900 ms ease/out), the dashed line fades in (400 ms, 500 ms delay), the recoverable dot pops (450 ms ease/pop, 700 ms delay), and its label counts from +$0 to +$4,212 over 800 ms (starting 500 ms in), then reads "+$4,212 recoverable" |
| Prepared action card | Opacity 0 → 1 and y +10 → 0, 700 ms; its glow blooms 18% → 60% → 18% over 2200 ms (peak at 35%), starting 300 ms after it appears |
| Answer finished | The assist bar switches straight to Suggesting with "Show me the declined ones": text in violet, Key cap shown, glow to 45% (900 ms ease/standard). Landed (glow 55% in 350 ms, back to Rest 1300 ms later) is used only when there is nothing to suggest, after the third turn |

### 7.5 Follow-ups
| Step | Spec |
|---|---|
| Accept a suggestion | Text turns from violet to ink and Send becomes Ready; 450 ms later the question travels from the bar into the new turn's title (FLIP, 650 ms ease/out) and the bar goes to Working |
| Payment rows (turn 2) | Rows appear: opacity 0 → 1, y +8 → 0, 420 ms, 70 ms apart. After 350 ms, each row selects in turn 90 ms apart: checkbox fill 250 ms ease/standard, check pops (scale 0.4 → 1, 300 ms ease/pop) while the footer total climbs; it ends on "19 selected · $4,212.00" |
| Turn 3 | Trace runs and stays visible; sentence fades in word by word; Prepared action card appears and blooms; the bar goes to Landed and offers nothing further |

### 7.6 Approval
| Step | Spec |
|---|---|
| Open the sheet | "Review and retry" opens it. Scrim fades in 350 ms ease/standard. Desktop: the sheet rises from y +24 and scale 0.98 to rest, 550 ms ease/out. Mobile: it slides up from fully below the screen, 550 ms ease/out. The sheet does not pulse or animate once open |
| Not now, or click the scrim | Reverse |
| Retry 19 payments | Sheet closes; the prepared card switches to Approved; the Receipt card appears (700 ms reveal); the progress bar fills to 21% over 2600 ms ease/out; "authenticated" counts 1, 2, 3, 4 and "recovered" adds $312.40, $189.00, $455.75, $98.20, one every 650 ms; the countdown runs down from 10:00 once a second |
| Cancel the rest | The countdown stops; the button becomes Disabled with "Stopped · $1,055.35 kept" |

### 7.7 Reduced motion
With reduced motion on, nothing animates: every state appears in its final form, the edge does not drift, the mark and spinner do not turn.

---

## 8. Screens to build

Build each as a full frame using only components. Desktop frames 1440 × 900 on `04 Screens · Desktop`; mobile frames 390 × 844 on `05 Screens · Mobile`. Name them exactly.

| # | Frame name | What it shows |
|---|---|---|
| 01 | `Home / Rest` | Briefing, Ask field Rest, insights; no assist bar |
| 02 | `Home / Card hover` | 01 with the Lead insight card in Hover |
| 03 | `Home / Typing` | Briefing dimmed to 30% except the ask section; Ask field Typing ("Why were payo") |
| 04 | `Home / Understood` | Ask field Understood with the scope line; Send Waiting for you |
| 05 | `Answer / Working` | Question title; trace with steps 1–2 Done and step 3 Running; assist bar Working |
| 06 | `Answer / Built` | Work summary (Steps Closed), answer sentence, Breakdown and Chart cards, Prepared card Waiting; assist bar Suggesting "Show me the declined ones" |
| 07 | `Answer / Steps open` | 06 with Steps Open and the Query block |
| 08 | `Answer / Chart hover` | 06 with the chart tooltip on 9 Nov |
| 09 | `Drill / Built` | Turn 2: title, summary, sentence, Payments card with all rows selected; assist bar Suggesting "Retry them" |
| 10 | `Retry / Prepared` | Turn 3: title, trace (both Done), sentence, Prepared card Waiting; assist bar Idle |
| 11 | `Retry / Confirm` | 10 with the scrim and the Confirm sheet |
| 12 | `Retry / Receipt` | Prepared card Approved; Receipt Running (4 of 19, $1,055.35, 9:58) |
| 13 | `Retry / Stopped` | Receipt Cancelled |

Mobile: build 01, 03, 04, 06, 09, 11 (bottom sheet) and 12.

Scroll positions: in 05–13 show the part of the conversation the person is looking at, as in the reference build (the latest turn in view, the assist bar pinned at the bottom). Make the view frame scroll vertically in the prototype.

---

## 9. Prototype (page `06 Prototype`)

Wire the duplicated screens as one flow. Use Smart Animate with the curves in §2.6 so the matching layers travel.

| From | Trigger | To | Transition |
|---|---|---|---|
| 01 Home / Rest | Click Ask field | 03 Home / Typing | Smart animate 500 ms ease/standard |
| 01 | Click Lead insight card | 05 Answer / Working | Smart animate 650 ms ease/out |
| 01 | Click "Review and retry" on the Lead card | 10 Retry / Prepared | Dissolve 450 ms |
| 01 | While hovering Lead card | 02 | Smart animate 250 ms ease/standard |
| 03 | After delay 2500 ms | 04 Home / Understood | Smart animate 500 ms ease/standard |
| 04 | Click Send | 05 Answer / Working | Smart animate 700 ms ease/out (name the ask field and the assist bar the same layer, `Assist`, so they travel; name the field text and the question title the same, `Question`) |
| 05 | After delay 3000 ms | 06 Answer / Built | Smart animate 700 ms ease/standard |
| 06 | Click Steps | 07 | Smart animate 300 ms ease/standard; 07 back to 06 the same way |
| 06 | Hover chart | 08 | Instant |
| 06 | Click assist bar, or key Tab | 09 Drill / Built | Smart animate 650 ms ease/out |
| 09 | Click assist bar, or key Tab | 10 Retry / Prepared | Smart animate 650 ms ease/out |
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

**Turn 1:** Why were payouts lower this week? · (trace steps in §5.7) · Read payouts, balance transactions, disputes and Westmoor Bank's notice of 17 Nov · 4 steps · (query in §5.7) · Payouts were $38,420.00 this week, $8,490.00 less than last week. Three things explain all of it, and about half can be recovered today. · Paid out · 16–22 Nov · $38,420.00 · −$8,490.00 · 18.1% below 9–15 Nov · (cause rows in §5.9) · Payouts, last 8 weeks · Dashed: what retrying would recover · 7-week average $44,096 · $38,420 · +$4,212 recoverable · Waiting for your approval · Retry the 19 declined payments · $4,212.00 · each customer gets a short 3-D Secure prompt · Review and retry

**Assist bar:** Ask a follow-up · Show me the declined ones · Retry them · Tab

**Turn 2:** Show me the declined ones · Filtered the 19 declined payments and checked each card's bank · 2 steps · These are the 19 payments Westmoor Bank declined with “authentication required”. Every one can be retried, so I've selected them all. · All 19 · Over $200 · Repeat customers · Customer · Declined · Card · Amount · 11 more · $1,815.20 · 19 selected · $4,212.00

**Turn 3:** Retry them · It's prepared. Nothing is sent until you confirm. · Retry 19 payments from Westmoor Bank cardholders · $4,212.00 · settles to the payout on Mon 24 Nov · Approved by you at 4:12 pm

**Confirm sheet:** Confirm · Halden Supply Co. · Retry 19 declined payments · $4,212.00 · Customers · 19 Westmoor Bank cardholders · What they see · A 3-D Secure prompt by email · Settles to · Payout on Mon 24 Nov · Not now · Retry 19 payments · This sheet is part of the platform, not generated. Nothing is sent until you press Retry.

**Receipt:** Retry started · today, 4:12 pm · 19 customers have been sent a 3-D Secure prompt · 4 of 19 authenticated · $1,055.35 recovered so far · Cancel the rest · 9:58 · Stopped · $1,055.35 kept

---

## 11. Data (it all adds up; keep it consistent)

- Business: Halden Supply Co. Bank: Westmoor Bank. Today: Fri 21 Nov. Week: 16–22 Nov; last week 9–15 Nov.
- Paid out this week $38,420.00; last week $46,910.00; difference −$8,490.00 (−18.1%).
- The $8,490.00 gap: $4,212.00 declined after 3-D Secure (19 payments, recoverable, 49.6%) · $2,640.00 held by 3 disputes (31.1%) · $1,638.00 holiday timing (19.3%).
- Eight weeks of payouts: 28 Sep $41,200 · 5 Oct $43,880 · 12 Oct $42,350 · 19 Oct $44,910 · 26 Oct $45,300 · 2 Nov $44,120 · 9 Nov $46,910 · 16 Nov $38,420. Seven-week average $44,096. With the retry: $42,632.
- Gross volume $214,860.40 (+3.2%). Payment success 94.1% (−2.3 pts). Open disputes 3, $2,640.00 held.
- The 19 payments: the 8 rows in §5.13 total $2,396.80; the other 11 total $1,815.20; all 19 total $4,212.00. The three largest (Larch $731.10, Norrow $455.75, Orchard $312.40) total $1,499.25; the other 16 total $2,712.75.
- Next payouts: Mon 24 Nov $41,276.40 (includes $1,638.00 from the holiday) · Tue 25 Nov $6,912.75 · Wed 26 Nov $7,388.10.
- Receipt: 4 of 19 authenticated, $312.40 + $189.00 + $455.75 + $98.20 = $1,055.35 recovered.

---

## 12. Never do this

These are the defaults that make it look generic. If you catch yourself reaching for one, stop.

- Blue text links, underlined words as actions, "→" or "↗" arrows, "See more", "Learn more".
- A row or list of suggested-question chips or links under an answer.
- A sparkle or star icon for AI; a filled gradient dot; the word "Assistant" repeated beside every mark.
- Gradient backgrounds, gradient hero bands, glows on money, the AI edge on anything that moves money.
- Pills or badges for status, coloured alert boxes, icons in rounded squares, avatars.
- Mini charts added to fill space; donut or gauge charts for things that are not parts of a whole.
- All-caps labels, numbered eyebrows (01, 02), more than one label style per screen.
- Animated attention-seekers: pulsing buttons, bouncing hints, anything that moves without a reason.
- A second orange button in view.
- Changing the copy, the numbers or the order of the flow.

---

## 13. Done checklist

- [ ] Collections `Primitives`, `Semantic`, `Layout`, `Motion` exist; no component uses a raw colour, size, radius or duration.
- [ ] All text uses the styles in §2.4; Figtree 400/600 only; tabular figures on.
- [ ] Effect styles in §2.5 exist and are applied; nothing has a one-off shadow.
- [ ] `AI/Edge` is one component with seven states and drifts in the prototype; it appears only on the ask field, the assist bar and the prepared card.
- [ ] Every component in §5 exists with every variant listed, built with auto layout.
- [ ] All 13 desktop and 7 mobile frames exist with the exact names in §8.
- [ ] The prototype in §9 runs from 01 to 13 using only the person's clicks (except 03 → 04 and 05 → 06, which stand in for typing and the assistant working).
- [ ] The Motion page has an annotated frame for every row in §7.
- [ ] Copy matches §10 character for character; numbers match §11.
- [ ] Nothing in §12 appears anywhere.
- [ ] Compare each frame side by side with the reference build at the same size; list any difference you could not resolve.
