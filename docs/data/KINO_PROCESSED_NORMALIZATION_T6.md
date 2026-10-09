# Issue #21 — T6 processed normalization

All draw-level raw observations from T2–T5 were normalized into one reversible processed representation.

## Counts

- processed observations: 30
- unique draw numbers: 17
- VALID_STRUCTURAL observations: 19
- INCOMPLETE observations: 11
- INVALID_STRUCTURAL observations: 0

## Canonical fields

Each processed observation contains:
- draw_number;
- draw_date (ISO or null);
- numbers sorted ascending when present;
- game/modality;
- source_id/source_url/source_type;
- independence_group;
- retrieved_at;
- rule_regime_id;
- raw_reference;
- raw_file_blob_sha;
- SHA-256 snapshot hash when the raw capture had one;
- record_version;
- structural validation status/errors;
- economic fields when captured, otherwise null/UNKNOWN.

## Structural rules

A complete numeric observation is structurally valid only when:
- exactly 14 numbers;
- all distinct;
- all integers 1–25;
- draw_date present;
- draw_number present.

No missing date or number was inferred from calendar, neighboring draws or another source.

## Reversibility

Every processed row links to its source-specific raw file. Raw files were not modified.

## Output

`data/processed/kino/observations_v1.json`
