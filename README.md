# FreeFlow Prompt Eval

Standalone evaluation harness for tuning FreeFlow-style dictation prompts against curated fixtures.

The runner mirrors the app's prompt flow:

1. Context synthesis from window metadata and an optional screenshot.
2. Dictation post-processing from raw transcript + context summary.

It is intentionally separate from the Swift app and uses only the Python standard library.

## Current Status

The latest saved recommendation in this repo is:

- Model: `openai/gpt-oss-20b`
- System prompt: `system-gptoss-multilingual-email-v24`

Recent result artifacts:

- `eval/results/model-compare-v24-hybrid-gpt54nano-judge-2026-04-05.json`
- `eval/results/model-compare-v24-hybrid-gpt54nano-judge-2026-04-05.md`
- `eval/results/v24-vs-v9-hybrid-concurrency25-en-context-2026-04-01.json`
- `eval/results/v9-vs-v24-concurrency25-en-context-2026-04-01.md`

Key numbers from those runs:

- `v24` + `openai/gpt-oss-20b`: `0.8901` average hybrid score on the 32-case English-context suite with `openai/gpt-5.4-nano` as judge.
- `v24` beat `v9` head-to-head on the same 32-case suite: `0.8717` vs `0.8300`.
- On the `v24` model comparison run, `openai/gpt-oss-20b` beat `meta-llama/llama-4-scout` under both hybrid and heuristic scoring.

The current tradeoff is the same one reflected in the saved notes: `v24` improves anti-hallucination, literal meta-instruction handling, greeting/name behavior, and several mixed-language cases, but there are still weaker Romanian and some formal-email cases worth improving further.

## What This Repo Contains

- `eval_groq_prompts.py`: standalone runner for context, post-process, and end-to-end pipeline evals
- `eval/prompt_variants.json`: context and system prompt candidates, including the current `v24` system prompt
- `eval/prompt_eval_cases*.json`: main and focused fixture suites
- `eval/results/`: saved JSON outputs plus a few Markdown comparison notes
- `tests/test_eval_groq_prompts.py`: regression tests for CLI parsing and output scoring behavior

## What The Runner Supports

- `context` mode: evaluate only the context-summary stage
- `postprocess` mode: evaluate only transcript cleanup
- `pipeline` mode: run both stages together
- OpenAI-compatible chat APIs via `--base-url`
- Groq direct and OpenRouter routing
- optional OpenRouter provider routing via `--provider-order`, `--provider-sort`, and `--allow-provider-fallbacks`
- heuristic, LLM-judge, and hybrid scoring
- separate judge model selection via `--judge-model`
- optional parallel case execution via `--max-concurrency`
- screenshot-backed context cases when `screenshot_path` is present in a fixture

## Scoring

Post-process scoring is no longer just raw reference similarity.

Heuristic scoring now combines:

- reference similarity
- required-term coverage
- forbidden-term penalties
- exact-match bonus
- formatting checks for email structure, explicit lists, and self-correction cleanup
- output-contract checks that penalize wrappers like `Here is the clean transcript`

LLM scoring is available with:

- `--scoring-mode llm`
- `--scoring-mode hybrid`

When LLM scoring is enabled, the judge defaults to the candidate model unless `--judge-model` is set explicitly.

## Main Suites

- `eval/prompt_eval_cases_system_only_en_context.json`
  Main post-processing suite with English context summaries and multilingual transcripts.

- `eval/prompt_eval_cases_system_only.json`
  Earlier system-only suite.

- `eval/prompt_eval_cases_wispr_claims.json`
  Wispr-inspired behavior suite.

- `eval/prompt_eval_cases_productivity.json`
  Older productivity-focused Slack / email / prompt-writing cases.

- `eval/prompt_eval_cases.json`
  Mixed pipeline-oriented cases.

There are also a few temporary focused case files under `eval/tmp_*.json` used during prompt iteration.

## Quick Start

Python 3 is enough. There are no third-party dependencies.

Set one of these environment variables, or pass `--api-key` directly:

- `LLM_API_KEY`
- `OPENROUTER_API_KEY`
- `OPENAI_API_KEY`
- `GROQ_API_KEY`

Optional base URL env vars:

- `LLM_BASE_URL`
- `OPENAI_BASE_URL`
- `GROQ_BASE_URL`

## Example Commands

Run the current recommended setup through OpenRouter with hybrid scoring and an explicit judge model:

```bash
python3 eval_groq_prompts.py \
  --api-key "$OPENROUTER_API_KEY" \
  --base-url https://openrouter.ai/api/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models openai/gpt-oss-20b \
  --system-variants system-gptoss-multilingual-email-v24 \
  --scoring-mode hybrid \
  --judge-model openai/gpt-5.4-nano \
  --min-request-interval 0 \
  --max-concurrency 6 \
  --output-json eval/results/example-v24-hybrid.json
```

Compare two prompt variants head to head:

```bash
python3 eval_groq_prompts.py \
  --api-key "$OPENROUTER_API_KEY" \
  --base-url https://openrouter.ai/api/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models openai/gpt-oss-20b \
  --system-variants system-gptoss-multilingual-email-v9 system-gptoss-multilingual-email-v24 \
  --scoring-mode hybrid \
  --judge-model openai/gpt-5.4-nano \
  --min-request-interval 0 \
  --max-concurrency 6 \
  --output-json eval/results/v9-vs-v24-example.json
```

Run an end-to-end pipeline smoke test:

```bash
python3 eval_groq_prompts.py \
  --api-key "$OPENROUTER_API_KEY" \
  --base-url https://openrouter.ai/api/v1 \
  --mode pipeline \
  --cases eval/prompt_eval_cases.json \
  --models openai/gpt-oss-20b \
  --context-variants app-default-context \
  --system-variants app-default-system \
  --max-postprocess-cases 3 \
  --min-request-interval 0 \
  --output-json eval/results/pipeline-smoke.json
```

Run directly against Groq:

```bash
python3 eval_groq_prompts.py \
  --api-key "$GROQ_API_KEY" \
  --base-url https://api.groq.com/openai/v1 \
  --mode postprocess \
  --cases eval/prompt_eval_cases_system_only_en_context.json \
  --models meta-llama/llama-4-scout-17b-16e-instruct openai/gpt-oss-20b \
  --system-variants app-default-system system-gptoss-multilingual-email-v24 \
  --scoring-mode heuristic \
  --output-json eval/results/direct-groq-example.json
```

## Output Format

The runner prints the top summary rows to stdout and can optionally save a full JSON payload with:

- run metadata
- routing settings
- scoring mode
- summary table
- per-case outputs
- per-case score breakdowns
- raw judge responses when LLM scoring is enabled

## Tests

Run the regression tests with:

```bash
python3 -m unittest discover -s tests -v
```

Current tests cover:

- CLI parsing for `--judge-model`
- email formatting penalties
- wrapper / boilerplate penalties
- explicit list formatting expectations
- self-correction cleanup penalties
- dictated email closing structure
- literal handling of meta-instruction transcripts

## Notes

- The script name is historical. It now targets generic OpenAI-compatible chat APIs, not just Groq.
- OpenRouter runs default to `provider.sort=throughput` unless you override it.
- `eval/results/` is the main reproducibility archive for this repo.
