# Kino Chile — reglas vigentes e inventario de fuentes oficiales

**Work Item:** Issue #2  
**Fecha de consulta:** 2026-10-01  
**Base de investigación:** `main@8e15f80b364b5b0d6fe5734e11393b6ae890d44e`  
**Estado:** evidencia de investigación para revisión del Supervisor.

## 1. Alcance y criterio de autoridad

Este documento registra únicamente hechos necesarios para fijar las reglas de Kino que condicionarán cálculos posteriores. Se priorizaron fuentes oficiales de Lotería de Concepción y, como marco normativo, LeyChile/BCN.

Regla de interpretación usada:

1. la página oficial **Productos y Reglamentación Kino** [S1] es la fuente operativa principal para la configuración actual del producto;
2. el Decreto Supremo N°659 y su sustitución por el DS N°1.114 [S5] explican el marco legal y la facultad de Lotería para fijar operatoria, categorías, precios y sorteos adicionales;
3. páginas oficiales actuales de juego, novedades y sorteos en vivo [S2, S3, S6] se usan como evidencia operacional complementaria;
4. una fuente oficial que contenga información desactualizada se conserva como evidencia de contradicción, no como autoridad actual [S4];
5. no se usan blogs ni fuentes secundarias para fijar reglas.

El inventario estructurado completo está en `sources/kino_official_sources.json`.

## 2. Kino base: reglas vigentes

Según [S1]:

- el apostador pronostica **14 números**;
- el universo es **1 a 25**;
- el sorteo principal extrae **14 bolillas de 25**;
- la extracción es **sin reposición**;
- para determinar coincidencias **el orden de extracción es irrelevante**;
- la apuesta base cuesta **$1.000 CLP**;
- el fondo asignado al pago de las categorías del Kino base corresponde al **47% del monto recaudado, excluidos impuestos**;
- el precio puede variar en Sorteos Especiales, por lo que $1.000 se registra como precio base ordinario publicado por la reglamentación consultada, no como precio inmutable para toda fecha especial.

Estas reglas son las que deben gobernar el futuro motor matemático del Kino base, salvo que un Work Item posterior documente una modificación oficial posterior a esta fecha de consulta.

## 3. Categorías base de 10 a 14 aciertos

| Categoría | Condición | Tratamiento del premio según [S1] |
|---|---|---|
| 14 aciertos | coinciden los 14 números pronosticados con los 14 extraídos | el monto de la categoría se reparte en partes iguales entre apuestas ganadoras; si no hay ganador, se acumula al sorteo siguiente |
| 13 aciertos | coinciden 13 de los 14 números pronosticados | el monto de la categoría se reparte en partes iguales entre apuestas ganadoras; si no hay ganador, se acumula al sorteo siguiente |
| 12 aciertos | coinciden 12 números | premio fijo de **al menos $10.000** por apuesta ganadora |
| 11 aciertos | coinciden 11 números | premio fijo de **al menos $2.000** por apuesta ganadora |
| 10 aciertos | coinciden 10 números | premio fijo de **al menos $1.000** por apuesta ganadora |

La fuente no publica en esta sección un porcentaje individual del 47% para cada categoría. Por ello no se inventa una distribución interna del fondo.

### Otras categorías incluidas en el precio base

[S1] también incluye en la apuesta base:

- **Club Kino**: participación gratuita condicionada al registro del RUT; sorteo semanal; premio variable con valorización mínima publicada de $10.000 por sorteo;
- **Premio Especial**: uno o más sorteos adicionales/promocionales; premio variable, con valorización mínima publicada de $10.000 por sorteo.

Estas categorías no deben confundirse con los juegos adicionales pagados descritos en la sección siguiente.

## 4. Juegos adicionales vigentes y separación respecto del Kino base

La reglamentación [S1] describe los siguientes juegos adicionales pagados. Todos usan los mismos 14 números del pronóstico principal, extraen 14 de 25 sin reposición y tienen una única categoría ganadora de 14 aciertos, salvo las particularidades del premio indicadas.

| Juego | Precio adicional | Precio total acumulado exigido | Premio/condición principal | Fondo |
|---|---:|---:|---|---|
| ReKino | $500 | $1.500 | 14 aciertos; pozo se divide entre ganadores y acumula si queda vacante | 47% de la recaudación del adicional, excluidos impuestos |
| RequeteKino | $500 | $2.000 | 14 aciertos; requiere Kino + ReKino | 47% de la recaudación del adicional, excluidos impuestos |
| Chao Jefe $2 Millones | $500 | $2.500 | 14 aciertos; $2.000.000 mensuales, reajustables y heredables, durante 50 años | 47% de la recaudación del adicional, excluidos impuestos |
| Chao Jefe $3 Millones | $500 | $3.000 | 14 aciertos; $3.000.000 mensuales, reajustables y heredables, durante 30 años | 47% de la recaudación del adicional, excluidos impuestos |
| Súper Combo Marraqueta | $500 | $3.500 | 14 aciertos; casa equivalente a $350.000.000, SUV $40.000.000, viaje $10.000.000 y $1.000.000 mensual reajustable/heredable por 50 años | 47% de la recaudación del adicional, excluidos impuestos |

[S1] denomina **Kino con Todo** a la combinación completa de Kino + ReKino + RequeteKino + Chao Jefe $2 Millones + Chao Jefe $3 Millones + Súper Combo Marraqueta, por $3.500, e incluye la categoría de premios al número de cartón.

Los precios del juego base y adicionales pueden variar por Sorteos Especiales. Los valores anteriores son los precios ordinarios publicados en [S1] a la fecha de consulta.

## 5. Cambio histórico desde el sorteo 2893

[S1] identifica explícitamente una reestructuración **a partir del sorteo N°2893, de 27-03-2024**:

**Eliminados:**
- Chanchito Regalón;
- Chao Jefe $1 Millón por 50 años;
- Combo Marraqueta.

**Creados:**
- RequeteKino;
- Súper Combo Marraqueta.

Este hito es material para cualquier dataset histórico: las modalidades no deben modelarse como si hubieran sido constantes a través de todo el historial.

## 6. Contradicción oficial detectada

La página oficial **Productos y Reglamentación Compras y Servicios Digitales** [S4] todavía enumera, en su sección sobre jugar Kino por internet, productos anteriores a la reestructuración de 2024, entre ellos Chanchito Regalón, Combo Marraqueta y Chao Jefe $1 Millón.

Esto contradice [S1], que:

- fija el cambio desde el sorteo 2893;
- declara eliminados esos tres productos;
- incorpora RequeteKino y Súper Combo Marraqueta;
- publica la estructura actual de precios.

**Tratamiento:** [S4] se clasifica como fuente oficial pero **desactualizada para la composición y precios actuales de Kino**. Puede usarse para documentar una contradicción y para aspectos generales de compras digitales, pero no para fijar la estructura vigente del producto cuando contradice [S1].

No se intenta reconciliar silenciosamente ambos textos.

## 7. Sorteos, horario y acceso público

El servicio oficial de Sorteos en Vivo [S3] publica actualmente para Kino:

- sorteos **miércoles, viernes y domingo**;
- horario de transmisión **22:30 hrs**;
- interfaz de “Sorteos Anteriores” con búsqueda por número de sorteo y rango de fechas;
- videos disponibles de los **últimos 60 días**.

[S1] indica además que los sorteos son de acceso público a través de Lotería y Sorteos en Vivo.

El URL incluido originalmente en Issue #2, `https://www.loteria.cl/sorteosenvivo` [S7], devolvió 404 en la consulta del 2026-10-01. El servicio operativo observado fue `https://www.sorteosenvivo.cl/sorteosenvivo` [S3].

## 8. Disponibilidad de resultados históricos y limitaciones

Se verificaron tres hechos oficiales relevantes:

1. [S3] ofrece una interfaz pública para buscar sorteos anteriores por número y fechas; los videos sólo cubren los últimos 60 días.
2. [S4] declara que Lotería pone estadísticas históricas a disposición del público y advierte que son recopiladas manualmente, por lo que no garantiza ausencia total de errores.
3. [S1] indica que los resultados oficiales son los entregados por Lotería a sus Agentes mediante los terminales instalados en Agencias Oficiales.

Por tanto, para el programa de investigación:

- **sí existe disponibilidad pública oficial de resultados/estadísticas históricas**, al menos mediante interfaces de consulta;
- **no se ha establecido todavía en Issue #2 un dataset oficial completo, descargable y machine-readable con rango histórico garantizado**;
- la consulta web no debe tratarse automáticamente como registro perfecto;
- Issue #4 deberá conservar raw, documentar rango obtenido, detectar gaps/duplicados y validar muestras contra fuente oficial antes de producir estadísticas.

No se recopila todavía el historial completo porque eso pertenece a Issue #4, no al scope de Issue #2.

## 9. Marco normativo

[S1] identifica a Kino como juego autorizado por el Decreto Supremo N°659, de 17-08-1990, cuyo texto fue sustituido por el Decreto Supremo N°1.114, publicado el 29-12-2005, en el marco de las leyes N°18.568 y N°18.768.

[S5] confirma que el DS N°659 regula el sorteo de números administrado por Lotería de Concepción y, en su texto sustituido, permite que Lotería determine la cantidad de números dentro de los rangos autorizados, categorías, modalidades y sorteos adicionales; el artículo 14 le entrega la facultad de precisar operatoria, territorialidad, frecuencia, precio y condiciones de participación.

Implicación: el decreto provee el marco legal, mientras la configuración operativa actual 14-de-25 y sus precios/categorías se toma de las instrucciones vigentes publicadas por Lotería [S1].

## 10. Estado de las fuentes oficiales del Work Item

| ID | Fuente | Estado observado 2026-10-01 | Uso |
|---|---|---|---|
| S1 | Productos y Reglamentación Kino | accesible/indexada; fuente operativa principal | reglas actuales, precios, categorías, reestructuración 2024 |
| S2 | Página de juego Kino | activa | confirmación operacional del producto; contenido textual recuperable limitado |
| S3 | Sorteos en Vivo | activa/indexada | horario y consulta de sorteos anteriores |
| S4 | Compras y Servicios Digitales | accesible/indexada, pero desactualizada en estructura Kino | contradicción histórica; advertencias sobre resultados/estadísticas |
| S5 | DS N°659 / LeyChile BCN | activa | marco legal |
| S6 | Novedades Lotería | activa; contiene publicaciones Kino de octubre 2026 | evidencia de comunicaciones vigentes y sorteos/promociones especiales |
| S7 | `loteria.cl/sorteosenvivo` | 404 observado | referencia del Issue reemplazada por S3 como endpoint operativo |

## 11. Hechos que quedan fuera de este Work Item

No se realiza en Issue #2:

- descarga exhaustiva del historial;
- normalización de sorteos;
- cálculo de probabilidades;
- estadística descriptiva;
- pruebas de aleatoriedad;
- simulación Monte Carlo;
- modelado predictivo;
- backtesting.

Esos trabajos pertenecen a Issues posteriores y siguen sujetos a sus gates.

## 12. Fuentes

- **[S1] Lotería de Concepción — Productos y Reglamentación Kino**  
  https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47  
  Consulta: 2026-10-01.

- **[S2] Lotería de Concepción — Kino**  
  https://www.loteria.cl/juegos/kino/  
  Consulta: 2026-10-01.

- **[S3] Lotería de Concepción — Sorteos en Vivo**  
  https://www.sorteosenvivo.cl/sorteosenvivo  
  Consulta: 2026-10-01.

- **[S4] Lotería de Concepción — Productos y Reglamentación Compras y Servicios Digitales**  
  https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=874&zoneid=47  
  Consulta: 2026-10-01.  
  Nota: desactualizada para composición/precios Kino frente a [S1].

- **[S5] Biblioteca del Congreso Nacional / LeyChile — Decreto Supremo N°659**  
  https://www.bcn.cl/leychile/navegar?idNorma=75710  
  Consulta: 2026-10-01.

- **[S6] Lotería de Concepción — Novedades**  
  https://www.loteria.cl/contenido/novedades/  
  Consulta: 2026-10-01.

- **[S7] URL de Sorteos en Vivo incluida en Issue #2**  
  https://www.loteria.cl/sorteosenvivo  
  Consulta: 2026-10-01.  
  Resultado observado: 404; no se usa como fuente operativa.
