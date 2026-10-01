# Issue #8 — Diseño Monte Carlo y baseline aleatorio Kino

**Master Work Item:** #13  
**Checkpoint:** #8  
**Fecha:** 2026-10-01  
**Parent checkpoint:** #7 commit `4476984a2f5ea7b4d18a51d55367ab570ba834a8`  
**Modo:** diseño de investigación; no se ejecuta simulación.

## 1. Objetivo

Definir una simulación futura que reproduzca el modelo nulo de Kino base y permita obtener distribuciones de referencia para estadísticas que no tengan una solución cerrada conveniente.

La simulación no sustituye la combinatoria exacta de Issue #5.

## 2. Generador conceptual

Una réplica de sorteo debe:

1. definir el universo ({1,ldots,25});
2. seleccionar uniformemente 14 elementos **sin reposición**;
3. conservarlos como conjunto para inferencia;
4. ordenarlos sólo para representación canónica.

No se permite:
- muestreo con reposición;
- duplicados;
- probabilidades no uniformes salvo que un experimento futuro las defina explícitamente como alternativa;
- “corregir” una muestra después de generarla.

La documentación oficial de NumPy para `Generator.choice` confirma que el patrón conceptual `replace=False` corresponde a muestreo sin reposición y que, sin vector `p`, el muestreo es uniforme. Esto es referencia de factibilidad futura, no adopción tecnológica en esta Issue.

## 3. Reproducibilidad

Una ejecución futura debe persistir:

- `rng_library`;
- `rng_library_version`;
- `bit_generator / algorithm`;
- `seed` o estado inicial reproducible;
- cantidad de réplicas;
- tamaño del historial simulado;
- parámetros del juego;
- dataset_version histórico usado para comparación;
- commit SHA del software que ejecute la simulación.

Una seed sin algoritmo/versión no es evidencia suficiente de reproducibilidad a largo plazo.

Si se paraleliza, los streams deben crearse mediante un mecanismo documentado para evitar solapamientos. NIST destaca la necesidad de replicaciones reproducibles y consistentes en análisis Monte Carlo distribuidos.

## 4. Dos niveles de simulación

### A. Un sorteo

Usado para validar distribuciones exactas:
- número de aciertos contra boleto fijo;
- pares/impares;
- suma;
- rango;
- consecutivos.

### B. Un historial completo

Una réplica debe generar exactamente (T) sorteos, donde (T) es el tamaño del periodo histórico válido que se desea comparar.

Se usa para:
- distribución de frecuencias por número;
- extremos entre 25 frecuencias;
- ventanas;
- autocorrelaciones;
- runs;
- pares/tríos;
- estadísticos globales como el (Q) de Issue #7.

Para calibrar un estadístico dependiente de la estructura temporal o de múltiples números debe simularse **el proceso completo**, no observaciones marginales aisladas.

## 5. Métricas a comparar contra histórico

Preespecificadas:

- frecuencias por número;
- máximo/mínimo de frecuencia entre 25 números;
- desviación global de uniformidad;
- composición par/impar;
- suma y rango;
- consecutivos;
- overlap entre sorteos consecutivos;
- intervalos entre apariciones;
- autocorrelación a lags predefinidos;
- número de hallazgos extremos tras corrección múltiple;
- frecuencias de pares/tríos, si se incluyen como exploratorias.

Cada comparación debe registrar si es:
- exact-check;
- primaria;
- exploratoria.

## 6. Controles exactos obligatorios

Antes de confiar en una simulación futura, sus salidas deben aproximar con error Monte Carlo compatible:

- (P(j	ext{ aparece})=14/25);
- (E[	ext{suma}]=182);
- (E[	ext{overlap consecutivo}]=7.84);
- distribución de aciertos (K) de Issue #5;
- (P(	ext{par específico})=91/300);
- (P(	ext{trío específico})=91/575);
- distribución par/impar hipergeométrica (N=25,M=12,n=14).

Un fallo en estos canaries invalida el uso inferencial del simulador.

## 7. Número de simulaciones

No se fija un número universal como “10.000” por costumbre.

Para una proporción Monte Carlo (hat p):

[
MCSE(hat p)approxsqrt{hat p(1-hat p)/N}.
]

El número de réplicas debe elegirse para que el error Monte Carlo sea pequeño frente a la precisión estadística requerida.

Procedimiento:
1. fijar tolerancia absoluta/relativa antes de ejecutar;
2. correr por lotes;
3. registrar estimación y MCSE;
4. aumentar (N) hasta cumplir tolerancia;
5. repetir controles exactos.

## 8. Convergencia

Reportar al menos:
- estimación por batch acumulado;
- MCSE;
- cuantiles de interés;
- estabilidad al duplicar (N).

No definir convergencia por “la gráfica parece estable”.

## 9. Bandas y percentiles

Para una métrica escalar del historial:
- distribución empírica nula sobre réplicas completas;
- percentiles preespecificados, por ejemplo 2.5% y 97.5%;
- posición del histórico dentro de esa distribución.

Si se construyen bandas simultáneas para muchos números/lags, deben calibrarse conjuntamente o tratar multiplicidad; no usar 25 intervalos puntuales como si fueran una banda familiar de 95%.

## 10. Monte Carlo p-values

Para una estadística donde “mayor = más extremo”, una futura estimación debe contabilizar réplicas tan o más extremas que el observado.

El procedimiento exacto de corrección finita se fijará en el Work Item de desarrollo y debe evitar reportar p=0 sólo porque ninguna de N réplicas excedió el observado.

## 11. Casos donde NO usar Monte Carlo

Usar solución exacta cuando existe y es manejable:

- (inom{25}{14});
- probabilidades de 3–14 aciertos;
- probabilidades 10–14;
- marginal (14/25);
- par/trío específico;
- distribución par/impar;
- overlap de dos sorteos independientes.

Monte Carlo se reserva para:
- distribuciones conjuntas complejas;
- extremos/máximos sobre muchas métricas;
- calibración de tests con dependencia intradraw;
- procedimientos de selección/multiplicidad;
- validación de implementación.

Para el jackpot 14/14, estimar (1/4{,}457{,}400) por simulación naïve sería especialmente ineficiente frente al resultado exacto.

## 12. Comparación histórico vs baseline

Requisitos previos:
- dataset validado;
- periodo y régimen declarados;
- gaps documentados;
- métricas preespecificadas.

Comparación:
1. calcular métrica histórica una vez;
2. generar réplicas nulas del mismo tamaño (T);
3. calcular exactamente la misma métrica en cada réplica;
4. reportar distribución nula, cuantiles, MCSE y posición del histórico;
5. no reinterpretar retrospectivamente la métrica después de ver resultados.

## 13. Referencias

- NIST — Preface: Uncertainty Evaluation by Monte Carlo Method  
  https://www.nist.gov/publications/preface-uncertainty-evaluation-monte-carlo-method
- NIST — Consistency in Monte Carlo Uncertainty Analyses  
  https://www.nist.gov/publications/consistency-monte-carlo-uncertainty-analyses
- NIST — Monte Carlo Tool  
  https://www.nist.gov/services-resources/software/monte-carlo-tool
- NumPy — random.Generator.choice  
  https://numpy.org/doc/stable/reference/random/generated/numpy.random.Generator.choice.html
- NumPy — new random Generator architecture  
  https://numpy.org/doc/stable/reference/random/new-or-different.html
- Issue #5 exact model and Issue #7 test plan.

## 14. Repositorios públicos comparativos

### yenseechen-data/lottery-predictability-monte-carlo
- URL: https://github.com/yenseechen-data/lottery-predictability-monte-carlo
- lenguaje: Jupyter Notebook
- consulta: 2026-10-01
- licencia GitHub: **no declarada**
- descripción: predictibilidad, walk-forward y Monte Carlo.
- utilidad: evidencia de que Monte Carlo puede combinarse con evaluación walk-forward.
- limitación: muy reciente, sin licencia y no es Kino; no reutilizar código ni adoptar conclusiones.

### forsing/Monte-Carlo-Lottery-739-535
- URL: https://github.com/forsing/Monte-Carlo-Lottery-739-535
- lenguaje: Python
- consulta: 2026-10-01
- estado: archivado
- licencia GitHub: **no declarada**
- utilidad: ejemplo nominal de simulación de lotería.
- limitación: sin licencia, archivado, reglas diferentes; sólo referencia comparativa.

## 15. Estado

```text
MONTE CARLO DESIGN: SPECIFIED
EXACT CONTROLS: SPECIFIED
REPRODUCIBILITY METADATA: SPECIFIED
SIMULATOR: NOT IMPLEMENTED
SIMULATION RUNS: 0
DEVELOPMENT_GATE: ACTIVE
```
