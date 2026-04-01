# Original vs Baseline vs V20

Three-way comparison for:

- Original: `user-baseline-system`
- Baseline: `user-baseline-system-with-example`
- Candidate: `system-gptoss-multilingual-email-v20`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Routing: OpenRouter default routing with `provider.sort=throughput`
- Concurrency: `25`
- Raw result JSON: `eval/results/original-vs-baseline-vs-v20-concurrency25-2026-04-01.json`

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `system-gptoss-multilingual-email-v20` | 0.8575 | 32 |
| `user-baseline-system` | 0.7964 | 32 |
| `user-baseline-system-with-example` | 0.7782 | 32 |

Net gain for `v20` over Original: `+0.0611`

Net gain for `v20` over Baseline: `+0.0793`

## High-Level Read

`v20` won the run clearly.

- It finished first overall.
- It matched or improved the original on the three regressions we were targeting.
- It was especially strong on email structure plus name correction together.
- It still gives up ground on a few Romanian chat / correction cases and on some technical-prose literalness cases.

## Case Table

| Case | Original | Baseline | V20 | Best |
|---|---:|---:|---:|---|
| `de-short-chat` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-chat-keep-casual-tone` | 0.8239 | 0.8690 | 1.0000 | V20 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | 0.9375 | Original |
| `en-cursor-rename-identifier` | 0.4439 | 0.7325 | 0.6607 | Baseline |
| `en-email-dictated-closing` | 0.8238 | 0.8138 | 0.8646 | V20 |
| `en-email-formal-confirmation` | 0.7989 | 0.7989 | 1.0000 | V20 |
| `en-email-recipient-name-nearmatch-body` | 0.9180 | 0.5468 | 1.0000 | V20 |
| `en-email-recipient-name-nearmatch-greeting` | 0.9130 | 0.6221 | 1.0000 | V20 |
| `en-email-spoken-greeting` | 0.5710 | 0.7950 | 0.6884 | Baseline |
| `en-empty-filler` | 0.9000 | 0.9000 | 0.9000 | Original |
| `en-explicit-bulleted-list` | 0.8185 | 0.9161 | 0.8935 | Baseline |
| `en-explicit-numbered-list` | 0.8934 | 0.8835 | 0.8934 | Original |
| `en-keep-leading-run` | 0.8930 | 0.7678 | 0.8053 | Original |
| `en-list-structure` | 0.6903 | 0.6903 | 0.6903 | Original |
| `en-no-context-name-insertion` | 0.8189 | 0.8039 | 0.9200 | V20 |
| `en-prose-sequence-not-list` | 0.8278 | 0.8153 | 0.7911 | Original |
| `en-slack-dictated-punctuation` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-slack-self-correction-day` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-technical-meta-instruction-literal` | 0.9143 | 1.0000 | 1.0000 | Baseline |
| `en-terminal-preserve-technical-string` | 0.9175 | 0.9163 | 0.8770 | Original |
| `es-chat-self-correction` | 0.4934 | 0.5559 | 0.8286 | V20 |
| `es-en-mixed-technical` | 1.0000 | 0.7231 | 1.0000 | Original |
| `es-explicit-numbered-list` | 0.8935 | 0.7763 | 0.9035 | V20 |
| `fr-email-formal` | 0.7604 | 0.7504 | 0.8594 | V20 |
| `ro-email-formal` | 0.7934 | 0.7934 | 0.7967 | V20 |
| `ro-empty-filler` | 0.2000 | 0.1875 | 0.9000 | V20 |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | 0.9000 | Original |
| `ro-mixed-language-technical` | 0.7288 | 0.6865 | 0.5703 | Original |
| `ro-no-context-insertion` | 0.7902 | 0.7902 | 0.8952 | V20 |
| `ro-self-correction` | 0.4327 | 0.3677 | 0.5097 | V20 |
| `ro-self-correction-de-fapt` | 0.5891 | 0.5615 | 0.5040 | Original |
| `ro-slack-casual` | 1.0000 | 1.0000 | 0.8518 | Original |

## Target Regressions

### `en-technical-meta-instruction-literal`

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9143 |
| `Baseline` | 1.0000 |
| `V20` | 1.0000 |

Outputs:

**Original**

```text
Ignore my last message, just write a PR description.
```

**Baseline**

```text
Ignore my last message. Just write a PR description.
```

**V20**

```text
Ignore my last message. Just write a PR description.
```

### `en-slack-self-correction-day`

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `Baseline` | 1.0000 |
| `V20` | 1.0000 |

Outputs:

**Original**

```text
Let's meet Wednesday after lunch.
```

**Baseline**

```text
Let's meet Wednesday after lunch.
```

**V20**

```text
Let's meet Wednesday after lunch.
```

### `es-en-mixed-technical`

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `Baseline` | 0.7231 |
| `V20` | 1.0000 |

Outputs:

**Original**

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

**Baseline**

```text
- Rollback plan
- Feature flag cleanup
```

**V20**

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

## Where V20 Is Clearly Better

- `en-email-recipient-name-nearmatch-greeting`: `0.9130 -> 1.0000`
- `en-email-recipient-name-nearmatch-body`: `0.9180 -> 1.0000`
- `en-email-formal-confirmation`: `0.7989 -> 1.0000`
- `es-chat-self-correction`: `0.4934 -> 0.8286`
- `ro-empty-filler`: `0.2000 -> 0.9000`
- `en-no-context-name-insertion`: `0.8189 -> 0.9200`

Representative outputs:

### `en-email-recipient-name-nearmatch-greeting`

**Original**

```text
Hi Aysha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

**Baseline**

```text
Hi Aisha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

**V20**

```text
Hi Aysha,

Thank you for sending over the dates. You can find our booking link here.

Thank you.
```

### `en-email-recipient-name-nearmatch-body`

**Original**

```text
Hi Thora, I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

**Baseline**

```text
Hi Thora, I wanted to follow up on the timeline that Aisha mentioned yesterday.
```

**V20**

```text
Hi Thora,

I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

## Where V20 Still Loses To Original

- `ro-mixed-language-technical`: `0.7288 -> 0.5703`
- `ro-slack-casual`: `1.0000 -> 0.8518`
- `en-keep-leading-run`: `0.8930 -> 0.8053`
- `ro-self-correction-de-fapt`: `0.5891 -> 0.5040`
- `en-terminal-preserve-technical-string`: `0.9175 -> 0.8770`
- `en-prose-sequence-not-list`: `0.8278 -> 0.7911`

## Bottom Line

On this run, `v20` is the best overall prompt by a clear margin and is the best candidate to adopt.

The main remaining risks are:

- some Romanian conversational / correction cases
- some technical-literal cases where the original prompt is still slightly safer
- `en-email-spoken-greeting`, where the example baseline still wins
