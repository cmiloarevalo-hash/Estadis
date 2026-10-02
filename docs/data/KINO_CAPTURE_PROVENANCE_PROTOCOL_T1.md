# Kino historical acquisition — snapshot, hash and provenance protocol

**Master:** #15  
**Checkpoint:** #16 (T1)  
**Date:** 2026-10-02  
**Status:** capture protocol only; no bulk acquisition.

## 1. Non-negotiable evidence rule

A normalized draw record is never evidence by itself.

For every future observation, preserve enough material to answer:

- which public source produced it;
- exact requested/final URL;
- when it was retrieved;
- what query was used;
- what bytes or rendered evidence were captured;
- how those bytes were hashed;
- what fields were extracted;
- whether the source is independent or derived from another source.

No snapshot/provenance => no VALIDATED record.

## 2. Stable source IDs

Use IDs from `sources/kino_historical_source_catalog_v1.json`.

Rules:

1. IDs are immutable once assigned.
2. URL migrations become aliases under the same ID.
3. A mirror/derivative gets its own ID but also `derived_from_source_id` or shared `independence_group`.
4. Corroboration counts independent evidence only when lineage is independent or explicitly verified.

## 3. Raw capture path

Future draw-level captures under Master #15 should use:

```text
data/raw/kino/<source_id>/<YYYY-MM-DD>/<capture_id>/
  request.json
  response.<html|json|pdf|txt|jpg|png|mp4|bin>
  metadata.json
```

T1 itself stores only source-catalog observations in:

```text
data/raw/kino/source_catalog/**
```

Do not overwrite an existing capture directory. A recapture creates a new `capture_id`.

## 4. capture_id

Recommended:

```text
<UTC_TIMESTAMP>__<draw-or-query-key>__<short-source-slug>
```

Example:

```text
2026-10-02T041500Z__draw-3286__sorteos-en-vivo
```

Timestamp is retrieval time, not draw time.

## 5. request.json

Minimum fields:

```json
{
  "source_id": "...",
  "requested_url": "...",
  "query": {
    "draw_number": null,
    "date_from": null,
    "date_to": null,
    "game": "kino"
  },
  "retrieved_at": "ISO-8601 with timezone",
  "capture_method": "manual_browser|public_download|authorized_other",
  "operator": "implementer",
  "work_item": 17
}
```

Do not store credentials, cookies, session secrets or personal data.

## 6. response snapshot

Hash the **exact raw bytes before parsing/normalization**.

Preferred digest:

```text
SHA-256
```

Store in `metadata.json`:

- `sha256`;
- byte length;
- media type;
- HTTP/status when observable;
- requested URL;
- final URL after redirects;
- retrieval timestamp;
- filename.

If a page is highly dynamic and raw HTML does not contain the visible result:
- preserve raw HTML when possible;
- additionally preserve a screenshot/PDF or other public rendered evidence;
- hash each artifact separately;
- document why rendered evidence was needed.

For videos:
- T1 does not download them;
- later Work Items should record URL, metadata and, only if reasonably/publicly downloadable and necessary, the exact media artifact/hash.

## 7. metadata.json

Minimum provenance:

```json
{
  "source_id": "...",
  "source_class": "OFFICIAL_DIRECT_RESULT",
  "independence_group": "official_loteria",
  "retrieved_at": "...",
  "requested_url": "...",
  "final_url": "...",
  "http_status": 200,
  "media_type": "text/html",
  "snapshot_path": "...",
  "sha256": "...",
  "content_length": 0,
  "capture_method": "manual_browser",
  "query_key": "draw:3286",
  "visible_fields": ["draw_number", "draw_date", "numbers"],
  "notes": []
}
```

## 8. Extraction provenance

A future processed record must retain links back to raw captures:

```text
source_id
capture_id
snapshot_path
source_sha256
requested/final URL
retrieved_at
extraction_method/version
record_version
```

One processed draw may have multiple source observations. Do not collapse them before conflict analysis.

## 9. Source classes

Use exactly:

- `OFFICIAL_DIRECT_RESULT`
- `OFFICIAL_STATISTICS`
- `OFFICIAL_INSTITUTIONAL`
- `SECONDARY_DATABASE`
- `SECONDARY_MEDIA`
- `COMMUNITY_DATASET`
- `COMMUNITY_DATASET_TOOL`
- `COMMUNITY_TOOL`

These classes describe provenance, not correctness.

## 10. Coverage statements

Use three levels:

- `VERIFIED`: directly observed examples/range in T1 or later captures.
- `SELF_STATED`: source/repository claims a range but T1 did not enumerate it.
- `NOT_VERIFIED`: unknown.

Never convert a self-stated range into verified coverage.

A visible first and last record do **not** prove continuity between them.

## 11. Independence and corroboration

Examples from T1:

- `77ruben/kino-scraper` scrapes `resultadoskinochile.com` => same evidence lineage.
- `Fernando8955/kino/kino-polla.json` declares `chileresultados.com` => same evidence lineage.
- `lotero.cl` and `loterochile.cl` use closely similar structures, but publisher relationship is not verified => treat independence as unknown until checked.
- official surfaces under Lotería are authoritative but may still share the same upstream official system; two Lotería pages are not automatically two operationally independent capture pipelines.

## 12. Corrections and source drift

If a source changes after capture:

- keep previous raw bytes;
- recapture to a new capture_id;
- never rewrite the old snapshot;
- compare hashes;
- document whether change is formatting-only or material.

If a source corrects a draw, preserve both versions and let later validation classify the record.

## 13. T1 boundary

Not performed here:

- mass draw enumeration;
- scraper/crawler code;
- automated endpoint discovery;
- statistical analysis;
- dataset normalization;
- confidence classification of draw records.

Those belong to #17–#25.
