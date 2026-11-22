# Network Assist

A creative-direction prototype for a language-first AI assistant inside a payments platform. The operator reads a weekly briefing, asks a question in plain language, and the assistant builds the answer from the payment records. Anything that moves money is prepared by the assistant and approved by a person on a fixed platform sheet.

**Agent experience:** https://richhemsley3.github.io/network-assist/experience/ (full window, no preview controls; refresh to start over)

**Prototype with notes:** https://richhemsley3.github.io/network-assist/

## What's here

| Page | What it shows |
|---|---|
| [Agent experience](https://richhemsley3.github.io/network-assist/experience/) | The assistant on its own, filling the browser window: no header, chapters, device switch or notes. Desktop or mobile layout follows the window width. Refresh to start the flow over. |
| [Prototype](https://richhemsley3.github.io/network-assist/) | The same experience inside a preview frame, with chapters, a desktop/mobile switch and notes on motion. It waits for you at each step: click the ask field, press send, accept each suggested follow-up (click it or press Tab), then review and approve the retry. Chapters jump to any step; desktop and mobile. |
| [AI element directions](https://richhemsley3.github.io/network-assist/directions/ai-elements.html) | Six treatments for the assistant's own elements: ring, edge light, spine, hard stops, wash and night. Ring was chosen. |
| [Layout comparison](https://richhemsley3.github.io/network-assist/directions/layouts.html) | An early comparison of three layouts: centred column, canvas with thread, and document. |
| [Figma instructions](figma/network-assist-figma-instructions.md) | Instructions for Claude to build the agent experience in Figma: tokens, type, effects, every component and state, motion, screens, prototype wiring, copy and data. |
| [Polish directions](https://richhemsley3.github.io/network-assist/directions/polish.html) | Editorial, Instrument, and Material and light. Material was chosen, without the background light. |

## The story

1. **Home.** The assistant's weekly briefing: a headline, the week's payout figure and trend, an ask field, and three insights, each one number and one sentence.
2. **Query.** The question rises into the title and the assistant's work runs in the open, one line per step.
3. **Build.** The answer composes: the figure, the gap split into its three causes, an eight-week chart that draws itself, and a prepared retry waiting for approval.
4. **Drill down.** The assistant proposes the next question in the bar; the 19 declined payments slide in and select themselves.
5. **Retry.** The retry is prepared and checked against limits. The confirm sheet carries no AI styling, because the commit belongs to the platform. After approval, a receipt shows customers authenticating, with ten minutes to cancel the rest.

## Design rules

- The assistant's own words are set in deep violet; facts and figures stay in ink.
- Its mark is a small gradient ring. Its edge is a gradient that drifts slowly along the border, with a soft glow that brightens while it works.
- AI styling never touches the money: the figures, the table, the confirm sheet and the Retry button stay plain.
- No text links and no arrows. The next question is offered inside the bar; secondary actions are tonal buttons.
- Surfaces are white with an inner highlight and a soft two-step shadow on a flat ground. Type is Figtree.

## Notes

Halden Supply Co., its customers and Westmoor Bank are fictional, and the figures add up. Each page is a single self-contained HTML file with no build step; fonts load from Google Fonts.
