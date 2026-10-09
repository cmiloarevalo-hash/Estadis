# Master #15 — Kino: auditoría histórica final v1.1.1

**Work Item:** #39 (C39.1–C39.6) · **Freeze inicial:** 2026-10-08 · **REWORK:** 2026-10-09 · **PR:** #37 · **Rama:** `data-kino-historical-acquisition-v1`.

**Estado:** `READY_FOR_REVIEW` del paquete documental; **no** es `SEMANTIC_ACCEPTED` y **no** autoriza merge ni desarrollo.

## Dictamen de preparación

```text
DATASET_ANALYSIS_READY: NO
MASTER #26: BLOCKED_BY_DATA_QUALITY_OR_COVERAGE
DEVELOPMENT: NOT STARTED
DEVELOPMENT_GATE: BLOCKED_PENDING_HUMAN_SUPERVISOR
```

El volumen supera el piso de conteo bruto de 1.000 sorteos completos, pero **cero** sorteos poseen `CONSENSUS_HIGH` con independencia editorial positivamente verificada y **ninguno** cumple `VALIDATED`. Por tanto no satisface el mínimo de 500 sorteos completos de alta confianza exigido para considerar un análisis limitado; tampoco existe cobertura económica histórica suficiente.

## Métricas finales, denominadores explícitos

| Métrica | Resultado |
|---|---:|
| Fuentes únicas en catálogo reconciliado | **26** |
| Fuente de discrepancia | #38 informó 25; unión del catálogo T1 (23) + nuevos IDs T38 (3) = **26** |
| IDs de fuente con observaciones de sorteo procesadas | **11** |
| Grupos de origen representados en observaciones | **7** (9 evaluados por #38) |
| Snapshots raw inmutados | **12** |
| Filas raw de sorteo, incluidas 799 legadas incompatibles | **4.200** |
| Observaciones modernas procesadas | **3.401** |
| Sorteos modernos únicos y completos, observados | **2.488** |
| VALIDATED | **0** |
| PROVISIONAL | **2.488** |
| CONFLICTED | **0** |
| INCOMPLETE (sorteo consolidado) | **0** |
| REJECTED (sorteo consolidado) | **0** |
| Observaciones individuales INCOMPLETE | **900** (889 Nicovh sin fecha y 11 observaciones oficiales parciales) |
| Grade CONSENSUS_HIGH | **0**; seis etiquetas previas degradadas al no demostrar independencia |
| Grade CONSENSUS_MEDIUM | **0**; una etiqueta previa degradada al no demostrar independencia |
| Grade SINGLE_SOURCE | **1.592** (una publicación numérica por sorteo) |
| Grade INSUFFICIENT | **896** (varias publicaciones numéricas; sin independencia verificable) |
| STRICT_DATASET (VALIDATED solamente) | **0** |
| HIGH_CONFIDENCE_RESEARCH_DATASET | **0**; los seis antiguos permanecen en cuarentena como INSUFFICIENT |
| Sorteos con dos o más observaciones | **904 / 2.488 (36,33 %)**; NO prueba independencia |
| Sorteos con alguna evidencia oficial, incluso parcial | **11 / 2.488 (0,44 %)** |
| Conflictos materiales de fecha o números observados | **0** |

## Ventana y continuidad

- **Ventana objetivo recuperada:** #799 (`2006-01-08`) a #3286 (`2026-09-30`), 21 años de calendario presentes (2006–2026); este rango supera la preferencia de aproximadamente 10 años de historia observable.
- **EXPECTED_DRAWS:** `UNKNOWN / null` respecto del cronograma oficial histórico externo: no se ha corroborado una enumeración oficial que permita derivar responsablemente el denominador de cobertura.
- **Cardinalidad del intervalo numérico observado:** 2.488 = #3286 − #799 + 1. Cero huecos internos en los *números* capturados; las fechas capturadas son únicas y crecientes. Esto NO certifica continuidad oficial ni implica que se haya adquirido todo el historial de Kino.
- **Período continuo observado más largo:** un tramo de 2.488 identificadores consecutivos (#799–#3286); la continuidad temporal oficial sigue **NO VERIFICADA**.
- **COVERAGE_PERCENT frente al calendario oficial:** `UNKNOWN`, no `100 %`. Los desgloses anuales y separaciones de días están en `reports/data-quality/kino/C39_5_COVERAGE_REPORT.json`.
- **Límites de régimen:** 799 filas raw anteriores a #799 contienen 15 números y permanecen excluidas sin conversión; #2893 marca un cambio institucional documentado de juegos adicionales, sin fabricar modificaciones al Kino base.

## Matriz, autoridad e independencia

La matriz reconstruida conserva 1.584 relaciones SINGLE_DATED_COMPLETE_SOURCE, 889 coincidencias solamente numéricas de gaaguile/Nicovh, 8 relaciones PARTIAL_OFFICIAL_CONTEXT (números de otra fuente) y 7 concordancias completas de fecha+números entre webs secundarias. Esas relaciones no son directamente equivalentes a los nuevos grados: **1.592 SINGLE_SOURCE y 896 INSUFFICIENT**, por conteo de publicaciones de números y evidencia positiva de procedencia. No hubo conflictos materiales entre las evidencias capturadas.

La tabla comunitaria gaaguile proviene de un XLSX cuyo editor original no está documentado; Nicovh obtiene información de ChileResultados y omite la fecha. Por tanto los 889 números coincidentes **no** son 889 corroboraciones editoriales independientes ni pueden convertirse en `CONSENSUS_HIGH` o `VALIDATED`. Ninguna interfaz oficial entregó, en la adquisición permitida, la serie completa de 14 números por sorteo requerida para elevar el estado global.

Los sorteos #3281–#3286 conservan los tres sitios publicadores con coincidencia completa de fecha+14 números (ResultadosKinoChile, ChileResultados y EpicentroChile); #3280 conserva dos. La independencia de su upstream editorial no fue demostrada. Los **siete** quedan `PROVISIONAL / INSUFFICIENT` y se conserva el recuento explícito de sitios y observaciones. Las 889 coincidencias numéricas gaaguile/Nicovh carecen de fecha en Nicovh y no acreditan independencia: Nicovh declara su origen ChileResultados y el XLSX de gaaguile tiene publicador desconocido.

## Cobertura por campos (denominador: 2.488 sorteos)

| Campo | Sorteos con datos adquiridos | Interpretación |
|---|---:|---|
| 14 números completos | 2.488 (100 % de los **observados**) | 0 oficialmente validados |
| Tabla de premios | 7 (0,28 %) | Capturas secundarias recientes |
| Conteos de ganadores | 7 (0,28 %) | Capturas secundarias recientes |
| Pozo total **estimado** | 2 (0,08 %) | Promoción/estimación, no pago real |
| Precio de boleto | 0 | Desconocido; no cero |
| Ventas / boletos vendidos | 0 | Desconocido; no cero |

No se calcularon patrones ganadores, inferencias estadísticas, modelos, simulaciones, backtesting ni análisis económico del producto.

## Brechas materiales y recuperación futura

El freeze clasifica **7 grupos de brecha**: corroboración numérica oficial, linaje de comunidad, fecha ausente en Nicovh, cobertura económica, seis fuentes profundas que requerirían automatización no autorizada, separación del legado de 15 números y falta de un censo oficial de sorteos/cronograma externo. Detalle: `reports/data-quality/kino/gap_report_frozen_v1.json` y `T38_ACQUISITION_AUTOMATION_GAPS.json`. Todo trabajo posterior queda sujeto a un **nuevo gate**; #40 no está autorizado en esta entrega y Master #26 permanece bloqueado.

## Artefactos verificables

- Raw y hashes Git: `reports/data-quality/kino/raw_inventory_v1.json`; se conserva contenido anterior en `data/raw/kino/**` sin alteración.
- Normalización: 6 particiones `data/processed/kino/observations_c39_*.json`; exclusión legado `legacy_regime_exclusion_c39_v1.json`.
- Matriz: 5 particiones activas `data/processed/kino/evidence_matrix_c39_*.json` descritas en `C39_3_EVIDENCE_MATRIX_QA.json`; la agregación grande `evidence_matrix_c39_2021_2026.json` queda SUPERSEDED por dos particiones más pequeñas.
- Consolidado y clasificación: 5 particiones `data/processed/kino/consolidated_c39_*.json`; QA en `C39_4_CLASSIFICATION_QA.json`.
- Separación física: `data/validated/kino/dataset_v1.json`, `data/quarantine/kino/dataset_v1.json` (manifiesto a cinco archivos), `data/rejected/kino/dataset_v1.json` y `high_confidence_research_dataset_c39_v1.json`.
- Auditorías de particiones y métricas: `C39_5_PARTITION_QA.json`, `C39_5_COVERAGE_REPORT.json`, `C39_5_ECONOMIC_FIELD_COVERAGE.json`.
- Catálogo de 26 fuentes: `sources/kino_historical_source_catalog_v2.json`; huellas Git por archivo: `reports/data-quality/kino/version_metadata_v1.json`.
- Diccionario: `docs/data/KINO_DATA_DICTIONARY_V1.md`.

## Hallazgos #41 y siguientes gates — **NO EJECUTADOS**

La investigación R41.1–R41.10 en [Issue #41](https://github.com/cmiloarevalo-hash/Estadis/issues/41#issuecomment-6071685134) terminó documentalmente con `VALIDATION_PATH: PARTIAL`. Calendario **observado**: sólo domingos hasta #816/2006-05-07, miércoles+domingo #817/2006-05-10 a #2260/2020-03-11, luego miércoles/viernes/domingo desde #2261/2020-03-13. La fuente arbitral NIC Chile reproduce una afirmación de Lotería sobre el cambio 2006, pero no certifica la fecha exacta del primer miércoles; anuncio contemporáneo de marzo 2020 respalda la incorporación del viernes. **2009-12-23** es fecha candidata de excepción `UNVERIFIED`, sin número de sorteo asignado. Estas referencias no certifican los 14 números ni la completitud oficial del calendario.

**Próximo trabajo sujeto a Work Items/Gates del Supervisor, pendiente:** (1) piloto de **diez consultas reales** por número y fecha en la interfaz oficial Kino, con selección estratificada por regímenes, resultados visibles/error/negativos y tiempo por consulta; R41.4 sólo hizo ocho *probes indexados*, **cero envíos funcionales de formularios**. (2) consulta formal a Lotería por el calendario 2006–2026, situación 2009-12-23, primera fecha de miércoles, extractos de premios/resultados con 14 números y condiciones de entrega/reproducción. Ni la consulta interactiva ni la solicitud externa están ejecutadas/autorizadas por el REWORK #39. No se presume capacidad de API ni automatización.

**Integridad:** se usan SHA-1 de blobs Git sobre archivos comprometidos; no se atribuye integridad HTTP original a capturas que son transcripciones manuales. Los controles documentales/estructurales C39.1–C39.5 se verificaron sin añadir pruebas ejecutables al repositorio.

**CI:** `NOT CONFIGURED` (sin workflow de CI comprobado). **Supervisor:** revisión pendiente del SHA final; no se emite aceptación ni se realiza merge.
