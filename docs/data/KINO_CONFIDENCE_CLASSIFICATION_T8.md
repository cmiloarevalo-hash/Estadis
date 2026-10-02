# Issue #23 — T8 confidence classification

Each of the 17 unique observed draws now has exactly one global confidence status.

## Counts

- VALIDATED: 0
- PROVISIONAL: 7
- CONFLICTED: 0
- INCOMPLETE: 10
- REJECTED: 0
- TOTAL: 17

Check: 17 = 17 unique draws.

## Interpretation

The seven recent draws 3280–3286 are PROVISIONAL. Their numeric observations are structurally valid and, for 3281–3286, multiple independent secondary publishers agree. However no official source acquired in T2–T4 exposes the full 14-number result, so these draws are not promoted to VALIDATED.

The older official observations (video/index or institutional metadata) are INCOMPLETE because they lack one or more core fields, principally numbers[14] and in T2 also draw_date.

No material number/date conflict was observed in the acquired sample, and no record was proven invalid enough to classify as REJECTED.

## Partitions

- `data/validated/kino/validated_v1_candidate.json`
- `data/quarantine/kino/provisional_v1.json`
- `data/quarantine/kino/conflicted_v1.json`
- `data/quarantine/kino/incomplete_v1.json`
- `data/rejected/kino/rejected_v1.json`

Only VALIDATED records are analysis-ready. At this checkpoint the validated partition is intentionally empty.
