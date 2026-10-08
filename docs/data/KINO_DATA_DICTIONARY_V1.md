# Diccionario de datos — Kino histórico v1.1.0 (freeze de #39)

**Master:** #15 · **Work Item:** #39 · **Freeze:** 2026-10-08 · **Tipo:** registros históricos y documentación, no producto ejecutable.

## Unidad y clave

Cada sorteo moderno se identifica mediante `draw_id = kino:<draw_number>`. Un sorteo puede tener varias **observaciones** por fuente, pero exactamente un estado global de confianza consolidado.

- `draw_number`: entero recuperado de fuente, nunca inferido por fecha.
- `draw_date`: fecha ISO `YYYY-MM-DD` observada; `null` si la observación no la contiene. En particular, las 889 filas Nicovh no tienen fecha de origen aunque otros registros del mismo sorteo sí la aportan.
- `numbers`: arreglo de 14 enteros distintos 1–25 ordenados; `null` si la fuente no publica todos.
- `game`, `modality`, `rule_regime_id`: base de juego, modalidad y límite de régimen. Antes del sorteo 799, las 799 filas comunitarias premodernas tienen 15 números, se mantienen en raw y NO se convierten a 14; desde #2893 cambia el régimen documentado de juegos adicionales.
- `validation_status`: validez estructural de observación, distinta de confianza/autoridad global.
- `confidence_status`: exactamente un valor `VALIDATED | PROVISIONAL | CONFLICTED | INCOMPLETE | REJECTED` por sorteo consolidado.
- `evidence_grade`: `OFFICIAL_CORROBORATED | CONSENSUS_HIGH | CONSENSUS_MEDIUM | SINGLE_SOURCE | INSUFFICIENT`; no es un reemplazo de `confidence_status`.

## Procedencia reversible

Campos por observación: `observation_id`, `source_id`, `source_url`, `source_type`, `independence_group`, `retrieved_at`, `raw_reference`, `raw_row_index` o `raw_csv_line` cuando corresponda, `raw_file_blob_sha`, `record_version`, `missing_core_fields`. La comparación por sorteo conserva referencias a `source_observation_ids` y la matriz de evidencia preserva las fuentes no seleccionadas como representación canónica.

**Importante:** el SHA-1 del blob Git identifica el archivo raw comprometido en Git; NO comprueba que una transcripción corresponda byte a byte al HTML externo. Un `snapshot_hash` anterior identifica el fragmento documentado cuando procede, no el HTML remoto completo.

## Almacenamiento canónico particionado

- Normalización: 6 archivos `data/processed/kino/observations_c39_*.json`, 3.401 observaciones modernas.
- Matriz: 5 archivos activos `data/processed/kino/evidence_matrix_c39_*.json` (rango anual sin solapes).
- Consolidación: 5 archivos `data/processed/kino/consolidated_c39_*.json`, 2.488 claves únicas.
- Dataset estricto: `data/validated/kino/dataset_v1.json`, **cero** `VALIDATED`.
- Cuarentena: `data/quarantine/kino/dataset_v1.json` es un **manifiesto** con rutas a 5 particiones `provisional_c39_*.json`, total **2.488 PROVISIONAL**. No es un archivo que contenga las 2.488 filas inline.
- Investigación alta confianza: `data/processed/kino/high_confidence_research_dataset_c39_v1.json`, seis registros **PROVISIONAL / CONSENSUS_HIGH**. No es un conjunto oficialmente validado ni autoriza por sí solo análisis de producto.
- Rechazados: `data/rejected/kino/dataset_v1.json` (cero). Conflictos: `reports/data-quality/kino/conflict_log_v1.json` (cero contradicciones materiales *observadas*).
- Legado excluido: `data/processed/kino/legacy_regime_exclusion_c39_v1.json`.

## Campos económicos y ausencia

`prizes_by_category`, `winners` (dentro de cada categoría), `jackpot_estimate_total_clp`, `ticket_price`, `tickets_sold`, `sales_amount`, y cuando exista `carryover` o `prize_pool`. `null` y un campo sin evidencia significan **NO ADQUIRIDO / DESCONOCIDO**, nunca cero. Un pozo promocional estimado no es pozo efectivamente pagado ni ventas.

Cobertura de 2.488 sorteos observados: resultados de 14 números 2.488 (0 oficialmente validados); premios y ganadores 7; estimaciones de pozo 2; ventas y precio de boleto 0. Ver `reports/data-quality/kino/C39_5_ECONOMIC_FIELD_COVERAGE.json`.

## Reglas de aceptación

No se infiere independencia por coincidencia de números. Nicovh deriva de ChileResultados y carece de fecha; el origen editorial del XLSX gaaguile no está demostrado. No existe un sorteo que cumpla los requisitos de `VALIDATED` en este freeze. La continuidad de números de sorteo #799–#3286 no certifica cobertura temporal oficial completa. `DATASET_ANALYSIS_READY: NO` y Master #26 permanece bloqueado.

Los archivos de checkpoints previos a C39 se conservan como evidencia histórica, **no** deben confundirse con la vista canónica de v1.1.0 indicada aquí.
