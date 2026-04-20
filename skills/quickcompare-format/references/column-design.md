# Column Design

Choosing and naming columns for a QuickCompare dataset.

**Core principle:** only add columns the user will use — as the prompt, as ground truth, or as an annotation the judge can reference.

## How many columns

- **One column** is valid and often enough: open-ended generation, "see how models respond to the same prompt," or LLM-as-Judge rubrics that don't need a gold answer.
- **Two columns** (`input` + `expected_answer`) is the common case — needed for non-LLM metrics (exact match, BLEU, ROUGE) and useful for judges comparing to a gold answer.
- **Three+ columns** when the task has real structure: RAG (`context`, `question`, `expected_answer`), few-shot, classification with per-row options, multi-turn dialogue. Multi-column inputs are combined via a Jinja2 template in the UI's advanced input mode, so pick names that read naturally: `{{ row.context }}` then `{{ row.question }}`.

Split one prompt string into multiple columns only when the parts vary independently and the user benefits from rewriting the wrapper without rewriting the data. Otherwise one column is simpler.

## Naming: keep vs. rename

**Default: keep the user's existing names.** `Question`, `userPrompt`, `gold_answer`, `answer` — all fine. Their scripts and notes reference them. Don't rewrite for style.

**Rename only when:**

| Problem | Example | Fix |
|---------|---------|-----|
| Missing or auto-generated | `col1`, `Unnamed: 0`, `field_3` | Give a descriptive name |
| No semantic content | `x`, `y`, `data`, `a` | Give a descriptive name |
| Reserved (WILL break things) | `response`, `row` | Rename to e.g. `expected_answer`, `row_id` |
| Spaces or non-ASCII punctuation | `expected answer` | `expected_answer` (hard requirement — breaks Jinja2) |
| Inconsistent casing across rows/files | mix of `Question` / `question` | Pick one, apply everywhere |

If you rename, tell the user what changed and why.

**Leave alone** (stylistic, not problems): `Answer`/`Question` (PascalCase), `userPrompt` (camelCase), short names like `answer`, abbreviations like `gt` or `ref` if the user clearly knows what they mean.

**When you *do* introduce a new name** (filling in a missing header, new annotation column), prefer snake_case lowercase ASCII and descriptive over cryptic. But don't retrofit this style onto the user's existing names.

Column names are case-sensitive in Jinja2 — whatever casing is chosen must be consistent across every row.

## Default names by task (for new datasets only)

Use these when there are no existing names. Otherwise keep what the user has.

| Task | Columns |
|------|---------|
| Open-ended / generation | `prompt` or `instruction` |
| Q&A | `question`, `expected_answer` (+ `context` for RAG, `category` for slicing) |
| Classification | `text` or `input`, `label` (+ `options` if per-row) |
| Summarisation | `source_text`, `reference_summary` (+ `target_length`) |
| Translation | `source_text`, `target_language`, `expected_translation` |
| Code generation | `task_description`, `function_signature`, `test_cases` |
| Instruction following | `instruction`, `input`, `expected_answer` |
| Agent / tool use | `user_query`, `expected_tool_calls` |
| Dialogue | `conversation_history`, `next_user_message`, `expected_reply` |