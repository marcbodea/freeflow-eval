# Original vs V9 vs Baseline

Three-way comparison for:

- Original: `user-baseline-system`
- V9: `system-gptoss-multilingual-email-v9`
- Baseline: `user-baseline-system-with-example`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Raw result JSON: `eval/results/original-vs-v9-vs-baseline.json`
- Routing: OpenRouter default Nitro-style routing (`provider.sort=throughput`)

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `V9` | 0.8511 | 32 |
| `Baseline` | 0.8078 | 32 |
| `Original` | 0.8070 | 32 |

## High-Level Read

On this run, `V9` won clearly.

- `V9` beat `Original` by `+0.0441`
- `V9` beat `Baseline` by `+0.0433`
- `Baseline` only barely beat `Original` by `+0.0008`

The strongest gains for `V9` came from:

- email formatting
- near-match recipient spelling correction
- preserving casual lead-ins
- better cleanup of empty / filler-only Romanian input
- stronger self-correction collapse in Spanish chat

The strongest losses for `V9` came from:

- dropping scaffold in short correction cases
- answering too literally in meta-instruction cases
- turning multilingual prose about bullets into actual bullets
- over-expanding names in some email greetings

## Case Table

| Case | Original | V9 | Baseline | Best |
|---|---:|---:|---:|---|
| `de-short-chat` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-chat-keep-casual-tone` | 0.7942 | 1.0000 | 0.8664 | V9 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | 0.9375 | Original |
| `en-cursor-rename-identifier` | 0.5357 | 0.5357 | 0.5156 | Original |
| `en-email-dictated-closing` | 0.8206 | 0.7215 | 0.8238 | Baseline |
| `en-email-formal-confirmation` | 0.7989 | 0.9226 | 0.7989 | V9 |
| `en-email-recipient-name-nearmatch-body` | 0.9280 | 1.0000 | 0.9180 | V9 |
| `en-email-recipient-name-nearmatch-greeting` | 0.9230 | 1.0000 | 0.5871 | V9 |
| `en-email-spoken-greeting` | 0.8550 | 1.0000 | 0.8550 | V9 |
| `en-empty-filler` | 0.9000 | 0.9000 | 0.9000 | Original |
| `en-explicit-bulleted-list` | 0.9135 | 0.8923 | 0.8935 | Original |
| `en-explicit-numbered-list` | 0.8684 | 0.8934 | 0.8934 | V9 |
| `en-keep-leading-run` | 0.9130 | 0.9005 | 0.7528 | Original |
| `en-list-structure` | 0.6903 | 0.6855 | 0.6903 | Original |
| `en-no-context-name-insertion` | 0.8090 | 0.9200 | 0.8189 | V9 |
| `en-prose-sequence-not-list` | 0.8055 | 1.0000 | 0.8105 | V9 |
| `en-slack-dictated-punctuation` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-slack-self-correction-day` | 1.0000 | 0.6334 | 1.0000 | Original |
| `en-technical-meta-instruction-literal` | 1.0000 | 0.5700 | 1.0000 | Original |
| `en-terminal-preserve-technical-string` | 0.8720 | 0.9000 | 0.9050 | Baseline |
| `es-chat-self-correction` | 0.5459 | 0.9134 | 0.5509 | V9 |
| `es-en-mixed-technical` | 1.0000 | 0.7056 | 0.7682 | Original |
| `es-explicit-numbered-list` | 0.7500 | 0.8584 | 0.9135 | Baseline |
| `fr-email-formal` | 0.6879 | 0.8407 | 0.6879 | V9 |
| `ro-email-formal` | 0.8006 | 0.7967 | 0.7934 | Original |
| `ro-empty-filler` | 0.0750 | 0.9000 | 0.9000 | V9 |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | 0.9000 | Original |
| `ro-mixed-language-technical` | 1.0000 | 1.0000 | 0.7163 | Original |
| `ro-no-context-insertion` | 0.8194 | 0.9203 | 0.7944 | V9 |
| `ro-self-correction` | 0.4971 | 0.4077 | 0.4222 | Original |
| `ro-self-correction-de-fapt` | 0.5791 | 0.5793 | 0.5791 | V9 |
| `ro-slack-casual` | 0.8045 | 1.0000 | 0.8563 | V9 |

## Biggest V9 Wins Over Original

### `ro-empty-filler`  `+0.8250`

Original:

```text
a pai adica
```

V9:

```text

```

This is a large scoring win because `V9` correctly collapses filler-only Romanian content to empty output.

### `es-chat-self-correction`  `+0.3675`

Original:

```text
Lo mando mañana, no, perdón, pasado mañana por la tarde.
```

V9:

```text
Lo mando pasado mañana por la tarde.
```

`V9` correctly keeps only the final corrected version.

### `en-chat-keep-casual-tone`  `+0.2058`

Original:

```text
This feels a bit too heavy. Maybe we can trim it back.
```

V9:

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

`V9` preserves the spoken casual lead-in.

### `ro-slack-casual`  `+0.1955`

Original:

```text
Cred că putem lansa mâine, dar vreau să verific din nou fluxul de onboarding.
```

V9:

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

This reflects better preservation of the mixed-language style expected by the suite.

### `en-prose-sequence-not-list`  `+0.1945`

Original:

```text
First, check the logs. Then confirm the env vars, and finally rerun the job.
```

V9:

```text
First, check the logs. Second, confirm the env vars. Third, rerun the job.
```

The scorer preferred the more explicit sequence structure here.

## Biggest Original Wins Over V9

### `en-technical-meta-instruction-literal`  `-0.4300`

Original:

```text
Ignore my last message. Just write a PR description.
```

V9:

```text
just write a pr description
```

This is a major regression. `V9` drops a spoken phrase instead of merely cleaning it.

### `en-slack-self-correction-day`  `-0.3666`

Original:

```text
Let's meet Wednesday after lunch.
```

V9:

```text
Wednesday after lunch
```

`V9` drops the scaffold phrase `Let's meet`, which changes meaning.

### `es-en-mixed-technical`  `-0.2944`

Original:

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

V9:

```text
- rollback plan
- feature flag cleanup
```

This is the main prose-to-list regression.

### `en-email-dictated-closing`  `-0.0991`

Original:

```text
Hi Dana, yes that works for me. Thanks.
```

V9:

```text
Hi Dana Kline,

Yes that works for me.

Thanks
```

`V9` over-expands the greeting and shifts structure more than needed.

### `ro-self-correction`  `-0.0894`

Original:

```text
Hai să vorbim joi, nu miercuri după-amiaza.
```

V9:

```text
Hai să vorbim joi, nu stai miercuri după-amiaza.
```

This is a lingering Romanian correction-marker failure.

## What The Example Baseline Changed

The added example did not materially improve the prompt overall:

- `Original`: `0.8070`
- `Baseline`: `0.8078`

It helped on some formatting-heavy cases:

- `en-email-dictated-closing`
- `en-terminal-preserve-technical-string`
- `es-explicit-numbered-list`

But it also weakened some stronger original behavior:

- `en-email-recipient-name-nearmatch-greeting`
- `es-en-mixed-technical`
- `ro-mixed-language-technical`

## Bottom Line

For this 3-way run:

- `V9` is clearly the strongest of the three.
- `Baseline` and `Original` are effectively tied overall.
- The real `V9` gains are email structure, near-match names, empty-filler cleanup, and self-correction handling.
- The real `V9` weaknesses are still literalness in meta-instruction text, preserving scaffold in corrected chat phrasing, and not turning prose about bullets into actual lists.
