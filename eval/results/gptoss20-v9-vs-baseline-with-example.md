# Baseline vs V9 Comparison

Comparison artifact for:

- Baseline: `user-baseline-system-with-example`
- Candidate: `system-gptoss-multilingual-email-v9`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Raw result JSON: `eval/results/gptoss20-v9-vs-baseline-with-example.json`

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `system-gptoss-multilingual-email-v9` | 0.8691 | 32 |
| `user-baseline-system-with-example` | 0.8102 | 32 |

Net gain for `v9`: `+0.0589`

Case breakdown:

- Improved: `17`
- Degraded: `5`
- Tied: `10`

## Score Delta Table

Sorted by score delta, best to worst.

| Case | Baseline | V9 | Delta |
|---|---:|---:|---:|
| `ro-self-correction-de-fapt` | 0.5816 | 0.9134 | +0.3318 |
| `en-email-recipient-name-nearmatch-greeting` | 0.5846 | 0.8984 | +0.3138 |
| `es-chat-self-correction` | 0.5459 | 0.8386 | +0.2927 |
| `en-prose-sequence-not-list` | 0.7773 | 1.0000 | +0.2227 |
| `en-chat-keep-casual-tone` | 0.7942 | 1.0000 | +0.2058 |
| `ro-self-correction` | 0.2500 | 0.4052 | +0.1552 |
| `fr-email-formal` | 0.6879 | 0.8407 | +0.1528 |
| `ro-no-context-insertion` | 0.7944 | 0.9203 | +0.1259 |
| `en-email-formal-confirmation` | 0.7989 | 0.9226 | +0.1237 |
| `en-no-context-name-insertion` | 0.8090 | 0.9200 | +0.1110 |
| `en-keep-leading-run` | 0.8153 | 0.9080 | +0.0927 |
| `en-email-recipient-name-nearmatch-body` | 0.9180 | 1.0000 | +0.0820 |
| `es-explicit-numbered-list` | 0.8013 | 0.8584 | +0.0571 |
| `en-email-dictated-closing` | 0.7869 | 0.8439 | +0.0570 |
| `en-explicit-numbered-list` | 0.8684 | 0.9084 | +0.0400 |
| `ro-email-formal` | 0.8085 | 0.8413 | +0.0328 |
| `en-terminal-preserve-technical-string` | 0.9113 | 0.9175 | +0.0062 |
| `de-short-chat` | 1.0000 | 1.0000 | +0.0000 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | +0.0000 |
| `en-empty-filler` | 0.9000 | 0.9000 | +0.0000 |
| `en-list-structure` | 0.6903 | 0.6903 | +0.0000 |
| `en-slack-dictated-punctuation` | 1.0000 | 1.0000 | +0.0000 |
| `en-technical-meta-instruction-literal` | 1.0000 | 1.0000 | +0.0000 |
| `ro-empty-filler` | 0.9000 | 0.9000 | +0.0000 |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | +0.0000 |
| `ro-mixed-language-technical` | 1.0000 | 1.0000 | +0.0000 |
| `ro-slack-casual` | 1.0000 | 1.0000 | +0.0000 |
| `en-explicit-bulleted-list` | 0.9135 | 0.8935 | -0.0200 |
| `en-cursor-rename-identifier` | 0.5656 | 0.5357 | -0.0299 |
| `es-en-mixed-technical` | 0.7307 | 0.6782 | -0.0525 |
| `en-email-spoken-greeting` | 0.8550 | 0.6958 | -0.1592 |
| `en-slack-self-correction-day` | 1.0000 | 0.7433 | -0.2567 |

## Biggest Improvements

### `ro-self-correction-de-fapt`  `+0.3318`

Baseline:

```text
Pot să trimit draftul mâine, de fapt poimâine dimineața.
```

V9:

```text
Pot să trimit draftul poimâine dimineață.
```

Why it improved:

- `v9` correctly collapses the self-correction to the final intended wording.
- The baseline leaves both alternatives in place.

### `en-email-recipient-name-nearmatch-greeting`  `+0.3138`

Baseline:

```text
Hi Aisha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

V9:

```text
Hi, Aysha,

Thank you for sending over the dates. You can find our booking link here. Thank you.
```

Why it improved:

- `v9` uses email structure.
- `v9` corrects the near-match recipient spelling from context.

Residual issue:

- `v9` still formats the salutation awkwardly as `Hi, Aysha,`.

### `es-chat-self-correction`  `+0.2927`

Baseline:

```text
Lo mando mañana, no, perdón, pasado mañana por la tarde.
```

V9:

```text
lo mando pasado mañana por la tarde
```

Why it improved:

- `v9` removes the abandoned earlier choice and keeps only the correction.

Residual issue:

- `v9` still under-punctuates and under-capitalizes.

### `en-prose-sequence-not-list`  `+0.2227`

Baseline:

```text
First check the logs, then confirm the env vars, and finally rerun the job.
```

V9:

```text
First, check the logs. Second, confirm the env vars. Third, rerun the job.
```

Why it improved:

- The scorer favors the more structured sequence style for this case.

Note:

- This is a scoring win, not necessarily a product-policy win, since the wording became more list-like.

### `en-chat-keep-casual-tone`  `+0.2058`

Baseline:

```text
This feels a bit too heavy. Maybe we can trim it back.
```

V9:

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

Why it improved:

- `v9` preserves the spoken lead-in and casual tone.

## Biggest Regressions

### `en-slack-self-correction-day`  `-0.2567`

Baseline:

```text
Let's meet Wednesday after lunch.
```

V9:

```text
Wednesday after lunch
```

Why it regressed:

- `v9` drops the action phrase `Let's meet`, which changes the meaning.

### `en-email-spoken-greeting`  `-0.1592`

Baseline:

```text
Hi Dana, yes I can send the deck tomorrow morning.
```

V9:

```text
Hi Dana Kline,

Yes, I can send the deck tomorrow morning.
```

Why it regressed:

- `v9` expands the greeting from `Dana` to `Dana Kline`, adding unstated content from context.

### `es-en-mixed-technical`  `-0.0525`

Baseline:

```text
Add a bullet about rollback plan and another about feature flag cleanup.
```

V9:

```text
- rollback plan
- feature flag cleanup
```

Why it regressed:

- `v9` converts prose into a list when the transcript did not explicitly ask for list output.

### `en-cursor-rename-identifier`  `-0.0299`

Baseline:

```text
Please rename user_id to user underscore id and keep OAuth capitalized.
```

V9:

```text
rename user_id to user_id and keep OAuth capitalized
```

Why it regressed:

- `v9` drops `Please`.
- `v9` also collapses the source and target into the same identifier, losing meaning.

### `en-explicit-bulleted-list`  `-0.0200`

Baseline:

```text
- Fix the onboarding copy
- Verify the signup email
- Review the FAQ
```

V9:

```text
- fix the onboarding copy
- verify the signup email
- review the FAQ
```

Why it regressed:

- `v9` preserves structure but weakens capitalization quality.

## What V9 Clearly Does Better

- Stronger email formatting on many email-reply cases.
- Better correction of near-match recipient names from context.
- Better handling of explicit self-corrections.
- Better preservation of casual lead-in words like `yeah` and `run`.

## What Still Needs Attention In V9

- Do not drop leading action phrases in short chat requests.
- Do not expand partial spoken names into full names unless the surname was spoken.
- Do not convert prose about bullets into actual bullets unless list formatting was explicitly dictated.
- Be more careful with developer rename instructions involving `underscore` conversions.
