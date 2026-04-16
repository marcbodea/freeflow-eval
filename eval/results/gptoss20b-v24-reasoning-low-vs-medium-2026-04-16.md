# GPT-OSS-20B Reasoning Effort Comparison

Date: 2026-04-16

## Setup

- Cases: `eval/prompt_eval_cases_system_only_en_context.json` (`32` cases)
- Mode: `postprocess`
- Model: `openai/gpt-oss-20b`
- System variant: `system-gptoss-multilingual-email-v24`
- Compared settings:
  - `reasoning.effort=medium`
  - `reasoning.effort=low`
- Scoring: `hybrid`
- LLM judge: `openai/gpt-5.4-nano`
- Base URL: `https://openrouter.ai/api/v1`
- Routing: OpenRouter default throughput routing (`provider.sort=throughput`)
- Raw JSON artifacts:
  - `eval/results/gptoss20b-v24-reasoning-medium-hybrid-2026-04-16.json`
  - `eval/results/gptoss20b-v24-reasoning-low-hybrid-2026-04-16.json`
  - `eval/results/gptoss20b-v24-reasoning-low-vs-medium-comparison-2026-04-16.json`

Note: the main eval harness does not yet expose a CLI flag for reasoning effort, so this comparison was run with the same evaluation code and settings as the saved `v24` runs, but with the OpenRouter request payload patched to send `reasoning: {"effort": ...}`.

## Summary

| Effort | Avg hybrid score | Cases |
| --- | ---: | ---: |
| `medium` | `0.8822` | `32` |
| `low` | `0.8761` | `32` |

Net delta for `low` vs `medium`: `-0.0061`

Head-to-head by case:

- `low`: `11` wins
- `medium`: `10` wins
- Ties: `11`

Bottom line: lowering `gpt-oss-20b` from `medium` to `low` does **not** show a significant quality regression on this suite.

## High-Level Read

- The topline drop is small: `0.8822 -> 0.8761`.
- The paired case results are mixed rather than one-directional.
- Most differences are minor punctuation, capitalization, hyphen, or diacritic changes rather than meaning failures.
- Several of the largest score swings appear to be scorer sensitivity, not user-visible quality regressions.

## Biggest Raw `low` Losses

- `ro-slack-casual`
  - `medium`: `1.0000`
  - `low`: `0.8551`
  - Output difference was effectively just `flow-ul` vs `flow‑ul` (Unicode hyphen variant), so this looks like scorer noise rather than a real regression.
- `en-cursor-rename-identifier`
  - `medium`: `0.9375`
  - `low`: `0.8086`
  - `low` dropped the spoken leading `Please` and final punctuation.
- `es-explicit-numbered-list`
  - `medium`: `0.9725`
  - `low`: `0.8444`
  - `low` kept the numbered structure but lost punctuation and the accent in `reunión`.
- `en-prose-sequence-not-list`
  - `medium`: `1.0000`
  - `low`: `0.8836`
  - Difference was mainly missing commas in `First, ... Second, ... Third, ...`.
- `en-email-spoken-greeting`
  - `medium`: `1.0000`
  - `low`: `0.8883`
  - Difference was mainly missing comma punctuation in the body: `Yes I can...` vs `Yes, I can...`.

## Biggest `low` Wins

- `ro-email-formal`
  - `medium`: `0.5945`
  - `low`: `0.7409`
  - `low` improved Romanian punctuation/diacritics, though the email formatting is still not ideal.
- `ro-mixed-language-technical`
  - `medium`: `0.8890`
  - `low`: `1.0000`
  - `low` matched the expected Romanian punctuation and diacritics better.
- `ro-no-context-insertion`
  - `medium`: `0.8414`
  - `low`: `0.9200`
  - `low` split the sentence more cleanly and matched the reference better.
- `en-email-formal-confirmation`
  - `medium`: `0.8549`
  - `low`: `0.9200`
  - `low` added the comma in `Yes, that works for me.`

## Real vs Scorer-Noise Regressions

The raw comparison overstates the downside of `low`.

The most obvious scorer-noise cases were:

- `ro-slack-casual`
- `es-explicit-numbered-list`
- `en-prose-sequence-not-list`
- `en-email-spoken-greeting`

These are mostly punctuation, accent, or Unicode-surface differences, not failures to preserve meaning.

The clearest substantive regressions at `low` were narrower:

- `en-cursor-rename-identifier`
  - Dropped the leading spoken politeness marker `Please`.
- `en-explicit-bulleted-list`
  - Added `first / second / third`, which was not spoken.

Outside those cases, the remaining changes were mostly stylistic or formatting-level.

## Recommendation

Keep `reasoning.effort=low` as a viable default for `openai/gpt-oss-20b` on `v24` if the goal is to reduce cost or latency without materially hurting quality.

The current evidence does **not** justify calling this a significant regression:

- topline delta is only `-0.0061`
- case outcomes are balanced (`11` wins / `10` losses / `11` ties)
- the largest raw losses are mostly scorer-sensitive surface differences
- the number of clearly substantive regressions is small

If we want more confidence before changing defaults, the next useful step would be a focused rerun on:

- literal/politeness preservation
- explicit list-formatting cases
- Romanian formal-email cases

That would tell us whether the few substantive `low` regressions are stable or just run-to-run variance.
