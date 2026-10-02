# Kino historical dataset v1 — data dictionary

Dataset: `kino-history-v1.0.0`

Core fields:
- draw_id: `kino:<draw_number>`.
- draw_number: source-traceable; never calendar-inferred.
- draw_date: ISO date or null when unobserved.
- numbers: sorted 14-number Kino base set or null.
- game / modality / rule_regime_id.
- confidence_status: VALIDATED | PROVISIONAL | CONFLICTED | INCOMPLETE | REJECTED.

Provenance fields:
- source_id / source_url / source_type / independence_group.
- retrieved_at / raw_reference / raw_file_blob_sha / snapshot_hash.
- record_version.

Economic fields may include ticket_price, addon_price, jackpot/carryover, prize_pool, prizes_by_category, winners_by_category, tickets_sold, sales_amount, draw_time, machine_id, ball_set_id, venue, procedure.

Null means UNKNOWN/not acquired, never zero.

Only VALIDATED is analysis-ready. Dataset v1 contains zero VALIDATED draws because the permitted official interfaces did not yield complete 14-number historical result evidence.
