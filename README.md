# Network Assist

A creative-direction prototype for a language-first AI assistant inside a payments platform. The operator reads a weekly briefing, asks a question in plain language, and the assistant builds the answer from the payment records. Anything that moves money is prepared by the assistant and approved by a person on a fixed platform sheet.

**Agent experience:** https://richhemsley3.github.io/network-assist/experience/ (full window, no preview controls; refresh to start over)

**Prototype with notes:** https://richhemsley3.github.io/network-assist/

## What's here

| Page | What it shows |
|---|---|
| [Agent experience](https://richhemsley3.github.io/network-assist/experience/) | The assistant on its own, filling the browser window: no header, chapters, device switch or notes. Desktop or mobile layout follows the window width. Refresh to start the flow over. Since November 23, 2026 it uses the Brightline Onboarding visual language (below); the prototype with notes keeps the earlier Material look. |
| [Prototype](https://richhemsley3.github.io/network-assist/) | The same experience inside a preview frame, with chapters, a desktop/mobile switch and notes on motion. It waits for you at each step: click the ask field, press send, accept each suggested follow-up (click it or press Tab), then review and approve the retry. Chapters jump to any step; desktop and mobile. |
| [AI element directions](https://richhemsley3.github.io/network-assist/directions/ai-elements.html) | Six treatments for the assistant's own elements: ring, edge light, spine, hard stops, wash and night. Ring was chosen. |
| [Layout comparison](https://richhemsley3.github.io/network-assist/directions/layouts.html) | An early comparison of three layouts: centred column, canvas with thread, and document. |
| [Figma instructions](figma/network-assist-figma-instructions.md) | Instructions for Claude to build the agent experience in Figma: tokens, type, effects, every component and state, motion, screens, prototype wiring, copy and data. Version 2 follows the agent experience as it is now. |
| [Polish directions](https://richhemsley3.github.io/network-assist/directions/polish.html) | Editorial, Instrument, and Material and light. Material was chosen, without the background light. |

## The story

1. **Home.** The assistant's weekly briefing: a headline, the week's payout figure and trend, an ask field, and three insights, each one number and one sentence.
2. **Query.** The question rises into the title and the assistant's work runs in the open, one line per step.
3. **Build.** The answer composes: the prepared retry waiting for approval and the next question first, then the figure, the gap split into its three causes, and eight weeks of payouts as bars from zero with the recoverable amount rising on this week.
4. **Drill down.** The assistant proposes the next question as a chip and in the bar; the 19 declined payments slide in and select themselves.
5. **Retry.** The retry is prepared and checked against limits. The confirm sheet carries no AI styling, because the commit belongs to the platform. After approval, a receipt shows customers authenticating, with ten minutes to cancel the rest.

## Design rules (agent experience)

- Colour is the Design System Labs palette, applied by its semantic tokens, on a white page. The assistant speaks in the system's info blue; facts and figures stay in ink.
- Type is Proxima Nova, light for large figures. Nothing is smaller than 14px, and spacing sits on an 8px grid.
- Only what waits on the person or floats is a surface: the prepared action, the ask field and bar, the confirm sheet. The briefing, figures, charts, tables and receipts sit on the page; turns are divided by one rule.
- The AI flourish is the multicolour flow: a gradient of blue, periwinkle and orange drifting along the edge of the ask field, the bar and the prepared action, with a soft glow, plus the ring mark and the spinner. It never touches money.
- Asks to the assistant are chips; acts are buttons, with one navy commit in view. No text links and no arrows.
- In each answer the action comes before the evidence. Charts are bars from zero in the system's chart series, with labels at true size.

## Notes

Halden Supply Co., its customers and Westmoor Bank are fictional, and the figures add up. Each page is a single self-contained HTML file with no build step. The earlier pages load Figtree from Google Fonts. The agent experience names Proxima Nova, a licensed typeface that is not included here; where it is not installed, the system sans is used.
