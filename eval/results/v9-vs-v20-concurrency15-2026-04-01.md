# V9 vs V20 Comparison

Two-way comparison for:

- V9: `system-gptoss-multilingual-email-v9`
- V20: `system-gptoss-multilingual-email-v20`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Routing: OpenRouter default routing with `provider.sort=throughput`
- Concurrency: `15`
- Raw result JSON: `eval/results/v20-vs-v9-hybrid-concurrency15-2026-04-01.json`
- Stability rerun: `eval/results/v20-vs-v9-hybrid-concurrency15-run2-2026-04-01.json`

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `system-gptoss-multilingual-email-v20` | 0.8590 | 32 |
| `system-gptoss-multilingual-email-v9` | 0.8121 | 32 |

Net gain for `v20`: `+0.0469`

Note: a second full rerun with the same settings also kept `v20` ahead, `0.8367` vs `0.8144`.

## High-Level Read

`v20` is clearly stronger overall.

- It fixes the literal meta-instruction case.
- It fixes the Slack self-correction case.
- It fixes the mixed Spanish/English bullet-prose case in this run.
- It also improves the email near-match body case without breaking explicit bullet-list formatting.
- It still loses on some casual chat and Romanian correction cases.

## Case Table

| Case | V9 | V20 | Best |
|---|---:|---:|---|
| `de-short-chat` | 1.0000 | 1.0000 | Tie |
| `en-chat-keep-casual-tone` | 1.0000 | 0.9074 | V9 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | Tie |
| `en-cursor-rename-identifier` | 0.5657 | 0.5357 | V9 |
| `en-email-dictated-closing` | 0.5250 | 0.7787 | V20 |
| `en-email-formal-confirmation` | 1.0000 | 0.9226 | V9 |
| `en-email-recipient-name-nearmatch-body` | 0.6170 | 1.0000 | V20 |
| `en-email-recipient-name-nearmatch-greeting` | 0.9750 | 0.9750 | Tie |
| `en-email-spoken-greeting` | 1.0000 | 0.6209 | V9 |
| `en-empty-filler` | 0.9000 | 0.9000 | Tie |
| `en-explicit-bulleted-list` | 0.8935 | 0.8935 | Tie |
| `en-explicit-numbered-list` | 0.8684 | 0.8835 | V20 |
| `en-keep-leading-run` | 0.9130 | 0.9080 | V9 |
| `en-list-structure` | 0.6754 | 0.6903 | V20 |
| `en-no-context-name-insertion` | 0.9200 | 0.9200 | Tie |
| `en-prose-sequence-not-list` | 0.8988 | 0.8988 | Tie |
| `en-slack-dictated-punctuation` | 1.0000 | 1.0000 | Tie |
| `en-slack-self-correction-day` | 0.7225 | 1.0000 | V20 |
| `en-technical-meta-instruction-literal` | 0.9121 | 1.0000 | V20 |
| `en-terminal-preserve-technical-string` | 0.8770 | 0.8850 | V20 |
| `es-chat-self-correction` | 0.9134 | 0.8260 | V9 |
| `es-en-mixed-technical` | 0.5482 | 1.0000 | V20 |
| `es-explicit-numbered-list` | 0.8309 | 0.9135 | V20 |
| `fr-email-formal` | 0.8519 | 0.8594 | V20 |
| `ro-email-formal` | 0.4184 | 0.7967 | V20 |
| `ro-empty-filler` | 0.9000 | 0.9000 | Tie |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | Tie |
| `ro-mixed-language-technical` | 0.3728 | 0.9215 | V20 |
| `ro-no-context-insertion` | 0.9078 | 0.9203 | V20 |
| `ro-self-correction` | 0.5045 | 0.4372 | V9 |
| `ro-self-correction-de-fapt` | 0.6368 | 0.5415 | V9 |
| `ro-slack-casual` | 1.0000 | 0.8143 | V9 |

## Target Cases

### `en-technical-meta-instruction-literal`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.9121 |
| `V20` | 1.0000 |

Outputs:

**V9**

```text
Ignore my last message just write a PR description.
```

**V20**

```text
Ignore my last message. Just write a PR description.
```

### `en-slack-self-correction-day`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.7225 |
| `V20` | 1.0000 |

Outputs:

**V9**

```text
Wednesday after lunch.
```

**V20**

```text
Let's meet Wednesday after lunch.
```

### `es-en-mixed-technical`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.5482 |
| `V20` | 1.0000 |

Outputs:

**V9**

```text
- rollback plan
- feature flag cleanup
```

**V20**

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

## Where V20 Improved Most

- `ro-mixed-language-technical`: `0.3728 -> 0.9215`
- `es-en-mixed-technical`: `0.5482 -> 1.0000`
- `en-email-recipient-name-nearmatch-body`: `0.6170 -> 1.0000`
- `ro-email-formal`: `0.4184 -> 0.7967`
- `en-slack-self-correction-day`: `0.7225 -> 1.0000`
- `en-email-dictated-closing`: `0.5250 -> 0.7787`

Representative outputs:

### `en-email-recipient-name-nearmatch-body`

**V9**

```text
Hi Thora,

I wanted to follow up on the timeline that Aisha mentioned yesterday.
```

**V20**

```text
Hi Thora,

I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

### `en-email-recipient-name-nearmatch-greeting`

**V9**

```text
Hi Aysha,

Thank you for sending over the dates. You can find our booking link here. Thank you.
```

**V20**

```text
Hi Aysha,

Thank you for sending over the dates. You can find our booking link here. Thank you.
```

## Where V20 Still Regresses

- `en-email-spoken-greeting`: `1.0000 -> 0.6209`
- `ro-slack-casual`: `1.0000 -> 0.8143`
- `ro-self-correction-de-fapt`: `0.6368 -> 0.5415`
- `en-chat-keep-casual-tone`: `1.0000 -> 0.9074`
- `es-chat-self-correction`: `0.9134 -> 0.8260`
- `en-email-formal-confirmation`: `1.0000 -> 0.9226`

<details>
<summary><code>en-email-spoken-greeting</code></summary>

Raw transcript:

```text
hi dana comma yes i can send the deck tomorrow morning
```

Expected text:

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

V9:

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

V20:

```text
Hi Dana Kline,

Yes, I can send the deck tomorrow morning.
```

</details>

<details>
<summary><code>ro-slack-casual</code></summary>

Raw transcript:

```text
cred ca putem lansa maine dar vreau sa mai verific o data flow ul de onboarding
```

Expected text:

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

V9:

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

V20:

```text
Bună,

Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

</details>

<details>
<summary><code>ro-self-correction-de-fapt</code></summary>

Raw transcript:

```text
pot sa trimit draftul maine de fapt poimaine dimineata
```

Expected text:

```text
Pot să trimit draftul poimâine dimineață.
```

V9:

```text
Pot să trimit draftul mâine, de fapt poimâine dimineață.
```

V20:

```text
Pot să trimit draftul mâine de fapt poimâine dimineață.
```

</details>

<details>
<summary><code>en-chat-keep-casual-tone</code></summary>

Raw transcript:

```text
yeah this feels a bit too heavy maybe we can trim it back
```

Expected text:

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

V9:

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

V20:

```text
Yeah this feels a bit too heavy, maybe we can trim it back.
```

</details>

<details>
<summary><code>es-chat-self-correction</code></summary>

Raw transcript:

```text
lo mando mañana no perdon pasado mañana por la tarde
```

Expected text:

```text
Lo mando pasado mañana por la tarde.
```

V9:

```text
Lo mando pasado mañana por la tarde.
```

V20:

```text
lo mando pasado mañana por la tarde
```

</details>

<details>
<summary><code>en-email-formal-confirmation</code></summary>

Raw transcript:

```text
yes that works for me i'll send the revised draft tonight
```

Expected text:

```text
Hi,

Yes, that works for me. I'll send the revised draft tonight.
```

V9:

```text
Hi,

Yes, that works for me. I'll send the revised draft tonight.
```

V20:

```text
Hi,

Yes that works for me. I'll send the revised draft tonight.
```

</details>

## Bottom Line

`v20` is the better prompt overall and the better candidate to ship.

The main remaining weak spots are:

- spoken-greeting email formatting
- Romanian casual chat / correction behavior
- a few cases where `v9` stays slightly more literal in casual or technical prose
