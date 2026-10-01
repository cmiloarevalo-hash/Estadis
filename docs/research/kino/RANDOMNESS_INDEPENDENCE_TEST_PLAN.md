# Issue #7 — Plan de pruebas de aleatoriedad e independencia para Kino

**Master Work Item:** #13  
**Checkpoint:** #7  
**Fecha:** 2026-10-01  
**Parent checkpoint:** #6 commit `d2b87899543912727ed8a585932a67e4c6732d74`  
**Modo:** revisión/metodología; ninguna prueba ejecutada.

## 1. Objetivo inferencial

Evaluar en una fase futura si un historial **validado** es compatible con el modelo nulo de sorteos uniformes 14-de-25 e independientes en el tiempo.

No se intenta “demostrar aleatoriedad perfecta”.

Regla de reporting:

```text
NO RECHAZAR H0
!=
DEMOSTRAR QUE EL PROCESO ES PERFECTAMENTE ALEATORIO
```

NIST advierte que ausencia de autocorrelación significativa no implica aleatoriedad completa y que una batería de pruebas puede ser necesaria.

## 2. Modelo nulo principal

Por sorteo (t), sea (X_{t,j}in{0,1}) indicador de aparición del número (j).

H0 combina dos componentes:

### H0-A — uniformidad/exchangeability marginal

Para todos los números:

[
P(X_{t,j}=1)=14/25.
]

### H0-B — independencia temporal entre sorteos

Para sorteos distintos, los subconjuntos de 14 son independientes bajo el baseline.

H1:
- al menos una probabilidad marginal difiere;
- y/o existe estructura temporal incompatible con independencia.

Estas hipótesis son del modelo estadístico, no una afirmación sobre intención/fraude/mecanismo físico.

## 3. Dependencia dentro de cada sorteo

Los 14 números de un mismo sorteo se extraen sin reposición.

Por tanto, los indicadores (X_{t,j}) no son independientes dentro de una fila.

Para dos números distintos (a,b):

[
P(a,bin A_t)=91/300approx0.303333.
]

Con (p=14/25=0.56):

[
Cov(X_{t,a},X_{t,b})
=P(a,b)-p^2
approx-0.0102667.
]

Equivalente al resultado de muestreo aleatorio simple sin reposición:

[
Cov=-rac{p(1-p)}{25-1}.
]

Consecuencia: no es correcto asumir automáticamente que las (14T) apariciones agregadas son ensayos categóricos independientes.

## 4. Uniformidad marginal y χ²

Puede definirse un estadístico descriptivo de discrepancia:

[
Q=sum_{j=1}^{25}rac{(F_j-E)^2}{E},
quad E=T(14/25).
]

NIST describe el χ² de bondad de ajuste como comparación entre observados y esperados y advierte que necesita tamaño suficiente/esperados adecuados:
https://www.itl.nist.gov/div898/handbook/eda/section3/eda35f.htm

### Aplicabilidad a Kino

El estadístico es útil, pero usar sin más una referencia (chi^2_{24}) supone una estructura de conteos que no respeta la dependencia intradraw.

**Plan recomendado:**
1. conservar (Q) como estadístico interpretable;
2. bajo desarrollo futuro, obtener su distribución nula por un procedimiento que reproduzca exactamente sorteos uniformes 14-de-25 e independencia entre sorteos;
3. reportar p-value calibrado por ese null y, si se usa una aproximación asintótica, justificarla y contrastarla.

Esto evita un falso supuesto multinomial.

## 5. Residuos

Para cada número:

[
R_j=F_j-Tp
]

y residuo marginal estandarizado:

[
Z_j=rac{F_j-Tp}{sqrt{Tp(1-p)}}.
]

Usos:
- diagnóstico;
- identificar qué números contribuyen a una discrepancia global.

No usar 25 residuos como 25 “descubrimientos” sin corrección múltiple.

Reportar:
- valor;
- dirección;
- intervalo;
- adjusted p si se formaliza test individual.

## 6. Pruebas de rachas

Para cada número (j), la serie (X_{1,j},...,X_{T,j}) es binaria.

Hipótesis:
- H0: orden temporal compatible con una secuencia aleatoria bajo el null;
- H1: exceso/defecto de rachas.

NIST documenta el runs test como prueba de no aleatoriedad y define su estadístico a partir del número de rachas:
https://www.itl.nist.gov/div898/handbook/eda/section3/eda35d.htm

Adaptación:
- usar la versión apropiada para secuencia binaria;
- preferir distribución exacta/condicional si tamaño pequeño;
- aplicar corrección sobre los 25 números.

Limitación:
- una runs test detecta ciertos patrones de orden, no todas las formas de dependencia.

## 7. Autocorrelación

Para cada número, estudiar ACF de la serie binaria a lags preespecificados.

NIST define autocorrelación y la usa para detectar no aleatoriedad:
https://www.itl.nist.gov/div898/handbook/eda/section3/eda35c.htm

Plan:
- priorizar lag 1 como prueba primaria temporal;
- lags 2..L sólo si L se fija antes de mirar resultados;
- no usar decenas de lags y luego reportar sólo el más extremo.

NIST también recalca que autocorrelación nula no garantiza aleatoriedad.

## 8. Repetición entre sorteos

Definir:

[
Q_t=|A_tcap A_{t-1}|.
]

Bajo independencia, su distribución marginal es la hipergeométrica derivada en #5.

Posibles pruebas futuras:
- bondad de ajuste de la distribución de (Q_t);
- media observada versus 7.84;
- colas de repetición.

Cautela:
- (Q_t) y (Q_{t+1}) comparten el sorteo (A_t), por lo que la secuencia de overlaps no debe tratarse como independiente.
- una alternativa simple de diagnóstico es usar pares no solapados; otra es calibrar el estadístico bajo el proceso completo.

## 9. Intervalos entre apariciones

Para cada número, bajo baseline i.i.d. marginal, los tiempos de espera tienen referencia geométrica con (p=14/25).

Uso:
- QQ/ECDF contra referencia;
- colas de intervalos.

Cautela:
- 25 números × múltiples métricas implica multiplicidad;
- “intervalo largo” no significa que el número esté “debido”.

## 10. Pares y tríos

Pueden examinarse coocurrencias contra:
- par: (91/300);
- trío: (91/575).

Pero existen:
- 300 pares;
- 2300 tríos.

Por ello estos análisis son **secundarios/exploratorios** salvo hipótesis preespecificada.

## 11. Múltiples comparaciones

### Familia primaria

Propuesta:
- alpha familiar = 0.05;
- pruebas primarias preespecificadas;
- Holm para controlar FWER cuando haya varias hipótesis confirmatorias.

Referencia bibliográfica:
Sture Holm (1979), *A Simple Sequentially Rejective Multiple Test Procedure*, Scandinavian Journal of Statistics 6:65–70.

### Exploración masiva

Para pares/tríos y diagnósticos amplios:
- etiquetar como exploratorio;
- considerar Benjamini–Hochberg con q predefinido para controlar FDR;
- reportar todos los tests que forman la familia.

Referencia:
Benjamini & Hochberg (1995), DOI 10.1111/j.2517-6161.1995.tb02031.x
https://onlinelibrary.wiley.com/doi/10.1111/j.2517-6161.1995.tb02031.x

FWER y FDR responden preguntas distintas; no intercambiarlos sin declaración.

## 12. Nivel alpha, intervalos y tamaño muestral

### Alpha

Default de investigación para diseño:
- (alpha=0.05) para familia primaria;
- debe fijarse antes de mirar resultados.

### Intervalos

Reportar intervalos/effect sizes junto con p-values:
- diferencias de frecuencia;
- autocorrelaciones;
- probabilidades de overlap.

### Tamaño muestral

No fijar un número arbitrario antes de conocer cobertura.

Procedimiento futuro:
1. definir efecto mínimo relevante;
2. calcular potencia bajo alternativas explícitas;
3. determinar T requerido;
4. si el historial disponible es menor, reportar baja potencia.

No interpretar “p>0.05” como evidencia fuerte si la potencia es insuficiente.

## 13. Segmentación por régimen

Antes de cualquier test:
- revisar cambios de reglas;
- no mezclar periodos incompatibles sin justificación;
- ejecutar sensibilidad por régimen si un cambio puede afectar la variable observada.

El cambio 2893 afecta adicionales; la mecánica base 14/25 debe verificarse para periodos históricos antes de asumir homogeneidad completa.

## 14. Preregistro del plan de análisis

Antes de ejecutar:
- dataset_version;
- periodo;
- exclusiones;
- H0/H1;
- tests primarios;
- lags;
- alpha;
- corrección múltiple;
- estadísticos;
- criterio de potencia;
- tratamiento de missing/gaps.

Cambios posteriores deben marcarse exploratorios.

## 15. Reporte

Para cada prueba:
- hipótesis;
- estadístico;
- null/calibración;
- p-value;
- adjusted p;
- intervalo/effect size;
- tamaño de muestra;
- supuestos;
- sensibilidad;
- interpretación limitada.

Texto permitido:
> “No se detectó evidencia suficiente para rechazar este aspecto del modelo nulo al nivel preespecificado.”

Texto no permitido:
> “Se demostró que Kino es perfectamente aleatorio.”

## 16. Fuentes

- NIST — Chi-Square Goodness-of-Fit Test  
  https://www.itl.nist.gov/div898/handbook/eda/section3/eda35f.htm
- NIST — Runs Test for Detecting Non-randomness  
  https://www.itl.nist.gov/div898/handbook/eda/section3/eda35d.htm
- NIST — Autocorrelation  
  https://www.itl.nist.gov/div898/handbook/eda/section3/eda35c.htm
- NIST — Autocorrelation Plot  
  https://www.itl.nist.gov/div898/handbook/eda/section3/autocopl.htm
- Holm (1979), DOI 10.2307/4615733
- Benjamini & Hochberg (1995), DOI 10.1111/j.2517-6161.1995.tb02031.x
- Issues #5–#6 research artifacts.

## 17. Estado

```text
H0/H1: SPECIFIED
PRIMARY TEST FAMILIES: SPECIFIED
MULTIPLICITY PLAN: SPECIFIED
TEST EXECUTION: NOT STARTED
RANDOMNESS CLAIM: NONE
DEVELOPMENT_GATE: ACTIVE
```
