# Issue #20 — T5 secondary-source acquisition

**Master:** #15  
**Retrieved:** 2026-10-02T01:24:00-03:00

## Acquired draw-level sources

T5 manually persisted recent Kino base observations from three independent secondary publisher groups:

1. ResultadosKinoChile.com — draws 3280–3286.
2. ChileResultados.com — draws 3281–3286.
3. Epicentro Chile — draws 3281–3286.

No source is treated as official.

## Draw coverage acquired

| Draw | Date | ResultadosKinoChile | ChileResultados | Epicentro |
|---:|---|---:|---:|---:|
| 3280 | 2026-09-16 | yes | no T5 capture | no T5 capture |
| 3281 | 2026-09-18 | yes | yes | yes |
| 3282 | 2026-09-20 | yes | yes | yes |
| 3283 | 2026-09-23 | yes | yes | yes |
| 3284 | 2026-09-25 | yes | yes | yes |
| 3285 | 2026-09-27 | yes | yes | yes |
| 3286 | 2026-09-30 | yes | yes | yes |

All acquired sources agree on date and 14-number set for the overlapping draws in this T5 sample.

This agreement is **not sufficient by itself for VALIDATED** because the numeric results still lack official draw-result evidence.

## Economic fields

ResultadosKinoChile and ChileResultados expose base-category prize totals, winner counts and per-winner payouts. T5 preserved these fields for categories 10–14 where transcribed.

No claim is made for:
- tickets_sold;
- sales_amount;
- machine_id;
- ball_set_id;
- venue/procedure.

Those remain UNKNOWN.

## Derived/community sources

The following were reviewed but not counted as independent draw evidence:
- `77ruben/kino-scraper` derives from ResultadosKinoChile.
- `Fernando8955/kino` contains `kino-polla.json` derived from ChileResultados.
- `gaaguile/Kino2026` contains draw identity ambiguity and no verified upstream provenance.
- `Blank2D/datos-de-azar` is tooling/reference; it did not persist a complete historical seed.

## Raw paths

- `data/raw/kino/secondary/resultadoskinochile/recent_3280_3286.json`
- `data/raw/kino/secondary/chileresultados/recent_3281_3286.json`
- `data/raw/kino/secondary/epicentrochile/recent_3281_3286.json`

## Coverage

```text
SECONDARY DRAW OBSERVATIONS ADDED: 19
UNIQUE DRAWS FROM T5: 7
COVERAGE: PARTIAL
```

T1 discovered much deeper secondary archives, but exhaustive enumeration would require prohibited automation. T5 therefore prioritizes an auditable cross-source recent sample.
