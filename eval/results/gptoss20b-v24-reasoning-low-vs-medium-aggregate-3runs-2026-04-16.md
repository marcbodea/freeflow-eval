# GPT-OSS-20B Reasoning Effort Comparison (3 Runs)

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

Run artifacts:

- Run 1:
  - `eval/results/gptoss20b-v24-reasoning-medium-hybrid-2026-04-16.json`
  - `eval/results/gptoss20b-v24-reasoning-low-hybrid-2026-04-16.json`
  - `eval/results/gptoss20b-v24-reasoning-low-vs-medium-comparison-2026-04-16.json`
- Run 2:
  - `eval/results/gptoss20b-v24-reasoning-medium-hybrid-2026-04-16-run2.json`
  - `eval/results/gptoss20b-v24-reasoning-low-hybrid-2026-04-16-run2.json`
  - `eval/results/gptoss20b-v24-reasoning-low-vs-medium-comparison-2026-04-16-run2.json`
- Run 3:
  - `eval/results/gptoss20b-v24-reasoning-medium-hybrid-2026-04-16-run3.json`
  - `eval/results/gptoss20b-v24-reasoning-low-hybrid-2026-04-16-run3.json`
  - `eval/results/gptoss20b-v24-reasoning-low-vs-medium-comparison-2026-04-16-run3.json`

Note: the main eval harness does not yet expose a CLI flag for reasoning effort, so these runs used the same evaluation flow and settings as the saved `v24` runs, but with the OpenRouter request payload patched to send `reasoning: {"effort": ...}`.

## Per-Run Results

| Run | `medium` avg | `low` avg | `low - medium` | `low` wins | `medium` wins | Ties |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| `run1` | `0.8822` | `0.8761` | `-0.0061` | `11` | `10` | `11` |
| `run2` | `0.8654` | `0.8830` | `+0.0176` | `13` | `9` | `10` |
| `run3` | `0.8667` | `0.8852` | `+0.0185` | `15` | `8` | `9` |

## Aggregate Summary

| Effort | Mean avg hybrid score | Std dev |
| --- | ---: | ---: |
| `medium` | `0.8714` | `0.0093` |
| `low` | `0.8814` | `0.0047` |

Aggregate mean delta for `low` vs `medium`: `+0.0100`

Aggregate case totals across all `96` paired case comparisons:

- `low`: `39` wins
- `medium`: `27` wins
- Ties: `30`

## High-Level Read

- Across three runs, `low` is slightly better on average than `medium`, not worse.
- The observed effect is still small in absolute terms: `+0.0100` on a `0..1` score scale.
- Run-to-run variance remains meaningful. `run1` slightly favored `medium`, while `run2` and `run3` favored `low`.
- The variance in the per-run delta (`std dev = 0.0140`) is larger than the mean advantage itself, so the right conclusion is not that `low` is decisively better. The safer conclusion is:
  - there is no meaningful regression from lowering reasoning effort
  - the aggregate evidence now leans slightly in favor of `low`

## Most Consistent `low` Regressions

These were the strongest repeated downside cases for `low` across all three runs.

- `en-prose-sequence-not-list`
  - Mean delta: `-0.1075`
  - `medium` consistently kept the comma structure: `First, ... Second, ... Third, ...`
  - `low` consistently dropped those commas.
- `en-email-spoken-greeting`
  - Mean delta: `-0.1044`
  - `medium`: `Yes, I can send the deck tomorrow morning.`
  - `low`: `Yes I can send the deck tomorrow morning.`
- `en-cursor-rename-identifier`
  - Mean delta: `-0.0935`
  - `medium` usually preserved `Please`; `low` consistently dropped it.
- `en-explicit-bulleted-list`
  - Mean delta: `-0.0326`
  - `low` sometimes added `first / second / third`, which was not spoken.

Interpretation:

- These are real but narrow regressions.
- They are mostly about punctuation discipline, literal wording preservation, and list realization, not broad meaning failures.

## Most Consistent `low` Improvements

These were the strongest repeated upside cases for `low`.

- `ro-no-context-insertion`
  - Mean delta: `+0.0803`
  - `low` consistently matched the expected sentence split better.
- `ro-mixed-language-technical`
  - Mean delta: `+0.0981`
  - `low` consistently improved Romanian punctuation and diacritics.
- `es-en-mixed-technical`
  - Mean delta: `+0.1875`
  - In two of the three runs, `medium` converted the dictated prose intent into bullets while `low` preserved the intended prose form.
- `en-email-formal-confirmation`
  - Mean delta: `+0.0659`
  - `low` consistently added the expected comma in `Yes, that works for me.`

## Scorer-Noise Caveat

Some single-run losses from the earlier write-up did not repeat and look more like scorer sensitivity than real quality differences.

The clearest example was:

- `ro-slack-casual`
  - Run 1 heavily penalized a `flow-ul` vs `flow‑ul` surface difference.
  - Runs 2 and 3 tied on the same case.

This matters because the three-run aggregate is more trustworthy than the initial one-off comparison for small deltas.

## Recommendation

Based on the three-run aggregate, `reasoning.effort=low` looks safe for `openai/gpt-oss-20b` on `v24`.

Recommended conclusion:

- Do **not** treat lowering reasoning effort from `medium` to `low` as a quality regression on this eval suite.
- If anything, the aggregate results lean slightly toward `low`, but not by a large enough margin to call it a decisive quality improvement.
- The main reason to switch to `low` should still be expected cost and latency savings, with the current eval evidence showing no meaningful quality downside.

If we want higher confidence before changing the default, the next targeted check should focus on:

- prose-vs-list realization
- spoken politeness markers like `please`
- email punctuation discipline
- mixed-language technical prose cases
