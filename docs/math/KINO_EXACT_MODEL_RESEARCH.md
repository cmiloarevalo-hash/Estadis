# Issue #5 — Modelo matemático exacto del Kino base

**Master Work Item:** #13  
**Checkpoint:** #5  
**Fecha de investigación:** 2026-10-01  
**Parent checkpoint:** #4 commit `44eeaf73e11de2aca215a76c6669521f3621873d`  
**Modo:** derivación matemática documental; sin motor, notebook ni código.

## 1. Reglas oficiales usadas

La reglamentación de Lotería de Concepción establece para Kino base:
- universo: 25 números;
- el jugador pronostica 14;
- se extraen 14 bolillas sin reposición;
- el orden no importa para coincidencias.

Fuente:
https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47

## 2. Supuesto matemático explícito

Para el modelo teórico se asume que cada subconjunto de 14 números entre 25 es igualmente probable.

Este es el **modelo nulo ideal de sorteo aleatorio**. La reglamentación confirma la extracción sin reposición, pero la equiprobabilidad perfecta es un supuesto matemático que posteriormente puede contrastarse empíricamente; no se declara aquí como hecho demostrado sobre el mecanismo físico.

## 3. Espacio muestral

Como el orden no importa:

```text
|Omega| = C(25,14) = 25! / (14! * 11!) = 4,457,400.
```

Para un boleto fijo de 14 números, todos los resultados posibles del sorteo son los subconjuntos de 14 elementos del universo de 25.

## 4. Variable de aciertos

Sea (K) el número de números del boleto que aparecen en el sorteo.

Parámetros hipergeométricos:
- población (N=25);
- “éxitos” en la población (M=14): los 14 números del boleto;
- “fracasos” (N-M=11);
- muestra (n=14): números sorteados.

Entonces:

```text
P(K=k) = C(14,k) * C(11,14-k) / C(25,14).
```

Referencia matemática:
Penn State STAT 414, sección Hypergeometric Distribution:
https://online.stat.psu.edu/stat414/Lesson07

La distribución hipergeométrica es la apropiada para muestreo sin reposición.

## 5. Soporte de la distribución

No es posible tener menos de 3 aciertos.

La intersección mínima entre dos subconjuntos de tamaño 14 dentro de un universo de 25 es:

```text
14 + 14 - 25 = 3.
```

Por tanto:

```text
K ∈ {3, 4, ..., 14}.
```

## 6. Distribución exacta completa

Denominador común: C(25,14) = 4,457,400.

| k aciertos | Resultados favorables (C(14,k) * C(11,14-k)) | Probabilidad exacta | Probabilidad decimal | % |
|---:|---:|---:|---:|---:|
| 3 | 364 | 364 / 4,457,400 | 0.0000816619554 | 0.00816619554% |
| 4 | 11,011 | 11,011 / 4,457,400 | 0.002470274151 | 0.2470274151% |
| 5 | 110,110 | 110,110 / 4,457,400 | 0.02470274151 | 2.470274151% |
| 6 | 495,495 | 495,495 / 4,457,400 | 0.1111623368 | 11.11623368% |
| 7 | 1,132,560 | 1,132,560 / 4,457,400 | 0.2540853412 | 25.40853412% |
| 8 | 1,387,386 | 1,387,386 / 4,457,400 | 0.3112545430 | 31.12545430% |
| 9 | 924,924 | 924,924 / 4,457,400 | 0.2075030287 | 20.75030287% |
| 10 | 330,330 | 330,330 / 4,457,400 | 0.07410822453 | 7.410822453% |
| 11 | 60,060 | 60,060 / 4,457,400 | 0.01347422264 | 1.347422264% |
| 12 | 5,005 | 5,005 / 4,457,400 | 0.001122851887 | 0.1122851887% |
| 13 | 154 | 154 / 4,457,400 | 0.00003454928882 | 0.003454928882% |
| 14 | 1 | 1 / 4,457,400 | 0.0000002243460313 | 0.00002243460313% |

Suma de favorables:

```text
364+11011+110110+495495+1132560+1387386+924924+330330+60060+5005+154+1
=4,457,400.
```

Luego:

```text
sum[k=3..14] P(K=k) = 1
```

por construcción combinatoria.

## 7. Categorías numéricas base 10–14

La reglamentación define categorías de 10, 11, 12, 13 y 14 aciertos.

| Categoría | Probabilidad | Aproximación “1 en” |
|---|---:|---:|
| 10 | 0.07410822453 | 13.4938 |
| 11 | 0.01347422264 | 74.2158 |
| 12 | 0.001122851887 | 890.589 |
| 13 | 0.00003454928882 | 28,944.16 |
| 14 | 1 / 4,457,400 | 4,457,400 |

La probabilidad de caer en **alguna de las categorías numéricas 10–14** es:

```text
P(K >= 10) = 395,550 / 4,457,400 = 2637 / 29716 ≈ 0.08874007269.
```

Equivale a:
- 8.874007269%;
- aproximadamente 1 en 11.2689.

Esto se refiere sólo a las categorías numéricas por coincidencia 10–14. No incluye Club Kino, premios al RUT/cartón ni premios especiales/promocionales, que obedecen a otros mecanismos.

## 8. Premios versus probabilidades

Debe mantenerse la separación:

```text
PROBABILIDAD DE ACERTAR k NÚMEROS
!=
MONTO ESPERADO DEL PREMIO
```

Razones:
- 14 y 13 pueden depender de pozos variables y número de ganadores;
- 12, 11 y 10 tienen mínimos publicados, no necesariamente montos fijos universales para toda circunstancia;
- sorteos especiales pueden modificar precios/premios;
- impuestos/retenciones y reglas operativas no cambian la probabilidad combinatoria de aciertos, pero sí el valor monetario.

No se calcula retorno esperado en este checkpoint.

## 9. Independencia entre sorteos

El modelo exacto de una sola extracción no implica por sí mismo independencia temporal entre sorteos distintos.

Para cálculos teóricos posteriores puede definirse un modelo i.i.d. como baseline, pero su adecuación al historial debe tratarse separadamente en Issue #7.

## 10. Control exacto versus Monte Carlo

Las probabilidades anteriores tienen solución cerrada exacta. Por tanto:
- deben usarse como referencia de verdad matemática;
- una futura simulación Monte Carlo no debe sustituirlas;
- Monte Carlo puede validar empíricamente una implementación, no redefinir estos valores.

## 11. Fuentes matemáticas

1. Penn State, STAT 414, Hypergeometric Distribution  
   https://online.stat.psu.edu/stat414/Lesson07
2. Statistics LibreTexts, The Hypergeometric Distribution  
   https://stats.libretexts.org/Bookshelves/Probability_Theory/Probability_Mathematical_Statistics_and_Stochastic_Processes_%28Siegrist%29/12%253A_Finite_Sampling_Models/12.02%253A_The_Hypergeometric_Distribution
3. Lotería de Concepción, Productos y Reglamentación Kino  
   https://mc.loteria.cl/absolutenm/templates/PyR.aspx?articleid=868&zoneid=47

## 12. Estado

```text
EXACT SAMPLE SPACE: DERIVED
HYPERGEOMETRIC MODEL: SPECIFIED
PRIZE-CATEGORY PROBABILITIES: DERIVED
SOFTWARE ENGINE: NOT IMPLEMENTED
MONTE CARLO: NOT EXECUTED
DEVELOPMENT_GATE: ACTIVE
```
