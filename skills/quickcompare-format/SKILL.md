---
name: quickcompare-format
description: Prepare a dataset for upload to Trismik QuickCompare. Use when the user wants to upload their own data (CSV or JSONL), needs advice on column naming, or wants to add annotation columns that an LLM-as-Judge evaluation can reference. Scope is strictly data formatting — judge prompt templates, model selection, metrics, privacy review, and synthetic data generation are out of scope and should be deferred to the Trismik docs or a separate conversation.
---

# QuickCompare Format

Help the user shape a dataset they can upload to **Trismik QuickCompare** (one row per prompt; accepts CSV or JSONL).

**In scope:** file format, column choice, column names, LLM-as-Judge annotation columns.
**Out of scope:** judge prompts, model/metric choice, PII review, synthetic data, anything in the UI beyond upload. Redirect to the Trismik docs.

## Workflow

1. **Understand the task.** Ask what they're evaluating, whether they have reference answers, and what format their raw data is in. Nothing more.
2. **Decide columns and names.** See `references/column-design.md`. Default to keeping the user's existing names — only rename if missing, non-descriptive, reserved (`response`, `row`), or containing spaces.
3. **Offer annotations if LLM-as-Judge is planned.** Extra columns become `{{ row.<name> }}` in the UI's judge template (you don't write the template). See `references/llmaj-annotations.md`. Don't run a checklist — ask an open-ended question or two about how they'll judge responses and suggest columns that fit the answer.
4. **Convert and validate.** See `references/format-spec.md`. Output in CSV or JSONL, UTF-8, consistent columns per row, ≤50k rows / ~50 MB. Show a 3-row preview and the final row count.

## Quick column picker

| User goal | Columns |
|-----------|---------|
| Single-prompt comparison | `input` |
| Q&A with known answer | `input`, `expected_answer` |
| Classification | `input`, `label` |
| Summarisation | `source_text`, `reference_summary` |
| Translation | `source_text`, `expected_translation` |
| RAG | `context`, `question`, `expected_answer` |

A single-column dataset is valid — don't push ground-truth columns on users who don't need them.

## References

- `references/format-spec.md` — CSV/JSONL rules, limits, parsing pitfalls
- `references/column-design.md` — column choice, naming, rename-vs-keep
- `references/llmaj-annotations.md` — annotation columns for LLM-as-Judge