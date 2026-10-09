# Issue #16 — Exhaustive source map and capture protocol for historical Kino

**Master Work Item:** #15  
**Checkpoint:** T1 / Issue #16  
**Base:** `main@b3d197042afcaa093fa4c208710dfe70698b18c8`  
**Consulted:** 2026-10-02  
**Mode:** source discovery + provenance design only.

## 1. Result

T1 identified a broad set of official, secondary and community sources that may contribute historical Kino evidence.

Important distinction:

```text
DISCOVERED SOURCE != VERIFIED COVERAGE
VISIBLE FIRST/LAST EXAMPLE != CONTINUOUS ARCHIVE
DERIVED COPY != INDEPENDENT CORROBORATION
```

No bulk result acquisition was performed.

The canonical structured inventory is:

`sources/kino_historical_source_catalog_v1.json`

## 2. Authority order

### Official direct result

1. `kino.official.direct.sorteos_en_vivo`
2. `kino.official.direct.consulta_resultado`

These are the preferred draw-level evidence surfaces.

### Official statistics

3. `kino.official.stats.estadisticas`

Lotería explicitly states its historical statistics are manually compiled and may contain errors. Therefore they are official but still require preservation and cross-checking.

### Official institutional/supporting

4. `kino.official.institutional.novedades`
5. `kino.official.institutional.ganadores`
6. `kino.official.institutional.reglamentacion`

These are valuable for schedules, reprogramming, winners, special draws and rule-regime changes, but are not assumed to be complete draw histories.

## 3. Official interfaces — verified capabilities

### Sorteos en Vivo

URLs:
- https://www.sorteosenvivo.cl/sorteosenvivo
- https://loteria.cl/sorteosenvivo

Visible controls:
- game selector;
- search by draw number;
- date from;
- date to;
- recent videos.

Verified limitation:
- videos are explicitly described as available for the last 60 days.

Not verified in T1:
- earliest queryable result;
- bulk export;
- documented API;
- exact schema returned by historical lookup.

This is intentionally deferred to #17.

### Consulta Resultado

URL:
- https://www.loteria.cl/resultados/consulta-resultado/

The official result-query surface is public, but static indexing exposes little detail. Exact query controls, returned fields and historical depth remain to be characterized in #18.

### Estadísticas

URL:
- https://www.loteria.cl/resultados/estadisticas/

Official terms:
- https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=874&zoneid=47

Verified fact:
Lotería makes historical statistics public and warns they are manually compiled and may contain errors.

Exact historical range/export/schema remains unverified.

## 4. Secondary databases with draw-level potential

### ResultadosKinoChile.com

`source_id: kino.secondary.db.resultadoskinochile`

URLs:
- https://www.resultadoskinochile.com/resultados-kino/
- per-draw archive pages.

Verified examples:
- draw 1505 — 2012-12-16;
- draw 1506 — 2012-12-19;
- archive page 282 exposes 1505–1509;
- current archive/search exposes draw 3286 — 2026-09-30.

Fields visible on many pages:
- draw number;
- date;
- 14 Kino numbers;
- prize categories;
- winners;
- prize amounts;
- additional games on modern pages.

Conclusion:
- strongest long-range secondary archive discovered in T1;
- range endpoints are visible, but continuity between 1505 and 3286 is **not yet verified**.

### ChileResultados.com

`source_id: kino.secondary.db.chileresultados`

URL:
- https://chileresultados.com/kino

Capabilities:
- previous draws;
- search older draws by date;
- per-draw pages keyed by draw number.

Verified examples include:
- 2396 / 2021-01-22;
- 2432 / 2021-04-16;
- 2982 / 2024-10-20;
- 3070 / 2025-05-14.

Fields:
- draw number/date;
- 14 numbers;
- prize/winner tables;
- several additional modalities.

The site explicitly states its data are **not official** and may contain errors.

### LoteroChile / Lotero

`kino.secondary.db.loterochile`  
`kino.secondary.db.lotero`

Both expose dense draw pages with numbers and prizes.

T1 does not establish whether the two domains are independent publishers or mirrors/related deployments. Until lineage is checked, they **must not** be counted as two independent corroborating sources.

### OpenLoto

`kino.secondary.db.openloto`

Visible current/recent draw number, date, 14 numbers and historical links. Site explicitly states it is independent from official lottery entities.

### Loteria.guru

`kino.secondary.db.loteria_guru`

Visible dates + 14 numbers and frequency summaries. The site states its Kino statistics cover results from 2021 to latest. That lower bound is `SELF_STATED`, not T1-enumerated.

### KinoHistorico.cl

`kino.secondary.db.kinohistorico`

Verified:
- draw-number search form;
- historical-result service.

Not verified:
- upstream source lineage;
- earliest record;
- continuity;
- export.

### Indicadores y Datos

`kino.secondary.db.indicadoresydatos`

Visible table fields:
- draw number;
- capture date;
- 14 extracted numbers.

T1 observed recent rows around 3264–3283. Earlier coverage not established.

### Sortuo

`kino.secondary.db.sortuo`

Visible “local archive” examples 3272–3274 with draw/date/14 numbers. Exact source lineage and historical depth are not established.

### Yelu

`kino.secondary.db.yelu`

Generic Lotería de Concepción history UI with game/month filtering and fields for date/game/winning numbers/draw number. Kino-specific depth remains unverified.

## 5. Secondary media

These are useful for isolated corroboration, not as presumed complete datasets:

- `kino.secondary.media.24horas` — topic archive with repeated result articles across 2025–2026;
- `kino.secondary.media.epicentrochile` — sampled draws 3284/3285 with 14 numbers and prize detail;
- `kino.secondary.media.prensadigital` — sampled draw 3285.

Media articles may be especially useful to resolve a conflict for a specific draw, but absence of an article is not a coverage gap in the lottery itself.

## 6. Community repositories / datasets

### 77ruben/kino-scraper

`source_id: kino.community.github.77ruben_kino_scraper`

README declares:
- source = resultadoskinochile.com;
- approx coverage 1506 (Dec 2012) → 3226 (May 2026);
- CSV columns sorteo, fecha, n1..n14, url.

Important:
- no license is declared;
- the current repository tree does not contain the declared CSV output;
- this is **derived from ResultadosKinoChile** and cannot corroborate it independently.

### gaaguile/Kino2026

`source_id: kino.community.github.gaaguile_kino2026`

The repository contains `kino-history.json` and XML.

T1 verified:
- records begin at 2023-11-17;
- later records extend into 2026;
- early `drawNumber` is a local 1,2,3... sequence;
- the file later mixes in official-style numbers such as 3273.

This identity ambiguity makes the file unsuitable for direct trust. If acquired in #20, it must enter quarantine until reconciled.

### Blank2D/datos-de-azar

`source_id: kino.community.github.blank2d_datos_de_azar`

MIT licensed. Code documents:
- official latest-result URL;
- KinoHistorico fallback for history.

README states historical seed is still incomplete. Therefore T1 classifies it as a **tool/reference**, not a verified historical dataset.

### Fernando8955/kino

`source_id: kino.community.github.fernando8955_kino`

T1 found:
- `kino-polla.json` with official-numbered recent draws and explicit source `chileresultados.com`;
- `resultados.json` with daily numbering/dates inconsistent with normal Kino base schedule and therefore not reliable as Kino base evidence.

This repository is mixed-quality. Its usable file is derived from ChileResultados and is not independent corroboration.

## 7. Public-source exclusions

Observed but excluded as primary public sources:
- https://agentes.loteria.cl/Home — login protected.

Authenticated/agent-only material must not be treated as a public source under Master #15.

## 8. Source ID and independence rules

Stable ID format:

`kino.<authority>.<class>.<slug>`

Examples:
- `kino.official.direct.sorteos_en_vivo`
- `kino.secondary.db.resultadoskinochile`
- `kino.community.github.77ruben_kino_scraper`

Do not rename a source_id when a URL changes. Record the new URL as an alias.

Every source also carries an `independence_group`.

This prevents double counting:
- scraper + its scraped website;
- mirror + origin;
- downstream GitHub JSON + upstream website.

## 9. Snapshot/hash/provenance

Protocol is defined in:

`docs/data/KINO_CAPTURE_PROVENANCE_PROTOCOL_T1.md`

Core rule:

```text
hash exact raw bytes first
→ preserve snapshot
→ then parse/normalize
→ keep source_id + capture_id + sha256 on every derived record
```

## 10. Coverage status after T1

Verified:
- multiple official public result/statistics surfaces exist;
- Sorteos en Vivo supports draw-number/date filtering;
- ResultadosKinoChile has publicly discoverable examples from draw 1505 / 2012-12-16 through 3286 / 2026-09-30;
- multiple secondary databases can corroborate recent draws;
- community datasets/tools exist but have lineage/licensing/data-quality caveats.

Not verified:
- earliest official draw accessible through Lotería interfaces;
- complete official continuity;
- complete ResultadosKinoChile continuity;
- bulk official export/API;
- exact Kino statistics schema/range;
- independence of similar secondary domains;
- completeness of any community dataset.

## 11. Next acquisition order

The source map supports the Master sequence:

```text
#17 Sorteos en Vivo
#18 Consulta oficial
#19 official complementary sources
#20 secondary/community bases
#21–#25 normalize, compare, validate, freeze v1
```

T1 itself stops before bulk acquisition.
