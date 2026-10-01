# Issue #6 — Metodología de estadística descriptiva para Kino

**Master Work Item:** #13  
**Checkpoint:** #6  
**Fecha:** 2026-10-01  
**Parent checkpoint:** #5 commit `3acbe43b7d53b63c39b2aa64d02f3b10c0fe80ee`  
**Modo:** investigación metodológica; sin cálculos masivos ni código.

## 1. Principio

Las estadísticas de este documento describen un futuro historial validado. No convierten frecuencia histórica en probabilidad futura.

Separación obligatoria:

```text
DESCRIPCIÓN DEL HISTORIAL
!=
PRUEBA DE ALEATORIEDAD
!=
PREDICCIÓN
```

NIST caracteriza EDA como un conjunto de técnicas para revelar estructura, detectar anomalías y examinar supuestos, no como un mecanismo automático de predicción.

Referencia:
https://www.nist.gov/publications/nistsematech-e-handbook-statistical-methods-chapter-1-exploratory-data-analysis

## 2. Unidad de análisis

Para cada sorteo validado (t=1,ldots,T), representar:

[
X_{t,j}=1
]

si el número (jin{1,ldots,25}) apareció en el sorteo, y 0 en caso contrario.

Cada fila tiene exactamente 14 unos.

Bajo el baseline teórico i.i.d. de Issue #5:

[
P(X_{t,j}=1)=rac{14}{25}=0.56.
]

## 3. Frecuencias absolutas y relativas

Para número (j):

[
F_j=sum_{t=1}^{T}X_{t,j}
]

y

[
hat p_j=F_j/T.
]

Esperanza bajo baseline:

[
E[F_j]=Trac{14}{25}.
]

Interpretación:
- (F_j) describe cuántas veces apareció;
- (hat p_j) describe proporción histórica observada;
- ninguna implica que (j) tenga mayor o menor probabilidad en el próximo sorteo.

## 4. Desviaciones e intervalos

Desviación simple:

[
D_j=F_j-T(14/25).
]

Bajo independencia entre sorteos, el conteo marginal de un número tiene baseline binomial:

[
F_jsim Binomial(T,14/25).
]

Puede usarse residuo estandarizado marginal:

[
Z_j=rac{F_j-Tp}{sqrt{Tp(1-p)}},quad p=14/25.
]

Advertencia:
- los 25 números dentro de un mismo sorteo no son independientes;
- por ello 25 z-scores marginales no constituyen 25 experimentos independientes.

Para intervalos de una proporción histórica se recomienda Wilson frente al Wald simple, siguiendo NIST:
https://www.itl.nist.gov/div898/handbook/prc/section2/prc241.htm

## 5. Ventanas móviles

Para una ventana de (W) sorteos:

[
F_{j,t}^{(W)}=sum_{s=t-W+1}^{t}X_{s,j}.
]

Reportar:
- W explícito;
- rango temporal;
- número de sorteos válidos;
- proporción (F/W);
- baseline esperado (W(14/25)).

No interpretar “subidas” o “bajadas” de ventanas solapadas como pruebas independientes. Las ventanas adyacentes comparten casi todos sus sorteos.

NIST recuerda que datos ordenados temporalmente pueden contener estructura interna y requieren tratamiento de series temporales:
https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm

## 6. Pares e impares

Entre 1 y 25:
- 12 números son pares;
- 13 son impares.

Sea (E_t) cantidad de pares en un sorteo.

Bajo baseline:

[
E_tsim Hypergeom(N=25,M=12,n=14).
]

Esperanza:

[
E[E_t]=14rac{12}{25}=6.72.
]

Describir:
- distribución observada de (E_t);
- media/mediana;
- frecuencias de 4,5,6,... pares;
- comparación visual con distribución teórica sólo como referencia.

No etiquetar una composición par/impar como “mejor” para apostar.

## 7. Suma de números

Para números sorteados (Y_{t,1},...,Y_{t,14}):

[
S_t=sum_iY_{t,i}.
]

La media poblacional de 1..25 es 13, por lo que:

[
E[S_t]=14	imes13=182.
]

Reportar:
- media, mediana, cuantiles;
- histograma/densidad empírica;
- mínimo/máximo observados;
- desviación respecto de 182.

La suma condensa información y no identifica combinaciones únicas.

## 8. Rango

Definir:

[
R_t=max(Y_t)-min(Y_t).
]

Reportar distribución, cuantiles y valores extremos. Un rango pequeño/grande puede ser raro sin ser predictivo.

## 9. Consecutivos

Ordenar ascendentemente los 14 números y definir número de adyacencias consecutivas:

[
C_t=sum_{i=1}^{13} I(Y_{t,i+1}=Y_{t,i}+1).
]

Distinguir:
- número de pares consecutivos;
- longitud de racha máxima consecutiva.

No contar dos veces sin declarar convención: 5-6-7 contiene dos adyacencias pero una racha de longitud 3.

## 10. Repeticiones entre sorteos

Para sorteos consecutivos (A_t,A_{t-1}):

[
Q_t=|A_tcap A_{t-1}|.
]

Si sorteos consecutivos fueran independientes y uniformes:

[
Q_t
]

tiene la misma distribución hipergeométrica de intersección de Issue #5, con:

[
E[Q_t]=14rac{14}{25}=7.84.
]

Esto es baseline descriptivo; la independencia temporal se evalúa en #7.

## 11. Intervalos entre apariciones

Para cada número (j), registrar secuencia de sorteos donde aparece. El intervalo puede definirse como:
- diferencia en índice de sorteo entre apariciones;
- número de sorteos completos sin aparición entre ambas.

Debe declararse cuál definición se usa.

Bajo un baseline i.i.d. marginal con (p=14/25), el tiempo de espera puede compararse conceptualmente con una geométrica. La comparación no convierte un intervalo largo en “número atrasado”.

## 12. Pares de números

Para par no ordenado ({a,b}):

[
F_{ab}=sum_t I(a,bin A_t).
]

Bajo un sorteo uniforme 14-de-25:

[
P(a,bin A_t)
=rac{14}{25}rac{13}{24}
=rac{91}{300}
approx0.3033333.
]

Hay:

[
inom{25}{2}=300
]

pares posibles.

## 13. Tríos

Para trío no ordenado ({a,b,c}):

[
P(a,b,cin A_t)
=rac{14}{25}rac{13}{24}rac{12}{23}
=rac{91}{575}
approx0.1582609.
]

Hay:

[
inom{25}{3}=2300
]

tríos posibles.

Buscar “pares calientes” o “tríos calientes” implica examinar cientos o miles de comparaciones. Valores extremos surgirán por azar aun bajo el null.

## 14. Comparaciones múltiples

Cuando una exploración genere hipótesis inferenciales:
- registrar número total de pruebas;
- distinguir análisis exploratorio de confirmatorio;
- aplicar control de error familiar (p.ej. Bonferroni/Holm) o FDR según objetivo;
- no seleccionar sólo los p-values menores sin contabilizar todas las pruebas.

Penn State resume que múltiples comparaciones sin ajuste inflan el error Tipo I y que Bonferroni/Tukey/Scheffé controlan el error familiar en contextos adecuados:
https://online.stat.psu.edu/stat502/Lesson02

Para pares/tríos de Kino, un análisis masivo debe planificarse específicamente en #7; no basta con ranking de frecuencias.

## 15. Catálogo mínimo de salida futura

Cada tabla/gráfico debe incluir:
- dataset_version;
- periodo de sorteos;
- T válido;
- régimen(es) de reglas incluidos;
- definición de métrica;
- baseline teórico cuando aplique;
- incertidumbre;
- cantidad de comparaciones;
- nota “descriptivo, no predictivo”.

## 16. Riesgos de mala interpretación

| Métrica | Riesgo |
|---|---|
| frecuencia | falacia de “número caliente/frío” |
| intervalo | falacia del jugador / “atrasado” |
| ventana móvil | cherry-picking del ancho/periodo |
| pares/tríos | multiplicidad extrema |
| suma/rango | confundir forma histórica con mayor probabilidad futura |
| consecutivos | considerar rareza visual como improbabilidad inválida |
| repeticiones | confundir dependencia aparente con evidencia sin test |

## 17. Fuentes

- NIST/SEMATECH e-Handbook — Exploratory Data Analysis  
  https://www.nist.gov/publications/nistsematech-e-handbook-statistical-methods-chapter-1-exploratory-data-analysis
- NIST — Confidence intervals for proportions  
  https://www.itl.nist.gov/div898/handbook/prc/section2/prc241.htm
- NIST — Introduction to Time Series Analysis  
  https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm
- Penn State STAT 502 — multiple comparisons  
  https://online.stat.psu.edu/stat502/Lesson02
- Issue #5 exact model, persisted in this branch.

## 18. Estado

```text
DESCRIPTIVE METRICS: SPECIFIED
INTERPRETATION RULES: SPECIFIED
HISTORICAL CALCULATION: NOT EXECUTED
PREDICTION: NOT CLAIMED
DEVELOPMENT_GATE: ACTIVE
```
