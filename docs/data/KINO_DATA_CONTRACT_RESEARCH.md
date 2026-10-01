# Issue #3 — Contrato reproducible y auditable de datos históricos Kino

**Master Work Item:** #13  
**Checkpoint:** #3  
**Fecha de investigación:** 2026-10-01  
**Base:** `main@3e2d4d25aec6f34176b8b21c99cb8a8ebd83ef52`  
**Modo:** investigación/especificación únicamente; sin validadores ni código.

## 1. Objetivo del contrato

Definir cómo representar sorteos Kino de manera que una futura adquisición pueda responder, para cada observación:

- qué sorteo es;
- qué fecha y modalidad representa;
- cuáles son los 14 números reportados;
- de qué fuente proviene;
- cuándo se consultó;
- qué transformación sufrió;
- bajo qué régimen de reglas fue interpretada;
- qué validaciones pasó o falló;
- qué premios fueron publicados, si existen.

Este contrato no afirma que exista ya un dataset completo. Define condiciones mínimas para que un dataset futuro pueda considerarse reproducible y auditable.

## 2. Fuentes que justifican el contrato

### Reglas oficiales de Kino

La reglamentación oficial de Lotería de Concepción establece que el Kino base extrae 14 bolillas de un universo de 25, sin reposición, y que el orden de extracción no se considera para determinar coincidencias. También registra una reestructuración de juegos adicionales desde el sorteo 2893 del 27-03-2024.

Fuente ya persistida por Issue #2:
- https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47

### Provenance

W3C PROV-DM define provenance como información sobre entidades, actividades y agentes involucrados en producir o entregar datos. Para Estadis se adopta como modelo conceptual, sin obligar todavía a una serialización PROV-O/PROV-N.

- https://www.w3.org/TR/2013/REC-prov-dm-20130430/
- https://www.w3.org/ns/prov

### Esquema tabular

Frictionless Table Schema/Data Package se usa como referencia de diseño porque permite declarar tipos, restricciones y metadatos para recursos tabulares. No se adopta todavía una dependencia de software.

- https://specs.frictionlessdata.io/tabular-data-resource/
- https://specs.frictionlessdata.io/tabular-data-package/
- https://framework.frictionlessdata.io/docs/guides/describing-data

## 3. Capas: raw / processed / derived

### 3.1 raw

Representa la evidencia tal como fue observada.

Debe preservar:
- fuente y URL exactas;
- fecha/hora de consulta;
- identificador visible del sorteo cuando exista;
- contenido o snapshot suficiente para auditoría;
- hash del snapshot cuando se materialice;
- método de captura;
- notas de acceso/error/ambigüedad.

Regla: **raw no se sobrescribe silenciosamente**.

### 3.2 processed

Representa una normalización reversible y trazable del raw.

Puede:
- convertir fecha a formato ISO;
- representar los 14 números como enteros;
- ordenar ascendentemente la lista para comparación canónica;
- mapear nombres de modalidad a vocabulario controlado;
- separar campos de premios.

No puede:
- completar números faltantes;
- corregir una fecha sin conservar la evidencia previa;
- reemplazar una contradicción por una decisión no documentada.

### 3.3 derived

Representa métricas o transformaciones analíticas calculadas posteriormente.

Ejemplos futuros:
- suma de números;
- cantidad de pares;
- frecuencia histórica;
- intervalos entre apariciones.

Los derivados deben apuntar a una versión concreta del dataset processed y nunca convertirse en fuente primaria.

## 4. Contrato lógico del registro de sorteo

Campos mínimos propuestos:

| Campo | Tipo conceptual | Obligatorio | Justificación |
|---|---|---:|---|
| `record_id` | string estable | sí | identidad interna versionable |
| `game` | enum | sí | separar Kino base de adicionales |
| `draw_number` | integer/string | sí cuando la fuente lo publique | identidad oficial visible |
| `draw_date` | date ISO YYYY-MM-DD | sí | orden temporal y auditoría |
| `numbers` | array<int> | sí para resultado confirmado | regla oficial: 14 de 25 |
| `numbers_order` | enum | sí | conservar si raw estaba en orden de extracción; processed puede usar `sorted_ascending` |
| `modality` | enum/string controlado | sí | `kino_base`, `rekino`, etc. |
| `rule_regime_id` | string | sí | cambios históricos de reglas/modalidades |
| `prizes` | objeto/tabla hija nullable | no | montos/categorías pueden no estar disponibles |
| `source_id` | string | sí | referencia al inventario de fuentes |
| `source_url` | URI | sí | provenance |
| `retrieved_at` | datetime con zona | sí | reproducibilidad |
| `source_snapshot_path` | path nullable | recomendado | evidencia raw persistida |
| `source_content_hash` | string nullable | recomendado | detectar cambios del snapshot |
| `capture_method` | enum | sí | manual, export oficial, futura adquisición automatizada autorizada |
| `schema_version` | string | sí | versionado del contrato |
| `record_version` | integer | sí | correcciones sin perder historial |
| `validation_status` | enum | sí | pending / valid / invalid / ambiguous |
| `validation_notes` | text/list | sí si no es valid | explicitar hallazgos |
| `supersedes_record_id` | string nullable | no | mantener correcciones auditables |

## 5. Entidades relacionadas

### 5.1 Source

Debe registrar publisher, title, URL, tipo, fecha de consulta, estado observado, limitaciones y licencia cuando aplique.

### 5.2 Rule regime

Debe contener:
- `rule_regime_id`;
- `effective_from_draw` y/o fecha;
- `effective_to_draw` si se conoce;
- universo numérico;
- cantidad extraída;
- modalidades vigentes;
- evidencia oficial;
- notas sobre cambios.

Ejemplo documental oficial verificable:
- `effective_from_draw = 2893`;
- `effective_from_date = 2024-03-27`;
- cambio: eliminación de Chanchito Regalón, Chao Jefe $1M/50 años y Combo Marraqueta; creación de RequeteKino y Súper Combo Marraqueta;
- fuente: reglamentación Kino de Lotería de Concepción.

Esto documenta el régimen del producto; no implica que la mecánica base 14/25 haya cambiado ese día.

### 5.3 Prize observation

Los premios se separan del núcleo del sorteo porque algunas categorías son variables, puede haber pozos acumulados y premios especiales/promocionales.

Campos mínimos:
- category;
- amount;
- currency;
- amount_type: fixed / minimum / estimated / final / in_kind;
- winner_count nullable;
- source_id;
- retrieved_at.

## 6. Reglas de validación documental

### 6.1 Números

Para `kino_base` cuando el resultado esté completo:
- exactamente 14 valores;
- todos enteros;
- rango inclusivo 1–25;
- todos distintos;
- la representación processed se normaliza ascendentemente;
- el raw conserva el orden observado si la fuente lo muestra.

Fallo de cualquiera de estas reglas:
- no se “repara” automáticamente;
- `validation_status = invalid` o `ambiguous`;
- se conserva evidencia para revisión.

### 6.2 Número de sorteo

- no asumir que toda secuencia debe ser continua;
- un salto se marca como **candidate_gap**;
- sólo se confirma como gap de datos después de contrastar fuente oficial;
- reprogramaciones/cancelaciones no deben transformarse en sorteos inventados.

### 6.3 Fecha

- normalizar a ISO YYYY-MM-DD;
- conservar texto raw si existiera ambigüedad;
- verificar consistencia con número de sorteo y fuente;
- si fuentes oficiales discrepan, registrar ambas observaciones.

### 6.4 Duplicados

Un duplicado candidato ocurre cuando coinciden game + draw_number + régimen compatible. Dos capturas de una misma observación desde fuentes distintas no se eliminan: se conservan como evidencia concordante.

### 6.5 Modalidades

Kino base y juegos adicionales son entidades de sorteo distintas aunque compartan el pronóstico del apostador. No se mezclan sus resultados en una sola lista sin campo de modalidad.

## 7. Provenance mínimo

Mapeo conceptual a W3C PROV:

- **Entity:** snapshot raw, registro processed, dataset versionado;
- **Activity:** captura, normalización, validación, derivación;
- **Agent:** Lotería de Concepción como publicador; proceso/humano autorizado que captura o transforma.

Relaciones mínimas:
- processed **wasDerivedFrom** raw;
- derived **wasDerivedFrom** processed;
- actividad **used** una entidad concreta;
- entidad **wasGeneratedBy** una actividad;
- actividad **wasAssociatedWith** un agente/proceso responsable.

No es necesario materializar RDF/PROV-O en esta fase.

## 8. Versionado

Se separan tres versiones:
1. `schema_version`: contrato;
2. `dataset_version`: publicación reproducible del conjunto processed;
3. `record_version`: observación individual.

Un registro corregido no borra el anterior: debe usar `supersedes_record_id` o mecanismo equivalente.

## 9. Ejemplos documentales basados en evidencia oficial

### Ejemplo A — régimen de reglas

```text
rule_regime_id: kino_product_from_draw_2893
effective_from_draw: 2893
effective_from_date: 2024-03-27
evidence_source: Lotería de Concepción, Productos y Reglamentación Kino
evidence_url: https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47
```

### Ejemplo B — observación de fuente

```text
source_id: loteria_kino_rules_868
publisher: Lotería de Concepción
source_type: official_primary
retrieved_on: 2026-10-01
supports: mecánica 14/25, sin reposición, orden irrelevante, reglas de premios y régimen desde sorteo 2893
```

No se inventan números de un sorteo para “rellenar” un ejemplo.

## 10. Estado

```text
DATA CONTRACT: SPECIFIED
DATASET: NOT BUILT
VALIDATORS: NOT IMPLEMENTED
DEVELOPMENT_GATE: ACTIVE
```
