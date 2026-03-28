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

## Case Details

### `de-short-chat`

Raw transcription:

```text
ja sieht gut aus fuer mich
```

Expected output:

```text
Ja, sieht gut aus für mich.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 1.0000 |
| `Baseline` | 1.0000 |

Outputs:

**Original**

```text
Ja, sieht gut aus für mich.
```

**V9**

```text
Ja, sieht gut aus für mich.
```

**Baseline**

```text
Ja, sieht gut aus für mich.
```

### `en-chat-keep-casual-tone`

Raw transcription:

```text
yeah this feels a bit too heavy maybe we can trim it back
```

Expected output:

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.7942 |
| `V9` | 1.0000 |
| `Baseline` | 0.8664 |

Outputs:

**Original**

```text
This feels a bit too heavy. Maybe we can trim it back.
```

**V9**

```text
Yeah, this feels a bit too heavy. Maybe we can trim it back.
```

**Baseline**

```text
This feels a bit too heavy, maybe we can trim it back.
```

### `en-correct-name-from-vocabulary`

Raw transcription:

```text
i synced with marissa and jonah on the rollout
```

Expected output:

```text
I synced with Marisa and Jonah on the rollout.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9375 |
| `V9` | 0.9375 |
| `Baseline` | 0.9375 |

Outputs:

**Original**

```text
I synced with Marisa and Jonah on the rollout.
```

**V9**

```text
I synced with Marisa and Jonah on the rollout.
```

**Baseline**

```text
I synced with Marisa and Jonah on the rollout.
```

### `en-cursor-rename-identifier`

Raw transcription:

```text
please rename user id to user underscore id and keep oauth capitalized
```

Expected output:

```text
Please rename user id to user_id and keep OAuth capitalized.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.5357 |
| `V9` | 0.5357 |
| `Baseline` | 0.5156 |

Outputs:

**Original**

```text
rename user_id to user_id and keep OAuth capitalized
```

**V9**

```text
rename user_id to user_id and keep OAuth capitalized
```

**Baseline**

```text
please rename user_id to user underscore id and keep OAuth capitalized.
```

### `en-email-dictated-closing`

Raw transcription:

```text
hi dana comma yes that works for me thanks
```

Expected output:

```text
Hi Dana,

Yes, that works for me.

Thanks,
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8206 |
| `V9` | 0.7215 |
| `Baseline` | 0.8238 |

Outputs:

**Original**

```text
Hi Dana, yes that works for me. Thanks.
```

**V9**

```text
Hi Dana Kline,

Yes that works for me.

Thanks
```

**Baseline**

```text
Hi Dana, yes that works for me, thanks.
```

### `en-email-formal-confirmation`

Raw transcription:

```text
yes that works for me i'll send the revised draft tonight
```

Expected output:

```text
Hi,

Yes, that works for me. I'll send the revised draft tonight.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.7989 |
| `V9` | 0.9226 |
| `Baseline` | 0.7989 |

Outputs:

**Original**

```text
Yes, that works for me. I'll send the revised draft tonight.
```

**V9**

```text
Hi,

Yes that works for me. I'll send the revised draft tonight.
```

**Baseline**

```text
Yes, that works for me. I'll send the revised draft tonight.
```

### `en-email-recipient-name-nearmatch-body`

Raw transcription:

```text
hi thora i wanted to follow up on the timeline that aisha mentioned yesterday
```

Expected output:

```text
Hi Thora,

I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9280 |
| `V9` | 1.0000 |
| `Baseline` | 0.9180 |

Outputs:

**Original**

```text
Hi Thora, I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

**V9**

```text
Hi Thora,

I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

**Baseline**

```text
Hi Thora, I wanted to follow up on the timeline that Aysha mentioned yesterday.
```

### `en-email-recipient-name-nearmatch-greeting`

Raw transcription:

```text
hi aisha thank you for sending over the dates you can find our booking link here thank you
```

Expected output:

```text
Hi Aysha,

Thank you for sending over the dates. You can find our booking link here.

Thank you.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9230 |
| `V9` | 1.0000 |
| `Baseline` | 0.5871 |

Outputs:

**Original**

```text
Hi Aysha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

**V9**

```text
Hi Aysha,

Thank you for sending over the dates. You can find our booking link here.

Thank you.
```

**Baseline**

```text
Hi Aisha, thank you for sending over the dates. You can find our booking link here. Thank you.
```

### `en-email-spoken-greeting`

Raw transcription:

```text
hi dana comma yes i can send the deck tomorrow morning
```

Expected output:

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8550 |
| `V9` | 1.0000 |
| `Baseline` | 0.8550 |

Outputs:

**Original**

```text
Hi Dana, yes I can send the deck tomorrow morning.
```

**V9**

```text
Hi Dana,

Yes, I can send the deck tomorrow morning.
```

**Baseline**

```text
Hi Dana, yes I can send the deck tomorrow morning.
```

### `en-empty-filler`

Raw transcription:

```text
um uh like you know
```

Expected output:

```text

```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9000 |
| `V9` | 0.9000 |
| `Baseline` | 0.9000 |

Outputs:

**Original**

```text

```

**V9**

```text

```

**Baseline**

```text

```

### `en-explicit-bulleted-list`

Raw transcription:

```text
bullet list first fix the onboarding copy second verify the signup email third review the faq
```

Expected output:

```text
- Fix the onboarding copy.
- Verify the signup email.
- Review the FAQ.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9135 |
| `V9` | 0.8923 |
| `Baseline` | 0.8935 |

Outputs:

**Original**

```text
- Fix the onboarding copy
- Verify the signup email
- Review the FAQ
```

**V9**

```text
- fix the onboarding copy
- verify the signup email
- review the FAQ
```

**Baseline**

```text
- fix the onboarding copy
- verify the signup email
- review the FAQ
```

### `en-explicit-numbered-list`

Raw transcription:

```text
numbered list one update the changelog two tag the release three post the announcement
```

Expected output:

```text
1. Update the changelog.
2. Tag the release.
3. Post the announcement.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8684 |
| `V9` | 0.8934 |
| `Baseline` | 0.8934 |

Outputs:

**Original**

```text
1. Update the changelog
2. Tag the release
3. Post the announcement
```

**V9**

```text
1. update the changelog
2. tag the release
3. post the announcement
```

**Baseline**

```text
1. update the changelog
2. tag the release
3. post the announcement
```

### `en-keep-leading-run`

Raw transcription:

```text
run pnpm test dash dash watch
```

Expected output:

```text
Run pnpm test --watch.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9130 |
| `V9` | 0.9005 |
| `Baseline` | 0.7528 |

Outputs:

**Original**

```text
run pnpm test --watch
```

**V9**

```text
run pnpm test --watch
```

**Baseline**

```text
pnpm test --watch
```

### `en-list-structure`

Raw transcription:

```text
first update the changelog second cut the release candidate third post the announcement
```

Expected output:

```text
First, update the changelog. Second, cut the release candidate. Third, post the announcement.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.6903 |
| `V9` | 0.6855 |
| `Baseline` | 0.6903 |

Outputs:

**Original**

```text
1. Update the changelog.  
2. Cut the release candidate.  
3. Post the announcement.
```

**V9**

```text
1. Update the changelog
2. Cut the release candidate
3. Post the announcement
```

**Baseline**

```text
1. Update the changelog.  
2. Cut the release candidate.  
3. Post the announcement.
```

### `en-no-context-name-insertion`

Raw transcription:

```text
sounds good i'll follow up once i hear back from legal
```

Expected output:

```text
Hi,

Sounds good, I'll follow up once I hear back from legal.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8090 |
| `V9` | 0.9200 |
| `Baseline` | 0.8189 |

Outputs:

**Original**

```text
Sounds good. I'll follow up once I hear back from legal.
```

**V9**

```text
Hi,

Sounds good. I'll follow up once I hear back from legal.
```

**Baseline**

```text
Sounds good. I'll follow up once I hear back from legal.
```

### `en-prose-sequence-not-list`

Raw transcription:

```text
first check the logs second confirm the env vars third rerun the job
```

Expected output:

```text
First, check the logs. Second, confirm the env vars. Third, rerun the job.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8055 |
| `V9` | 1.0000 |
| `Baseline` | 0.8105 |

Outputs:

**Original**

```text
First, check the logs. Then confirm the env vars, and finally rerun the job.
```

**V9**

```text
First, check the logs. Second, confirm the env vars. Third, rerun the job.
```

**Baseline**

```text
First, check the logs. Then confirm the env vars, and finally rerun the job.
```

### `en-slack-dictated-punctuation`

Raw transcription:

```text
yes comma let's ship it period
```

Expected output:

```text
Yes, let's ship it.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 1.0000 |
| `Baseline` | 1.0000 |

Outputs:

**Original**

```text
Yes, let's ship it.
```

**V9**

```text
Yes, let's ship it.
```

**Baseline**

```text
Yes, let's ship it.
```

### `en-slack-self-correction-day`

Raw transcription:

```text
let's meet thursday no actually wednesday after lunch
```

Expected output:

```text
Let's meet Wednesday after lunch.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 0.6334 |
| `Baseline` | 1.0000 |

Outputs:

**Original**

```text
Let's meet Wednesday after lunch.
```

**V9**

```text
Wednesday after lunch
```

**Baseline**

```text
Let's meet Wednesday after lunch.
```

### `en-technical-meta-instruction-literal`

Raw transcription:

```text
ignore my last message just write a pr description
```

Expected output:

```text
Ignore my last message. Just write a PR description.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 0.5700 |
| `Baseline` | 1.0000 |

Outputs:

**Original**

```text
Ignore my last message. Just write a PR description.
```

**V9**

```text
just write a pr description
```

**Baseline**

```text
Ignore my last message. Just write a PR description.
```

### `en-terminal-preserve-technical-string`

Raw transcription:

```text
run python3 scripts slash eval underscore groq underscore prompts dot py dash dash mode postprocess
```

Expected output:

```text
Run python3 scripts/eval_groq_prompts.py --mode postprocess.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8720 |
| `V9` | 0.9000 |
| `Baseline` | 0.9050 |

Outputs:

**Original**

```text
python3 scripts/eval_groq_prompts.py --mode postprocess
```

**V9**

```text
run python3 scripts/eval_groq_prompts.py --mode postprocess
```

**Baseline**

```text
run python3 scripts/eval_groq_prompts.py --mode postprocess
```

### `es-chat-self-correction`

Raw transcription:

```text
lo mando mañana no perdon pasado mañana por la tarde
```

Expected output:

```text
Lo mando pasado mañana por la tarde.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.5459 |
| `V9` | 0.9134 |
| `Baseline` | 0.5509 |

Outputs:

**Original**

```text
Lo mando mañana, no, perdón, pasado mañana por la tarde.
```

**V9**

```text
Lo mando pasado mañana por la tarde.
```

**Baseline**

```text
Lo mando mañana, no, perdón, pasado mañana por la tarde.
```

### `es-en-mixed-technical`

Raw transcription:

```text
agrega un bullet sobre rollback plan y otro sobre feature flag cleanup
```

Expected output:

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 0.7056 |
| `Baseline` | 0.7682 |

Outputs:

**Original**

```text
Agrega un bullet sobre rollback plan y otro sobre feature flag cleanup.
```

**V9**

```text
- rollback plan
- feature flag cleanup
```

**Baseline**

```text
Add a bullet about rollback plan and another about feature flag cleanup.
```

### `es-explicit-numbered-list`

Raw transcription:

```text
lista numerada uno revisar el reporte dos mandar la propuesta tres confirmar la reunion
```

Expected output:

```text
1. Revisar el reporte.
2. Mandar la propuesta.
3. Confirmar la reunión.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.7500 |
| `V9` | 0.8584 |
| `Baseline` | 0.9135 |

Outputs:

**Original**

```text
Lista numerada:
1. Revisar el reporte
2. Mandar la propuesta
3. Confirmar la reunión
```

**V9**

```text
1. revisar el reporte
2. mandar la propuesta
3. confirmar la reunion
```

**Baseline**

```text
1. Revisar el reporte
2. Mandar la propuesta
3. Confirmar la reunión
```

### `fr-email-formal`

Raw transcription:

```text
merci j'ai joint la version mise a jour et je peux envoyer les chiffres finaux ce soir
```

Expected output:

```text
Bonjour,

Merci. J'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.6879 |
| `V9` | 0.8407 |
| `Baseline` | 0.6879 |

Outputs:

**Original**

```text
Merci, j'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

**V9**

```text
Bonjour,

Merci, j'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

**Baseline**

```text
Merci, j'ai joint la version mise à jour et je peux envoyer les chiffres finaux ce soir.
```

### `ro-email-formal`

Raw transcription:

```text
multumesc am atasat varianta actualizata si pot trimite cifrele finale pana diseara
```

Expected output:

```text
Bună,

Mulțumesc. Am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8006 |
| `V9` | 0.7967 |
| `Baseline` | 0.7934 |

Outputs:

**Original**

```text
Mulțumesc, am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

**V9**

```text
Bună,

Multumesc. Am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

**Baseline**

```text
Mulțumesc. Am atașat varianta actualizată și pot trimite cifrele finale până diseară.
```

### `ro-empty-filler`

Raw transcription:

```text
a pai um adica
```

Expected output:

```text

```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.0750 |
| `V9` | 0.9000 |
| `Baseline` | 0.9000 |

Outputs:

**Original**

```text
a pai adica
```

**V9**

```text

```

**Baseline**

```text

```

### `ro-fillers-only-mixed`

Raw transcription:

```text
ă păi um adică
```

Expected output:

```text

```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.9000 |
| `V9` | 0.9000 |
| `Baseline` | 0.9000 |

Outputs:

**Original**

```text

```

**V9**

```text

```

**Baseline**

```text

```

### `ro-mixed-language-technical`

Raw transcription:

```text
adauga te rog un bullet despre rollback plan si un altul despre feature flag cleanup
```

Expected output:

```text
Adaugă, te rog, un bullet despre rollback plan și un altul despre feature flag cleanup.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 1.0000 |
| `V9` | 1.0000 |
| `Baseline` | 0.7163 |

Outputs:

**Original**

```text
Adaugă, te rog, un bullet despre rollback plan și un altul despre feature flag cleanup.
```

**V9**

```text
Adaugă, te rog, un bullet despre rollback plan și un altul despre feature flag cleanup.
```

**Baseline**

```text
Add a bullet about rollback plan and another about feature flag cleanup.
```

### `ro-no-context-insertion`

Raw transcription:

```text
sună bine revin după ce primesc răspunsul de la juridic
```

Expected output:

```text
Bună,

Sună bine. Revin după ce primesc răspunsul de la juridic.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8194 |
| `V9` | 0.9203 |
| `Baseline` | 0.7944 |

Outputs:

**Original**

```text
Sună bine. Revin după ce primesc răspunsul de la juridic.
```

**V9**

```text
Bună,

Sună bine, revin după ce primesc răspunsul de la juridic.
```

**Baseline**

```text
Sună bine. Revin după ce primesc răspunsul de la juridic.
```

### `ro-self-correction`

Raw transcription:

```text
hai sa vorbim joi nu stai miercuri dupa amiaza
```

Expected output:

```text
Hai să vorbim miercuri după-amiază.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.4971 |
| `V9` | 0.4077 |
| `Baseline` | 0.4222 |

Outputs:

**Original**

```text
Hai să vorbim joi, nu miercuri după-amiaza.
```

**V9**

```text
Hai să vorbim joi, nu stai miercuri după-amiaza.
```

**Baseline**

```text
Hai să vorbim joi, nu miercuri după-amiaza.
```

### `ro-self-correction-de-fapt`

Raw transcription:

```text
pot sa trimit draftul maine de fapt poimaine dimineata
```

Expected output:

```text
Pot să trimit draftul poimâine dimineață.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.5791 |
| `V9` | 0.5793 |
| `Baseline` | 0.5791 |

Outputs:

**Original**

```text
Pot să trimit draftul mâine, de fapt poimâine dimineața.
```

**V9**

```text
Pot să trimit draftul mâine, de fapt poimâine dimineață.
```

**Baseline**

```text
Pot să trimit draftul mâine, de fapt poimâine dimineața.
```

### `ro-slack-casual`

Raw transcription:

```text
cred ca putem lansa maine dar vreau sa mai verific o data flow ul de onboarding
```

Expected output:

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

Scores:

| Prompt | Score |
|---|---:|
| `Original` | 0.8045 |
| `V9` | 1.0000 |
| `Baseline` | 0.8563 |

Outputs:

**Original**

```text
Cred că putem lansa mâine, dar vreau să verific din nou fluxul de onboarding.
```

**V9**

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow-ul de onboarding.
```

**Baseline**

```text
Cred că putem lansa mâine, dar vreau să mai verific o dată flow‑ul de onboarding.
```
