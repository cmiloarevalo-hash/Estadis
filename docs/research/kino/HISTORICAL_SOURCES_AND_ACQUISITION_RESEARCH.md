# Issue #4 — Fuentes históricas, cobertura y estrategia futura de adquisición Kino

**Master Work Item:** #13  
**Checkpoint:** #4  
**Fecha de investigación:** 2026-10-01  
**Parent checkpoint:** #3 commit `50ac8e65d7a1aba11456a540e5fc1aee42bcabe9`  
**Modo:** investigación únicamente; sin scraper, ingestión masiva ni dataset completo.

## 1. Conclusión de cobertura

Se verificó que Lotería de Concepción ofrece **interfaces públicas consultables** para resultados/estadísticas, pero esta investigación no estableció un dataset oficial completo, machine-readable y con rango histórico garantizado.

Hechos verificables:

- Sorteos en Vivo expone una interfaz de “Sorteos Anteriores” con selección de juego, búsqueda por N° de sorteo y rango de fechas.
- La misma interfaz declara que los videos disponibles corresponden a los últimos 60 días.
- La página principal de Lotería expone enlaces a “Estadísticas” y “Sorteos en Vivo”.
- Los términos de servicios digitales indican que las estadísticas son información histórica recopilada manualmente y que Lotería no garantiza ausencia completa de errores.
- La reglamentación Kino declara que los resultados oficiales son los entregados por Lotería a sus Agentes mediante terminales.
- No se verificó una API pública documentada, export CSV/JSON completo ni política pública de rate limit para estas interfaces.

Por tanto:

```text
HISTORICAL RESULTS: PUBLICLY CONSULTABLE
COMPLETE OFFICIAL DATASET: NOT VERIFIED
EARLIEST AVAILABLE DRAW: NOT VERIFIED
PUBLIC MACHINE-READABLE EXPORT: NOT VERIFIED
PUBLIC API / RATE LIMIT POLICY: NOT VERIFIED
```

## 2. Inventario de fuentes históricas

### H1 — Sorteos en Vivo

URL:
- https://www.sorteosenvivo.cl/sorteosenvivo
- índice alternativo observado: https://loteria.cl/sorteosenvivo

Capacidades visibles:
- selector de juego;
- búsqueda por número;
- fecha desde / hasta;
- videos recientes;
- horario Kino miércoles/viernes/domingo 22:30.

Limitaciones:
- el texto indexado no muestra el rango histórico más antiguo;
- no se verificó export estructurado;
- videos limitados a 60 días no implican que los resultados tabulares tengan el mismo límite.

### H2 — Consulta de resultados

URL:
- https://www.loteria.cl/resultados/consulta-resultado/

Estado:
- interfaz oficial de consulta;
- el contenido recuperable mediante indexación web es limitado;
- no se documentó en este checkpoint un endpoint público estable de datos subyacentes.

### H3 — Estadísticas de Lotería

La navegación oficial publica la categoría “Estadísticas”. Los términos digitales añaden una advertencia material: son estadísticas históricas recopiladas manualmente y no se garantiza ausencia total de errores.

Fuente:
- https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=874&zoneid=47

Implicación:
- una futura adquisición no puede tratar la estadística publicada como ground truth infalible;
- se requiere validación de muestras y provenance.

### H4 — Reglamentación Kino

URL:
- https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47

Utilidad histórica:
- fija reglas actuales;
- registra el hito del sorteo 2893, 27-03-2024;
- permite segmentar cambios de régimen de juegos adicionales.

### H5 — Novedades

URL:
- https://www.loteria.cl/contenido/novedades/

Utilidad:
- documentar sorteos especiales, cambios y comunicaciones mensuales;
- no sustituye al registro principal de resultados.

## 3. Muestras manuales mínimas

No se efectuó descarga masiva.

### Muestra A — metadatos de sorteo futuro publicados oficialmente

Fuente: https://pendon-kino.loteria.cl/pendonkino

Observación indexada:
- próximo sorteo N°3285;
- fecha: 27-09-2026;
- monto total estimado publicado: $6.200 millones;
- desglose promocional visible.

Uso:
- valida que `draw_number`, `draw_date`, prize estimate y provenance son campos necesarios;
- **no** se usa como resultado final del sorteo;
- no contiene en la evidencia recuperada los 14 números ganadores.

### Muestra B — cambio de régimen

Fuente: reglamentación Kino H4.

Observación:
- sorteo N°2893;
- fecha 27-03-2024;
- hito explícito de reestructuración de juegos adicionales.

Uso:
- valida necesidad de `rule_regime_id`.

### Estado de muestra numérica

No se obtuvo de forma verificable en este checkpoint una lista oficial de 14 números de un sorteo mediante una página estática recuperable. No se sustituyó con datos de terceros ni se inventó una combinación.

```text
OFFICIAL NUMERIC DRAW SAMPLE: UNRESOLVED
ACTION: include in future authorized acquisition pilot
```

## 4. Gaps, duplicados y riesgos

### Gaps

Un hueco en secuencia de `draw_number` debe clasificarse primero como `candidate_gap`.

Posibles causas:
- dato ausente;
- cambio de numeración;
- sorteo reprogramado;
- interfaz que no expone todo el rango;
- fallo de captura.

No se debe fabricar un sorteo para restaurar continuidad.

### Duplicados

Dos observaciones con mismo número de sorteo pueden ser:
- duplicado real;
- mismo sorteo publicado en distintas fuentes;
- resultados de modalidades distintas;
- actualización/corrección posterior.

Se requiere source_id + modalidad + record_version.

### Riesgo de captura manual

La propia Lotería advierte que sus estadísticas históricas son recopiladas manualmente y pueden contener errores. Esto justifica:
- validación estructural;
- revisión de outliers;
- contraste de muestras;
- conservación de raw.

## 5. Formatos e interfaces

Formatos observados/documentados:
- HTML interactivo;
- contenido web indexable;
- video para sorteos recientes;
- páginas de novedades/reglamentación.

No verificado:
- CSV oficial completo;
- JSON oficial documentado;
- API pública documentada;
- descarga bulk oficial;
- esquema formal.

No se infiere un endpoint sólo por inspección indirecta.

## 6. Rate limits y restricciones

No se encontró una política pública específica de rate limits para las interfaces de resultados/estadísticas.

Regla futura:
- empezar con piloto manual;
- si se autoriza automatización, respetar términos, robots/políticas publicadas y carga conservadora;
- cachear/snapshotear en lugar de repetir consultas;
- detenerse ante bloqueo, autenticación o señales de restricción;
- documentar user agent, frecuencia y respuestas HTTP en el Work Item de desarrollo.

No se implementa nada de esto en #4.

## 7. Plan futuro de adquisición y validación

### Fase A — piloto manual

1. seleccionar una muestra pequeña de sorteos en varios periodos;
2. capturar número, fecha y 14 números desde interfaz oficial;
3. guardar snapshot/evidencia;
4. contrastar con otra publicación oficial cuando exista;
5. documentar cambio de formato/régimen.

### Fase B — evaluación técnica autorizada

Sólo con nuevo Work Item:
- determinar si existe endpoint oficial estable;
- evaluar términos y rate limits;
- definir estrategia de adquisición mínima;
- diseñar reintentos y auditoría;
- preservar raw antes de normalizar.

### Fase C — validación antes de dataset

Por registro:
- rango 1–25;
- 14 distintos;
- fecha;
- draw_number;
- modalidad;
- duplicados;
- candidate gaps;
- rule regime;
- source/provenance.

Por conjunto:
- cobertura inicial/final;
- conteo esperado vs observado sólo cuando el calendario esté confirmado;
- gaps resueltos/no resueltos;
- duplicados;
- cambios de esquema;
- hash/versionado.

## 8. Repositorios públicos comparativos

### alphatrl/sg-lottery-scraper

- URL: https://github.com/alphatrl/sg-lottery-scraper
- lenguaje principal: TypeScript
- licencia: MIT
- consulta: 2026-10-01
- problema: scraping de resultados de Singapore Pools.
- relevancia: evidencia de separación fuente oficial → adquisición → almacenamiento.
- limitación: jurisdicción, HTML y reglas distintas; no prueba estabilidad de endpoints Kino.

### szczyglis-dev/python-lottery-dataset-analyze

- URL: https://github.com/szczyglis-dev/python-lottery-dataset-analyze
- lenguaje: Jupyter Notebook/Python
- licencia: MIT
- consulta: 2026-10-01
- problema: descarga CSV históricos y análisis exploratorio.
- relevancia: ejemplo de declarar configuración de lotería (rango, count, formato de fecha).
- limitación: fuentes polacas y objetivos analíticos diferentes; no valida datasets de Kino.

### yongyct/singapore-pools-analysis

- URL: https://github.com/yongyct/singapore-pools-analysis
- lenguaje: Jupyter Notebook/Python
- licencia en GitHub: no declarada
- consulta: 2026-10-01
- problema: análisis de resultados Singapore Pools.
- relevancia: evidencia de workflow scraping+análisis.
- limitación: sin licencia explícita → no reutilizar código por defecto; además mezcla análisis predictivo que no es autoridad matemática.

## 9. Riesgos de provenance

- contenido web puede cambiar sin versionado visible;
- páginas oficiales pueden contradecirse;
- estadísticas oficiales reconocen posibilidad de error manual;
- páginas indexadas pueden estar cacheadas;
- sorteos especiales pueden alterar premios/modalidades;
- resultados y premios no deben extraerse de una sola captura sin fuente registrada.

## 10. Criterio de aceptación futura de cobertura

No declarar “dataset histórico completo” hasta registrar:
- primer y último sorteo verificables;
- mecanismo de enumeración;
- cantidad de registros;
- gaps explicados;
- duplicados tratados;
- muestra contrastada;
- snapshot/hash;
- reglas de régimen;
- provenance completo.

## 11. Estado

```text
SOURCE INVENTORY: RESEARCHED
COVERAGE: PARTIALLY CHARACTERIZED
FULL DATASET: NOT ACQUIRED
SCRAPER: NOT IMPLEMENTED
BULK INGESTION: NOT STARTED
DEVELOPMENT_GATE: ACTIVE
```
