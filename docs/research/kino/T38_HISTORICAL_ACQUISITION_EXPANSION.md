# Issue #38 — Deep historical Kino acquisition expansion

**Master:** #15
**Retrieved:** 2026-10-02T02:30:00-03:00

## Outcome

A public versioned artifact materially expands the historical scale.

The historical commit `a498fd81bbb89c672f04c0dc53b414ee7da57ad0` of `gaaguile/Kino2026` contains `kino-history.json` with 3268 rows. For the modern 14-number regime, draws **#799 through #3267** are structurally complete and contiguous by draw number:

- modern 14-number complete rows: **2469**
- earliest complete: **#799 — Sunday, Jan 08, 2006**
- latest complete: **#3267 — Sunday, Aug 16, 2026**
- internal draw-number gaps: **0**
- observed depth: **2006–2026**

The same upstream commit contains `stats/estadisticas-kino.xlsx` and a `build-from-xlsx.ts` importer. The original publisher of that XLSX is not documented, so the bulk remains community/secondary evidence rather than official evidence.

## Cross-check

The Nicovh public CSV contains 889 Kino rows derived from ChileResultados. Across 889 overlapping draw numbers, all 889 14-number sets match the gaaguile historical artifact; numeric mismatches = 0.

Nicovh omits draw_date, so this is strong numeric corroboration but does not independently establish exact date+numbers for CONSENSUS_MEDIUM/HIGH.

## Broad search completed

The second pass evaluated deep pagination, per-draw archives, public datasets/exports and search surfaces, including ResultadosKinoChile, ChileResultados, Lotero, Sortuo, OpenLoto, KinoHistorico, Indicadores y Datos, GitHub CSV/JSON/XLS/XLSX sources and LoterAtor.

ResultadosKinoChile index pages were observed across 2016, 2017, 2018, 2019 and 2020. Indicadores y Datos states Kino coverage from draw #799 (2006) onward. ChileResultados exposes complete historical per-draw pages. These sources are useful but mass extraction would require automation prohibited by Master #15.

## Raw preservation

Exact downloadable public artifacts:
- `data/raw/kino/community/gaaguile/kino-history_a498fd81.json`
- `data/raw/kino/community/nicovh/kino_principal.csv`

Upstream commit/blob provenance is persisted in `data/raw/kino/community/deep_expansion_manifest_t38.json`.

Rows before #799 in the gaaguile artifact contain 15 numbers and remain raw only; they are excluded from the modern 14-number target dataset and are not silently altered.

## Automation-required sources

See `reports/data-quality/kino/T38_ACQUISITION_AUTOMATION_GAPS.json`.

For these sources:

```text
AUTOMATION_REQUIREMENT_IDENTIFIED
IMPLEMENTATION_NOT_AUTHORIZED
```

## Saturation reassessment

After broad web search, multiple-year archive testing, pagination checks, public dataset discovery and multi-source comparison:

```text
ACQUISITION SATURATION: REACHED_WITHIN_NO_AUTOMATION_SCOPE
```

This does not claim every historical HTML page was captured. It means further material bulk acquisition from the remaining deep archives requires prohibited automation.

## Next

Proceed immediately to #39 and rebuild the final auditable dataset.
