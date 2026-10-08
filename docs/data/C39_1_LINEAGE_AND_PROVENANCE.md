# C39.1 — Procedencia y linaje

Master #15 · Issue #39 · 2026-10-08.

Se han inspeccionado 12 archivos raw en `data/raw/kino/**` sin modificarlos. Se conserva la identidad de cada archivo mediante su Git blob SHA; este identificador no acredita la integridad de la página HTTP originalmente consultada. Las capturas secundarias realizadas como transcripción estructurada no preservan necesariamente HTML original.

El dataset comunitario gaaguile contiene 3268 filas, incluidas 799 de un régimen histórico de 15 números que deben preservarse raw pero **no** convertirse a resultados Kino de 14 números. Las 2469 filas modernas de gaaguile corresponden al rango #799–#3267 sin faltantes internos. Nicovh contiene 889 filas sin fecha: sus 889 coincidencias numéricas con gaaguile no demuestran independencia editorial ni justifica `CONSENSUS_MEDIUM` por sí solas.

La procedencia primaria del XLSX usado por gaaguile no está demostrada. El catálogo identifica Fernando8955 como derivado de ChileResultados; no se cuenta como grupo independiente. Toda ausencia de prueba de independencia debe conservarse hasta la clasificación C39.4.

El inventario y las referencias anteriores a #39 se encuentran en `data/raw/kino/community/deep_expansion_manifest_t38.json`, `sources/kino_deep_source_evaluation_t38.json`, `sources/kino_historical_source_catalog_v1.json` y el árbol de Git del HEAD de #38 (`89989300c0e8764aea945c8cee83d6eefdcebce7`).

Checkpoint documental, sin desarrollo de producto, sin modificar raw, sin emitir aceptación semántica.
