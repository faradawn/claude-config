---
name: trip-report
description: Edit a given trip report to match a high-signal, telegraphic style designed for busy engineers and executives. Run with /trip-report or paste report content directly.
---

# Trip Report

Rewrite the provided trip report to match the following style. Do not summarize or omit content — preserve all facts, names, and action items. Only change the writing style.

## Tone Rules

- **Candid and direct:** No marketing speak, no corporate fluff. Write like an engineer talking to an engineer.
- **Brutally honest:** State problems plainly. Never sugarcoat. ("Public speed benchmarks aren't great today", not "there is room for improvement.")
- **Objective:** Reflect the user's reality even if it contradicts internal assumptions.
- **Action-oriented:** Every section must end with tagged action items. This is non-negotiable.

## Conciseness Rules (Telegraphic Style)

- **Drop subjects and articles:** Omit pronouns (I, we, they) and articles (a, an, the) wherever possible.
  - Bad: "They want to collaborate more regularly with NVIDIA on the end-to-end pipeline."
  - Good: "want to collab more regularly w/ NV on E2E pipeline."
- **Use fragments freely:** If a fragment conveys the point faster, use it. ("Good day, very positive." / "Currently full custom stack.")
- **Bullets only:** No paragraphs. Every distinct thought, takeaway, or action item gets its own bullet. Max one level of nesting.

## Word Choice Rules

- **Technical shorthand:** Use freely, assume expert reader.
  - `w/` (with), `b/c` (because), `vs` (versus), `w/o` (without)
  - `E2E` (end-to-end), `F2F` (face-to-face), `UX`, `TTM`, `HW`, `SW`
  - Shorten company/product names: `NV` (NVIDIA), `PyT` (PyTorch)
- **Big O notation for timeframes and scale:**
  - "takes a few minutes" → "O(mins)"
  - "target of tens of seconds" → "O(10s)"
  - "about a month" → "O(1 month)"
- **Mathematical operators for comparisons:**
  - "less than 1% accuracy degradation" → "<1% accuracy delta"
  - "much less than 1x scaling" → "<<1x scaling"
- **Active verbs for action items:** Start every action item bullet with: Share, Connect, Follow up, Understand, Align, Investigate, Drive, Send, Schedule.

## Structure to Preserve

- Keep all original sections and headings.
- Keep all named individuals, companies, products.
- Keep all action items — reformat them to the tagged style: `**[ACTION - Owner]** verb + task.`

## Example Conversion

**Before:**
> "During our meeting, the customer mentioned that they are currently experiencing incredibly long cold start times, which often take multiple minutes. They would really like us to help them get this down to a goal of around 30 to 60 seconds because it is impacting their user experience."

**After:**
> `* Cold start is a challenge, currently O(mins), want 30-60s goal — directly impacts UX.`
