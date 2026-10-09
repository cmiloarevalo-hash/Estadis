# Issue #18 — T3 official Consulta Resultado acquisition

**Master:** #15  
**Source ID:** `kino.official.direct.consulta_resultado`  
**Retrieved:** 2026-10-02T01:24:00-03:00

## Result

The official Lotería page `https://www.loteria.cl/resultados/consulta-resultado/` is publicly reachable and identifies itself as the official draw-results consultation surface.

Within the permitted non-automated acquisition method, the static/indexed representation did not expose draw-level Kino rows, historical controls or a documented downloadable payload.

No data from #17 or from secondary sources was copied into this raw partition.

## Coverage

```text
DRAW-LEVEL RAW RECORDS ACQUIRED: 0
UNIQUE DRAWS: 0
COVERAGE: UNKNOWN
```

This is an acquisition limitation, not evidence that the source has zero historical coverage.

## Comparison with T2

At this checkpoint:

- T2 Sorteos en Vivo official observations: 8 draw-number-only records.
- T3 Consulta Resultado complete or incomplete draw observations: 0.
- Draws only in T2: 1829, 1836, 1842, 1843, 1856, 1858, 1892, 1903.
- Draws only in T3: none observed.
- Apparent cross-source conflicts: none, because T3 yielded no comparable draw fields.

## Access limitation

Obtaining the historical result payload from this dynamic official surface appears to require interactive form execution or discovery/use of underlying requests. Implementing such automation/endpoint extraction would cross the current no-scraper/no-crawler development boundary.

Therefore the limitation is preserved as `ACQUISITION_GAP` and the batch continues.

## Economic fields

No draw-level economic fields were acquired from this source in T3. All remain UNKNOWN.

## Raw evidence

`data/raw/kino/consulta_resultados/interface_observation_2026-10-02.json`

The captured representation is hashed with SHA-256.
