# Issue #22 — T7 evidence matrix

T7 matched all 17 unique draw numbers observed in processed data.

## Relation counts

- MISSING_SOURCE: 11
- EXACT_MATCH: 5
- PARTIAL_MATCH: 1

## Important distinction

```text
EXACT_MATCH != VALIDATED
```

For draws 3281–3284 and 3286, three independent secondary publisher groups agree exactly on date and numbers. This is strong corroboration, but no official numeric observation was acquired, so confidence classification remains a separate T8 decision.

Draw 3285 has three agreeing secondary numeric observations plus one official institutional pre-draw observation that confirms draw number/date but lacks numbers; therefore the overall matrix relation is PARTIAL_MATCH.

Single-source/incomplete observations are marked MISSING_SOURCE rather than treated as conflicts.

## Output

- `data/processed/kino/evidence_matrix_v1.json`
- `reports/data-quality/kino/T7_EVIDENCE_MATRIX_REPORT.json`
