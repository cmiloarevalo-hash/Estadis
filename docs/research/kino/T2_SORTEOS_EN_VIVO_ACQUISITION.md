# Issue #17 — T2 Sorteos en Vivo official acquisition

**Master:** #15  
**Source ID:** `kino.official.direct.sorteos_en_vivo`  
**Retrieved:** 2026-10-02T01:24:00-03:00  
**Mode:** public/manual/indexed evidence only; no scraper/crawler.

## Result

The public Sorteos en Vivo interface was verified as supporting:

- game selection;
- search by draw number;
- date-from/date-to filtering;
- recent video access.

The interface itself states that videos correspond to the last 60 days.

A web-indexed historical footprint also exposed individual official video pages for Kino draw numbers:

`1829, 1836, 1842, 1843, 1856, 1858, 1892, 1903`.

Each observed page identifies the product as Kino and the draw number, but the indexed representation does **not** expose:

- draw date;
- the 14 drawn numbers;
- prize/winner tables.

Therefore these observations are preserved as official but incomplete evidence.

## Raw persistence

`data/raw/kino/sorteos_en_vivo/indexed_video_observations_2026-10-02.json`

Each raw observation retains:
- source_id;
- exact source URL;
- retrieval timestamp;
- captured textual representation;
- SHA-256 of that representation;
- explicit missing-core-field list.

No numbers from any secondary source were copied into this official raw partition.

## Coverage

```text
EARLIEST OBSERVED DRAW NUMBER: 1829
LATEST OBSERVED DRAW NUMBER: 1903
OBSERVED OFFICIAL DRAW PAGES: 8
NUMERIC RESULTS ACQUIRED: 0
COVERAGE: PARTIAL
```

The range 1829–1903 is **not** claimed continuous. It represents only indexed pages actually observed.

## Access limitation

The current public interface is dynamic. Within the allowed no-code/no-crawler method, T2 could verify historical query capability and individual official draw/video pages, but could not obtain the 14-number historical result payload from the static indexed representation.

Exhaustive interaction with the query result surface would require browser automation or endpoint-level acquisition beyond this Work Item's allowed technique.

This is recorded as an **ACQUISITION_GAP**, not as missing lottery draws.

## Economic fields

For the observed historical video pages:
- ticket_price: UNKNOWN
- addon_price: UNKNOWN
- jackpot: UNKNOWN
- prize_pool: UNKNOWN
- winners_by_category: UNKNOWN
- tickets_sold: UNKNOWN
- sales_amount: UNKNOWN
- machine_id: UNKNOWN
- ball_set_id: UNKNOWN
- venue: UNKNOWN
- procedure: UNKNOWN

No values were inferred.

## Verification

- source_id and URL present on every raw observation;
- SHA-256 present for each captured representation;
- no raw overwrite;
- no secondary-source substitution;
- no executable acquisition artifact created.

## Handoff to #18

T2 provides an official-source footprint but no complete draw records. T3 must independently examine `kino.official.direct.consulta_resultado`.
