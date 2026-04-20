# Format Spec

Rules for QuickCompare dataset uploads and the parsing pitfalls users hit most.

## Accepted formats

**CSV** and **JSONL** only. Not Excel, TSV, Parquet, or Avro — convert first (Excel → Save As CSV UTF-8; anything else → pandas/polars → `to_csv` or JSONL).

## CSV

- Extension `.csv`, UTF-8 (BOM ok)
- Comma delimiter only (not `;`, not tab)
- First row is headers
- Quote any field containing commas, newlines, or quotes; escape quotes by doubling (`""`)
- `\n` or `\r\n` line endings both fine

```csv
question,expected_answer,category
"What is the capital of France?","Paris","geography"
"Who wrote ""Hamlet""?","Shakespeare","literature"
```

Common failures: semicolon/tab delimiters (European Excel), smart quotes from word processors, non-UTF-8 encodings (Windows-1252/Latin-1), whitespace in headers, embedded commas in unquoted fields.

## JSONL

- Extension `.jsonl` or `.ndjson`, UTF-8
- One JSON **object** per line — not an array, not pretty-printed
- Same keys across every line
- No blank lines
- Nested values work but show as JSON strings in the UI's column selector — prefer flat objects

```jsonl
{"question": "What is the capital of France?", "expected_answer": "Paris", "category": "geography"}
{"question": "Who wrote Hamlet?", "expected_answer": "Shakespeare", "category": "literature"}
```

Common failures: pretty-printed JSON split across lines, a single JSON array instead of JSONL, blank lines between records, trailing commas, mixed keys across rows.

## Limits

- 1–50,000 rows, ~50 MB
- ≥1 column (a single-column dataset is valid)
- No hard column cap, but very wide datasets preview slowly

Over limits: sample rows (200–2,000 is usually enough), drop unused columns, or strip long unused fields.

## Validation checklist

- [ ] Parses as UTF-8 without mojibake
- [ ] CSV comma-delimited, or JSONL one-object-per-line with no blank lines
- [ ] Same columns/keys on every row, no empties
- [ ] Within row and size limits
- [ ] No column named `response` or `row` (reserved), no spaces in column names
- [ ] A sample row round-trips correctly when printed back