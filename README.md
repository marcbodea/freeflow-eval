# FreeFlow Prompt Eval

Standalone evaluation harness for finding the best context prompt and dictation post-processing prompt for FreeFlow-style speech correction.

This repo does not depend on the Swift app. It evaluates prompt variants against curated dictation fixtures and writes comparable JSON results.
It works with OpenAI-compatible chat APIs, including Groq directly and OpenRouter with provider routing.

## What This Repo Contains

- `eval_groq_prompts.py`: the runner
- `eval/prompt_variants.json`: system and context prompt candidates
- `eval/prompt_eval_cases*.json`: fixture suites
- `eval/results/`: saved comparison runs

## What I Optimized For

The final prompt search focused on:

- short-message cleanup
- email replies
- prompt-writing
- terminal / developer syntax
- multilingual dictation
- self-corrections and false starts
- explicit numbered / bulleted list dictation
- anti-hallucination behavior

## Final Winner

Best final combo found:

- Model: `openai/gpt-oss-20b`
- Route: OpenRouter with provider forced to `groq`
- Prompt: `system-gptoss-multilingual-email-v9`

Relevant result artifacts:

- `eval/results/gptoss20-v9-vs-baseline-with-example.json`
- `eval/results/gptoss20-v9-llm-expanded-2026-03-25.json`
- `eval/results/gptoss20-v9-email-name-focused-2026-03-26.json`
- `eval/results/gptoss20-v9-name-meta-focused-2026-03-26.json`

Hybrid-scored result against the baseline prompt with example on the 32-case English-context suite:

- `system-gptoss-multilingual-email-v9`: `0.8691`
- `user-baseline-system-with-example`: `0.8102`

## Models Tested

The search did not start with a single model. I compared multiple candidates first, then narrowed prompt tuning to the winner.

Models tested in saved runs:

- `openai/gpt-oss-20b`
- `openai/gpt-oss-120b`
- `meta-llama/llama-4-scout-17b-16e-instruct`
- `amazon/nova-micro-v1`
- `bytedance-seed/seed-2.0-mini`

Routing used during comparison:

- Groq direct for early free-tier checks
- OpenRouter with provider forced to `groq` for `gpt-oss-20b` and `llama-4-scout`
- OpenRouter default routing for the non-Groq candidates

Relevant result artifacts:

- `eval/results/system-only-model-compare.json`
- `eval/results/openrouter-groq-compare.json`
- `eval/results/openrouter-default-compare.json`
- `eval/results/gptoss20-prompt-optimization.json`

## How I Evaluated

### 1. Broad heuristic search

I first used heuristic scoring to search prompt variants quickly and cheaply.

Heuristic score components:

- reference similarity
- required term coverage
- forbidden term penalties
- exact-match bonus

### 2. Same-model judging

I then added LLM judging with the same model under test.

Modes:

- `heuristic`
- `llm`
- `hybrid`

For the final selector, I used `hybrid` because pure LLM judging was too unstable on its own.

### 3. Provider-controlled comparisons

For OpenRouter runs, I forced `groq` for supported models using provider routing.
The same runner also works against Groq directly by changing only `--base-url` and `--api-key`.

## Main Suites

- `eval/prompt_eval_cases_system_only_en_context.json`
  Main multilingual system-prompt suite with English context summaries and multilingual transcripts.

- `eval/prompt_eval_cases_wispr_claims.json`
  Wispr-inspired suite built from publicly advertised product behaviors that overlap with this app.

- `eval/prompt_eval_cases_productivity.json`
  Earlier productivity-focused suite for Slack / email / prompts.

## Example Commands

Run directly against Groq:

```bash
python3 eval_groq_prompts.py \
  --api-key "$GROQ_API_KEY" \
  --base-url https://api.groq.com/openai/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models meta-llama/llama-4-scout-17b-16e-instruct openai/gpt-oss-20b \
  --system-variants app-default-system system-gptoss-multilingual-email-v9 \
  --scoring-mode heuristic \
  --output-json eval/results/direct-groq-example.json
```

Run through OpenRouter with Groq forced as the provider:

```bash
python3 eval_groq_prompts.py \
  --api-key "$OPENROUTER_API_KEY" \
  --base-url https://openrouter.ai/api/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models openai/gpt-oss-20b \
  --system-variants user-baseline-system-with-example system-gptoss-multilingual-email-v9 \
  --scoring-mode heuristic \
  --min-request-interval 0 \
  --provider-order groq \
  --no-allow-provider-fallbacks \
  --output-json eval/results/example.json
```

Run the stricter final comparison with hybrid scoring:

```bash
python3 eval_groq_prompts.py \
  --api-key "$OPENROUTER_API_KEY" \
  --base-url https://openrouter.ai/api/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models openai/gpt-oss-20b \
  --system-variants user-baseline-system-with-example system-gptoss-multilingual-email-v9 \
  --scoring-mode hybrid \
  --min-request-interval 0 \
  --provider-order groq \
  --no-allow-provider-fallbacks \
  --output-json eval/results/gptoss20-v9-vs-baseline-with-example.json
```

## Notes

- `eval/results/` contains reproducibility artifacts from the search.
- LLM-judge responses are stored in output JSON when LLM scoring is enabled.
- The runner uses only the Python standard library.
- The script name is historical. It now supports generic OpenAI-compatible APIs, not just Groq.
