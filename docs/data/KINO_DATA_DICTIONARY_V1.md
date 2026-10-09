# Diccionario de datos — Kino histórico v1.1.1 (freeze de #39)

**Master:** #15 · **Work Item:** #39 · **Freeze original:** 2026-10-08 · **REWORK:** 2026-10-09 · **Tipo:** registros históricos y documentación, no producto ejecutable.

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
- Investigación alta confianza: `data/processed/kino/high_confidence_research_dataset_c39_v1.json`, **0 registros**. Los seis antes etiquetados CONSENSUS_HIGH permanecen PROVISIONAL en cuarentena y pasan a evidencia INSUFFICIENT hasta demostrar independencia positiva.
- Rechazados: `data/rejected/kino/dataset_v1.json` (cero). Conflictos: `reports/data-quality/kino/conflict_log_v1.json` (cero contradicciones materiales *observadas*).
- Legado excluido: `data/processed/kino/legacy_regime_exclusion_c39_v1.json`.

## REWORK #39 — semántica de autoridad e independencia (2026-10-09)

- `PROVISIONAL` se interpreta **únicamente como retención** de un resultado estructuralmente completo que no cumple `VALIDATED`, incluyendo registros comunitarios o secundarios sin corroboración. No implica credibilidad verificada.
- `source_authority` conserva el tipo de la publicación de la cual proviene el candidato elegido; `source_type` y `official_complete_numeric_sources` no se alteran. Una referencia reglamentaria puede confirmar fecha de evento sin validar los 14 números.
- `independence_evidence_status` distingue origen no verificado de publicación numérica única; `verified_independent_numeric_source_count` cuenta **sólo** pruebas positivas de procedencias independientes, no grupos inferidos por dominio.
- `distinct_numeric_publication_count` y `matching_numeric_publication_source_ids` cuentan publicaciones coincidentes en 14 números. `matching_full_date_numeric_publication_count` y `matching_date_numeric_publication_source_ids` exigen adicionalmente coincidencia de fecha. **Coincidencia no equivale a independencia.**
- `reported_secondary_publisher_groups` conserva los grupos nominales de la versión previa; `independent_full_secondary_publishers` queda vacío donde no se documentó independencia verificable. `corroborating_sources` queda vacío mientras no haya fuentes calificadas para corroboración independiente; los `source_observation_ids` originales permanecen intactos.
- `calendar_evidence_status` es evidencia de la fecha observada, nunca certificación externa de todos los eventos. `numeric_result_evidence_status` separa el estado del resultado de 14 números de esa cronología.
- `SINGLE_SOURCE`: 1.592 sorteos de procedencia numérica única; `INSUFFICIENT`: 896 sorteos con varias publicaciones numéricas coincidentes, pero linaje/editorialidad independiente pendiente (889 gaaguile/Nicovh numéricos sin fecha Nicovh, otros 7 recientes con coincidencia fecha+números). `CONSENSUS_HIGH=0`, `CONSENSUS_MEDIUM=0`, `VALIDATED=0`.
- Nicovh **deriva** de ChileResultados; gaaguile posee un origen XLSX **UNKNOWN** y otro script distinto de su repositorio numera filas artificialmente, sin que esté probado que se usara para generar el JSON congelado. Detalle en [R41.7](https://github.com/cmiloarevalo-hash/Estadis/issues/41#issuecomment-6071663636).
- Regímenes calendarios observados: domingo (#799–#816); miércoles/domingo (#817–#2260); miércoles/viernes/domingo (desde #2261). La fecha candidata `2009-12-23` no tiene sorteo asignado: excepción **UNVERIFIED**, no se añade una observación.

## Campos económicos y ausencia

`prizes_by_category`, `winners` (dentro de cada categoría), `jackpot_estimate_total_clp`, `ticket_price`, `tickets_sold`, `sales_amount`, y cuando exista `carryover` o `prize_pool`. `null` y un campo sin evidencia significan **NO ADQUIRIDO / DESCONOCIDO**, nunca cero. Un pozo promocional estimado no es pozo efectivamente pagado ni ventas.

Cobertura de 2.488 sorteos observados: resultados de 14 números 2.488 (0 oficialmente validados); premios y ganadores 7; estimaciones de pozo 2; ventas y precio de boleto 0. Ver `reports/data-quality/kino/C39_5_ECONOMIC_FIELD_COVERAGE.json`.

## Reglas de aceptación

No se infiere independencia por coincidencia de números. Nicovh deriva de ChileResultados y carece de fecha; el origen editorial del XLSX gaaguile no está demostrado. No existe un sorteo que cumpla los requisitos de `VALIDATED` en este freeze. La continuidad de números de sorteo #799–#3286 no certifica cobertura temporal oficial completa. `DATASET_ANALYSIS_READY: NO` y Master #26 permanece bloqueado.

Los archivos de checkpoints previos a C39 se conservan como evidencia histórica, **no** deben confundirse con la vista canónica de v1.1.1 indicada aquí.
