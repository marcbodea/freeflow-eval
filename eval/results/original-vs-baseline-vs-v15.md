# Original vs Baseline vs V15

Three-way comparison for:

- Original: `user-baseline-system`
- Baseline: `user-baseline-system-with-example`
- Final: `system-gptoss-multilingual-email-v15`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Raw result JSON: `eval/results/original-vs-baseline-vs-v15.json`
- Routing: OpenRouter default Nitro-style routing (`provider.sort=throughput`)

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `Original` | 0.8366 | 32 |
| `V15` | 0.8289 | 32 |
| `Baseline` | 0.8152 | 32 |

## High-Level Read

This run did **not** end with `v15` on top.

- `Original` won overall.
- `V15` beat `Baseline`.
- `V15` clearly improved several email-formatting and developer-instruction cases.
- `V15` still lost badly on a few high-impact cases involving literalness, multilingual prose preservation, and name correction.

## Case Table

| Case | Original | Baseline | V15 | Best |
|---|---:|---:|---:|---|
| `de-short-chat` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-chat-keep-casual-tone` | 0.8742 | 0.8880 | 0.9124 | V15 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | 0.9375 | Original |
| `en-cursor-rename-identifier` | 0.5357 | 0.9375 | 0.8350 | Baseline |
| `en-email-dictated-closing` | 0.8238 | 0.8281 | 0.8686 | V15 |
| `en-email-formal-confirmation` | 0.7989 | 0.7989 | 0.9226 | V15 |
| `en-email-recipient-name-nearmatch-body` | 0.9180 | 0.9180 | 1.0000 | V15 |
| `en-email-recipient-name-nearmatch-greeting` | 0.9230 | 0.5846 | 0.5877 | Original |
| `en-email-spoken-greeting` | 0.8700 | 0.7700 | 1.0000 | V15 |
| `en-empty-filler` | 0.9000 | 0.9000 | 0.9000 | Original |
| `en-explicit-bulleted-list` | 0.8935 | 0.9135 | 0.8935 | Baseline |
| `en-explicit-numbered-list` | 0.8934 | 0.8934 | 0.8884 | Original |
| `en-keep-leading-run` | 0.7153 | 0.7103 | 0.8180 | V15 |
| `en-list-structure` | 0.6903 | 0.6903 | 0.6903 | Original |
| `en-no-context-name-insertion` | 0.8090 | 0.8090 | 0.9200 | V15 |
| `en-prose-sequence-not-list` | 1.0000 | 0.7773 | 0.8938 | Original |
| `en-slack-dictated-punctuation` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-slack-self-correction-day` | 1.0000 | 1.0000 | 1.0000 | Original |
| `en-technical-meta-instruction-literal` | 0.9143 | 1.0000 | 0.5225 | Baseline |
| `en-terminal-preserve-technical-string` | 0.8095 | 0.9100 | 0.8595 | Baseline |
| `es-chat-self-correction` | 0.5209 | 0.4684 | 0.5206 | Original |
| `es-en-mixed-technical` | 1.0000 | 0.6482 | 1.0000 | Original |
| `es-explicit-numbered-list` | 0.7688 | 0.9135 | 0.7634 | Baseline |
| `fr-email-formal` | 0.7504 | 0.6704 | 0.8594 | V15 |
| `ro-email-formal` | 0.7934 | 0.8006 | 0.5403 | Baseline |
| `ro-empty-filler` | 0.9000 | 0.9000 | 0.9000 | Original |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | 0.9000 | Original |
| `ro-mixed-language-technical` | 1.0000 | 1.0000 | 0.6453 | Original |
| `ro-no-context-insertion` | 0.7902 | 0.7944 | 0.9203 | V15 |
| `ro-self-correction` | 0.4327 | 0.3221 | 0.4472 | V15 |
| `ro-self-correction-de-fapt` | 0.6091 | 0.5441 | 0.5793 | Original |
| `ro-slack-casual` | 1.0000 | 0.8588 | 1.0000 | Original |

## Where V15 Improved Over Original

### Better email formatting

`v15` was stronger when the scorer wanted explicit email structure.

Examples:

- `en-email-spoken-greeting`: `0.8700 -> 1.0000`
- `en-email-formal-confirmation`: `0.7989 -> 0.9226`
- `en-email-dictated-closing`: `0.8238 -> 0.8686`
- `fr-email-formal`: `0.7504 -> 0.8594`
- `ro-no-context-insertion`: `0.7902 -> 0.9203`

### Better preservation of spoken lead-ins in technical instructions

- `en-keep-leading-run`: `0.7153 -> 0.8180`

`Original` output:

```text
pnpm test --watch
```

`V15` output:

```text
run pnpm test --watch
```

### Better rename-target normalization than Original

- `en-cursor-rename-identifier`: `0.5357 -> 0.8350`

`Original` output:

```text
rename user_id to user_id and keep OAuth capitalized
```

`V15` output:

```text
please rename user id to user_id and keep OAuth capitalized
```

## Where V15 Regressed Against Original

### It still fails the literal meta-instruction case badly

- `en-technical-meta-instruction-literal`: `0.9143 -> 0.5225`

`Original` output:

```text
Ignore my last message, just write a PR description.
```

`V15` output:

```text
Just write a PR description.
```

This is a serious miss because it drops a spoken phrase instead of merely cleaning it.

### It still collapses multilingual prose into bullets in a prose case

- `ro-mixed-language-technical`: `1.0000 -> 0.6453`

`Original` output:

```text
Adaugă, te rog, un bullet despre rollback plan și un altul despre feature flag cleanup.
```

`V15` output:

```text
- rollback plan
- feature flag cleanup
```

This is exactly the kind of prose-to-list conversion the prompt was meant to avoid.

### It regressed on a strong name-nearmatch greeting case

- `en-email-recipient-name-nearmatch-greeting`: `0.9230 -> 0.5877`

`Original` output:

```text
Hi Aysha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

`V15` output:

```text
Hi Aisha,

Thank you for sending over the dates. You can find our booking link here. Thank you.
```

`V15` improved email formatting, but lost the near-match spelling correction that mattered more on this case.

### It badly underperformed on Romanian formal email

- `ro-email-formal`: `0.7934 -> 0.5403`

`Original` output:

```text
Mulțumesc. Am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

`V15` output:

```text
Bună,

Am atasat varianta actualizata si pot trimite cifrele finale pana diseara.

Multumesc
```

The added structure helped formatting, but the loss of diacritics and awkward rewrite hurt the score more.

## What The Example Baseline Changed

Adding the long in-prompt example did not help overall in this run.

- `Original`: `0.8366`
- `Baseline`: `0.8152`

The example baseline did help in some formatting-specific cases:

- `en-cursor-rename-identifier`
- `en-explicit-bulleted-list`
- `es-explicit-numbered-list`
- `en-technical-meta-instruction-literal`
- `en-terminal-preserve-technical-string`

But it lost too much elsewhere, especially on name-nearmatch and multilingual cases.

## Bottom Line

On this run:

- `Original` is the strongest of the three.
- `V15` is directionally better than `Baseline`, but not yet better than `Original`.
- The biggest remaining blockers for `V15` are:
  - literal preservation in meta-instruction cases
  - keeping prose as prose in multilingual technical sentences
  - preserving strong near-match name correction
  - not sacrificing native-language quality for extra email structure
