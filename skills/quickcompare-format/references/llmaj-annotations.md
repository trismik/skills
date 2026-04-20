# LLM-as-Judge Annotations

Annotation columns are extra dataset columns the user's judge template can reference as `{{ row.<name> }}` in the QuickCompare UI. Good ones make judges more consistent and rubrics sharper.

This file covers **which annotations to suggest**. It does not cover writing the judge template — that's done in the UI.

## Mental model

Each row is a mini test case. The judge sees the model's response plus whatever is in the dataset. Anything you leave out, it cannot see. Common things a judge might need: what was asked, what a correct answer looks like, what must or must not appear, what style/length/format is expected, what counts as partial credit.

## The annotation columns worth knowing

| Column | What it's for |
|--------|---------------|
| `expected_answer` / `reference` | Gold answer. Most useful annotation when a correct answer exists. |
| `rubric` | Per-row rubric when "good" varies across rows. Stronger than a single global rubric. |
| `must_contain` | Facts, keywords, entities that must appear. Use a delimiter (`|`, commas, or JSON list). |
| `must_not_contain` | Banned content: wrong answers, forbidden phrases, competitor names. |
| `category` / `topic` | Tag for slicing results after the run. Usually ignored by the judge. |
| `context` | Background material the response depends on (RAG). The judge sees what the model saw. |
| `expected_format` | Output shape: JSON schema, number of bullets, markdown vs. plain, length bound. |
| `persona` / `audience` | Intended reader — affects tone and vocabulary. |
| `language` | Expected response language for multilingual datasets. |
| `difficulty_hint` | Optional human estimate, useful for post-hoc analysis. |

Example:

```csv
question,expected_answer,must_contain,category
"Who won the 2022 FIFA World Cup final?","Argentina","Argentina | France | penalties","sport"
```

## When LLMaJ is the right evaluation

LLMaJ shines for open-ended outputs: generation, summarisation, dialogue, instruction-following, RAG answer quality. For these, `rubric`, `must_contain`, `must_not_contain`, and `expected_format` are the annotations that most often move the needle.

For tasks with an objective correct answer (classification, extraction, exact-match Q&A), the user probably doesn't need LLMaJ at all — exact-match or label comparison works and is cheaper. If you notice the task is like this, say so before suggesting annotation columns.

(Base columns like `question` or `source_text` are in `column-design.md`, not here.)

## How to suggest annotations in conversation

Don't read a script. Have a short chat about how the user plans to judge, and suggest columns where they fit the answer.

Example prompts for you (pick one or two that fit — skip all of them if the user already knows what they want):

- "How will you know whether a response is good?"
- "Do you already have a reference answer for each row?" → `expected_answer`
- "Anything that must always appear, or must never appear?" → `must_contain` / `must_not_contain`
- "Does 'good' look the same for every row?" → `rubric` if it varies
- "Want to slice results by topic or difficulty later?" → `category`
- "Does output need a specific shape — JSON, bullets, length?" → `expected_format`

Stop as soon as you can propose columns. Every extra column is one more thing to fill in and maintain.

**Not your job:** writing the judge prompt, setting scoring scales, picking models. All done in the UI.