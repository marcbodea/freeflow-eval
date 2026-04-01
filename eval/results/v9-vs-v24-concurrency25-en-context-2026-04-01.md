# V9 vs V24 Comparison

Two-way comparison for:

- V9: `system-gptoss-multilingual-email-v9`
- V24: `system-gptoss-multilingual-email-v24`
- Model: `openai/gpt-oss-20b`
- Scoring: `hybrid`
- Cases: `eval/prompt_eval_cases_system_only_en_context.json`
- Routing: OpenRouter default routing with `provider.sort=throughput`
- Concurrency: `25`
- Raw result JSON: `eval/results/v24-vs-v9-hybrid-concurrency25-en-context-2026-04-01.json`

## Summary

| Prompt | Avg hybrid score | Cases |
|---|---:|---:|
| `system-gptoss-multilingual-email-v24` | 0.8717 | 32 |
| `system-gptoss-multilingual-email-v9` | 0.8300 | 32 |

Net gain for `v24`: `+0.0417`

## High-Level Read

`v24` is stronger overall on this suite.

- It preserves the `v20` gains on literal meta-instructions and Slack self-correction.
- It improves the email near-match body case and the mixed Spanish/English bullet-prose case versus `v9` in this run.
- It keeps the no-expansion greeting rule for spoken names while still correcting near-match names from context.
- It still regresses on some formal email and Romanian no-context cases.

## Case Table

| Case | V9 | V24 | Best |
|---|---:|---:|---|
| `en-slack-self-correction-day` | 0.6950 | 1.0000 | V24 |
| `en-email-formal-confirmation` | 0.9226 | 0.8068 | V9 |
| `en-no-context-name-insertion` | 0.9200 | 0.8189 | V9 |
| `en-correct-name-from-vocabulary` | 0.9375 | 0.9375 | Tie |
| `en-terminal-preserve-technical-string` | 0.8519 | 0.8244 | V9 |
| `en-chat-keep-casual-tone` | 1.0000 | 1.0000 | Tie |
| `en-list-structure` | 0.6903 | 0.6791 | V9 |
| `ro-slack-casual` | 1.0000 | 1.0000 | Tie |
| `ro-email-formal` | 0.7276 | 0.5739 | V9 |
| `ro-self-correction` | 0.4472 | 0.5247 | V24 |
| `ro-mixed-language-technical` | 0.4478 | 0.8555 | V24 |
| `ro-empty-filler` | 0.9000 | 0.9000 | Tie |
| `ro-no-context-insertion` | 0.9203 | 0.7902 | V9 |
| `ro-self-correction-de-fapt` | 0.9134 | 0.9134 | Tie |
| `ro-fillers-only-mixed` | 0.9000 | 0.9000 | Tie |
| `en-prose-sequence-not-list` | 0.7911 | 0.9138 | V24 |
| `en-keep-leading-run` | 0.9080 | 0.8955 | V9 |
| `es-chat-self-correction` | 0.9134 | 0.9134 | Tie |
| `fr-email-formal` | 0.8594 | 0.6979 | V9 |
| `es-en-mixed-technical` | 0.5482 | 0.6831 | V24 |
| `de-short-chat` | 1.0000 | 1.0000 | Tie |
| `en-explicit-numbered-list` | 0.8934 | 0.8934 | Tie |
| `en-explicit-bulleted-list` | 0.8935 | 0.8885 | V9 |
| `es-explicit-numbered-list` | 0.8309 | 0.8309 | Tie |
| `en-email-spoken-greeting` | 1.0000 | 1.0000 | Tie |
| `en-slack-dictated-punctuation` | 0.9950 | 1.0000 | V24 |
| `en-empty-filler` | 0.9000 | 0.9000 | Tie |
| `en-cursor-rename-identifier` | 0.7007 | 0.9375 | V24 |
| `en-email-dictated-closing` | 0.8647 | 0.8407 | V9 |
| `en-email-recipient-name-nearmatch-greeting` | 0.9750 | 0.9750 | Tie |
| `en-email-recipient-name-nearmatch-body` | 0.6170 | 1.0000 | V24 |
| `en-technical-meta-instruction-literal` | 0.5975 | 1.0000 | V24 |

## Target Cases

### `en-email-spoken-greeting`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 1.0000 |
| `V24` | 1.0000 |

Outputs:

**V9**

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

**V24**

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

### `en-technical-meta-instruction-literal`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.5975 |
| `V24` | 1.0000 |

Outputs:

**V9**

```text
Just write a PR description.
```

**V24**

```text
Ignore my last message. Just write a PR description.
```

### `en-slack-self-correction-day`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.6950 |
| `V24` | 1.0000 |

Outputs:

**V9**

```text
Wednesday after lunch.
```

**V24**

```text
Let's meet Wednesday after lunch.
```

### `es-en-mixed-technical`

Scores:

| Prompt | Score |
|---|---:|
| `V9` | 0.5482 |
| `V24` | 0.6831 |

Outputs:

**V9**

```text
- rollback plan
- feature flag cleanup
```

**V24**

```text
- rollback plan
- feature flag cleanup
```

## Where V24 Improved Most

- `ro-mixed-language-technical`: `0.4478 -> 0.8555`
- `en-technical-meta-instruction-literal`: `0.5975 -> 1.0000`
- `en-email-recipient-name-nearmatch-body`: `0.6170 -> 1.0000`
- `en-slack-self-correction-day`: `0.6950 -> 1.0000`
- `en-cursor-rename-identifier`: `0.7007 -> 0.9375`
- `es-en-mixed-technical`: `0.5482 -> 0.6831`
- `en-prose-sequence-not-list`: `0.7911 -> 0.9138`
- `ro-self-correction`: `0.4472 -> 0.5247`

Representative outputs:

### `en-email-recipient-name-nearmatch-body`

**V9**

```text
Hi Thora,

I wanted to follow up on the timeline that Aisha mentioned yesterday.
```

**V24**

```text
Hi Thora,

I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

### `en-technical-meta-instruction-literal`

**V9**

```text
Just write a PR description.
```

**V24**

```text
Ignore my last message. Just write a PR description.
```

## Where V24 Still Regresses

- `fr-email-formal`: `0.8594 -> 0.6979`
- `ro-email-formal`: `0.7276 -> 0.5739`
- `ro-no-context-insertion`: `0.9203 -> 0.7902`
- `en-email-formal-confirmation`: `0.9226 -> 0.8068`
- `en-no-context-name-insertion`: `0.9200 -> 0.8189`
- `en-terminal-preserve-technical-string`: `0.8519 -> 0.8244`
- `en-email-dictated-closing`: `0.8647 -> 0.8407`
- `en-keep-leading-run`: `0.9080 -> 0.8955`

<details>
<summary><code>fr-email-formal</code></summary>

Raw transcript:

```text
merci j'ai joint la version mise a jour et je peux envoyer les chiffres finaux ce soir
```

Expected text:

```text
Bonjour,

Merci. J'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

V9:

```text
Bonjour,

Merci, j'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

V24:

```text
Merci, j'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

</details>

<details>
<summary><code>ro-email-formal</code></summary>

Raw transcript:

```text
multumesc am atasat varianta actualizata si pot trimite cifrele finale pana diseara
```

Expected text:

```text
Bună,

Mulțumesc. Am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

V9:

```text
Bună,

Multumesc. Am atasat varianta actualizata și pot trimite cifrele finale până diseară.
```

V24:

```text
Multumesc. Am atasat varianta actualizata si pot trimite cifrele finale pana diseara.
```

</details>

<details>
<summary><code>ro-no-context-insertion</code></summary>

Raw transcript:

```text
sună bine revin după ce primesc răspunsul de la juridic
```

Expected text:

```text
Bună,

Sună bine. Revin după ce primesc răspunsul de la juridic.
```

V9:

```text
Bună,

Sună bine, revin după ce primesc răspunsul de la juridic.
```

V24:

```text
Sună bine, revin după ce primesc răspunsul de la juridic.
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

Yes that works for me. I'll send the revised draft tonight.
```

V24:

```text
Yes that works for me. I'll send the revised draft tonight.
```

</details>

<details>
<summary><code>en-no-context-name-insertion</code></summary>

Raw transcript:

```text
sounds good i'll follow up once i hear back from legal
```

Expected text:

```text
Hi,

Sounds good, I'll follow up once I hear back from legal.
```

V9:

```text
Hi,

Sounds good. I'll follow up once I hear back from legal.
```

V24:

```text
Sounds good. I'll follow up once I hear back from legal.
```

</details>

<details>
<summary><code>en-terminal-preserve-technical-string</code></summary>

Raw transcript:

```text
run python3 scripts slash eval underscore groq underscore prompts dot py dash dash mode postprocess
```

Expected text:

```text
Run python3 scripts/eval_groq_prompts.py --mode postprocess.
```

V9:

```text
python3 scripts/eval_groq_prompts.py --mode postprocess
```

V24:

```text
python3 scripts/eval_groq_prompts.py --mode postprocess
```

</details>

<details>
<summary><code>en-email-dictated-closing</code></summary>

Raw transcript:

```text
hi dana comma yes that works for me thanks
```

Expected text:

```text
Hi Dana,

Yes, that works for me.

Thanks,
```

V9:

```text
Hi Dana,

Yes that works for me, thanks.
```

V24:

```text
Hi Dana,

Yes that works for me thanks.
```

</details>

<details>
<summary><code>en-keep-leading-run</code></summary>

Raw transcript:

```text
run pnpm test dash dash watch
```

Expected text:

```text
Run pnpm test --watch.
```

V9:

```text
run pnpm test --watch
```

V24:

```text
run pnpm test --watch
```

</details>

<details>
<summary><code>en-list-structure</code></summary>

Raw transcript:

```text
first update the changelog second cut the release candidate third post the announcement
```

Expected text:

```text
First, update the changelog. Second, cut the release candidate. Third, post the announcement.
```

V9:

```text
1. Update the changelog.  
2. Cut the release candidate.  
3. Post the announcement.
```

V24:

```text
- Update the changelog
- Cut the release candidate
- Post the announcement
```

</details>

<details>
<summary><code>en-explicit-bulleted-list</code></summary>

Raw transcript:

```text
bullet list first fix the onboarding copy second verify the signup email third review the faq
```

Expected text:

```text
- Fix the onboarding copy.
- Verify the signup email.
- Review the FAQ.
```

V9:

```text
- fix the onboarding copy
- verify the signup email
- review the FAQ
```

V24:

```text
- fix the onboarding copy
- verify the signup email
- review the FAQ
```

</details>
