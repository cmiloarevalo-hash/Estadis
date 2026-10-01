# Issue #10 — Metodología de backtesting y readiness para desarrollo Kino

**Master Work Item:** #13  
**Checkpoint:** #10  
**Fecha:** 2026-10-01  
**Parent checkpoint:** #9 commit `eaf52626c2bb54afa5979f8a207f3ee4b4753d9c`  
**Modo:** metodología/readiness; no se ejecuta modelo ni backtest.

## 1. Propósito

Definir cómo evaluar en el futuro cualquier heurística que afirme usar historial Kino para seleccionar 14 números, sin leakage, sin selección retrospectiva y contra un baseline aleatorio fijado antes de observar el test.

La hipótesis de mejora debe ser tratada como una afirmación extraordinaria frente al modelo nulo uniforme/independiente de Issues #5, #7 y #8.

## 2. Baseline previo a cualquier heurística

Para cada sorteo futuro (D_t), una predicción válida (A_t) contiene exactamente 14 números distintos de 1 a 25.

Bajo H0:
- (D_t) es uniforme entre los (inom{25}{14}) subconjuntos;
- (D_t) es independiente de toda información anterior disponible al método.

Si (A_t) se construye **sólo con información previa a (t)**, entonces, condicionalmente al historial y a (A_t):

[
P(|A_tcap D_t|=k)=
rac{inom{14}{k}inom{11}{14-k}}{inom{25}{14}}.
]

Consecuencia: bajo H0, cualquier selector history-only que siempre entregue 14 números tiene la misma distribución exacta de aciertos que una combinación fija o aleatoria, salvo que explote una dependencia real del proceso.

Este resultado fija el baseline **antes** de probar heurísticas.

## 3. Orden temporal y split

No se permite shuffle aleatorio.

Diseño futuro mínimo:

```text
periodo total validado
├─ DEVELOPMENT/TRAIN
│  ├─ subtrain
│  └─ rolling/expanding validation
└─ FINAL TEST / LOCKBOX
```

El test final:
- es el bloque temporal más reciente reservado;
- no se usa para elegir variables;
- no se usa para elegir ventana;
- no se usa para elegir métrica;
- no se usa para descartar heurísticas;
- se abre una sola vez tras congelar la especificación.

Scikit-learn documenta que validación convencional puede entrenar con futuro y evaluar pasado; `TimeSeriesSplit` mantiene test posterior al train. Hyndman/FPP describe rolling forecasting origin: cada test usa únicamente observaciones anteriores.

Fuentes:
- https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html
- https://scikit-learn.org/stable/modules/cross_validation.html
- https://otexts.com/fpp3/tscv.html

## 4. Prevención de leakage

Leakage incluye cualquier información que no habría estado disponible antes del sorteo objetivo.

Prohibido:
- computar frecuencias usando el sorteo que se intenta predecir;
- normalizar/seleccionar features con todo el dataset;
- definir “números calientes” usando test;
- escoger longitud de ventana por desempeño del test;
- ajustar reglas después de ver resultados del lockbox;
- usar correcciones históricas publicadas después de la fecha objetivo sin versionado temporal;
- usar variables derivadas de premios/resultados del mismo sorteo.

Scikit-learn define leakage como uso en construcción del modelo de información no disponible al momento de predicción y recomienda separar train/test antes de preprocessing.

Fuente:
https://scikit-learn.org/stable/common_pitfalls.html

## 5. Rolling/expanding evaluation dentro de train

La selección de método debe usar sólo DEVELOPMENT/TRAIN.

En cada origen temporal (t):
1. construir features sólo con sorteos < t;
2. ajustar parámetros sólo con pasado;
3. emitir exactamente una predicción de 14 números para t;
4. registrar predicción antes de incorporar resultado t;
5. observar resultado t;
6. actualizar historial y avanzar.

Puede usarse:
- expanding window: todo el pasado;
- rolling window: ventana fija predefinida/tuneada sólo dentro de train.

El diseño concreto depende de cobertura y cambios de régimen.

## 6. Heurísticas y registro de búsqueda

Antes de test final, persistir catálogo completo:

| Campo | Requisito |
|---|---|
| heuristic_id | estable |
| descripción | completa |
| inputs | sólo pasado |
| hiperparámetros | espacio buscado |
| ventanas | espacio buscado |
| fecha de definición | antes del lockbox |
| folds de selección | sólo train |
| métricas | predefinidas |
| estado | retained/rejected antes del test |

No borrar candidatos fallidos.

## 7. Métricas para selector de 14 números

### Primaria

**Número de aciertos por sorteo**:

[
K_t=|A_tcap D_t|.
]

Reportar:
- media;
- mediana;
- distribución completa 3–14;
- diferencia versus expectativa (7.84);
- intervalo de diferencia.

### Categorías de premio

Secundaria:
- tasa de (Kge10);
- tasas 10,11,12,13,14 por separado;
- comparar con probabilidades exactas de #5.

Eventos muy raros no deben ser la única métrica: un test finito puede no contener ningún 13/14.

### Si el método produce probabilidades

Sólo si una futura heurística emite probabilidades calibradas:
- definir proper scoring rule antes del test;
- comparar contra (p_j=14/25);
- no añadir esta métrica después de ver el resultado.

## 8. Comparación contra azar

La comparación debe incluir:

### Baseline exacto
Para (K_t), la distribución exacta de #5.

### Baseline Monte Carlo
Para estadísticas no cerradas:
- usar diseño #8;
- replicar misma longitud temporal;
- misma métrica;
- semilla/algoritmo/versiones persistidos;
- MCSE reportado.

### Comparación emparejada
Cuando sea posible, evaluar diferencia por sorteo entre candidato y baseline generado/preespecificado, preservando dependencia temporal relevante.

## 9. Significancia y estabilidad

No basta una media mayor.

Exigir:
- tamaño de efecto;
- intervalo;
- p-value/calibración preespecificada si aplica;
- estabilidad entre folds temporales;
- sensibilidad a ventanas razonables **definidas en train**;
- ausencia de dependencia de un único periodo extraordinario.

Un resultado puede clasificarse:
- compatible con baseline;
- mejora aparente no robusta;
- mejora out-of-sample que requiere confirmación adicional;
- degradación.

No usar lenguaje predictivo fuerte tras un único test.

## 10. Data snooping y múltiples modelos

White (2000), *A Reality Check for Data Snooping*, advierte que reutilizar un mismo conjunto para inferencia/selección puede hacer que el mejor método observado parezca superior por azar.

Fuente:
https://onlinelibrary.wiley.com/doi/10.1111/1468-0262.00152

Reglas:
- registrar universo de heurísticas probado;
- no reportar sólo la ganadora;
- corregir por selección/multiplicidad;
- mantener lockbox final separado;
- si el lockbox se usa para una nueva decisión, deja de ser lockbox para esa decisión.

Para familias confirmatorias puede reutilizarse plan Holm de #7. Para búsqueda extensa, un Work Item de desarrollo deberá seleccionar formalmente método de ajuste/data-snooping antes de ejecutar.

## 11. Selección de modelos

Proceso:

```text
define baseline
→ freeze candidate family
→ temporal CV only inside train
→ select candidate/hyperparameters
→ freeze complete analysis plan
→ evaluate once on final lockbox
→ report every predeclared metric
```

Si se crea una nueva heurística después de ver el test:
- debe etiquetarse **post-hoc**;
- necesita un nuevo periodo futuro independiente para validación confirmatoria;
- no puede reciclar el mismo test como prueba independiente.

## 12. Resultados negativos

Resultado válido:

```text
NO MEJORA ESTADÍSTICAMENTE DEMOSTRABLE SOBRE AZAR
```

Debe persistirse:
- candidatos probados;
- resultados;
- incertidumbre;
- potencia/limitaciones;
- lockbox usado;
- decisiones posteriores.

No se permite ajustar retrospectivamente hasta obtener significancia y presentar el último intento como confirmatorio.

## 13. Cambios de régimen

Antes de backtesting:
- validar que la mecánica base 14/25 es comparable en el periodo;
- anotar cambios de producto/modalidades;
- impedir que features de una modalidad inexistente históricamente sean tratadas como disponibles;
- ejecutar análisis de sensibilidad por régimen si corresponde.

## 14. Prerregistro futuro mínimo

Antes de ejecución:
- dataset_version;
- cutoff train/test;
- candidate family;
- feature definitions;
- window search;
- tuning rule;
- metric primaria/secundarias;
- baseline;
- alpha/multiplicity;
- seed/RNG si hay simulación;
- stopping criteria;
- treatment of gaps;
- reporting template.

## 15. Fuentes metodológicas

- scikit-learn — TimeSeriesSplit  
  https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html
- scikit-learn — Cross-validation of time series data  
  https://scikit-learn.org/stable/modules/cross_validation.html
- scikit-learn — Common pitfalls: Data leakage  
  https://scikit-learn.org/stable/common_pitfalls.html
- Hyndman & Athanasopoulos — Forecasting: Principles and Practice, Time series cross-validation  
  https://otexts.com/fpp3/tscv.html
- White, H. (2000), A Reality Check for Data Snooping, Econometrica 68:1097–1126  
  DOI: 10.1111/1468-0262.00152
- Issue #7 multiplicity plan; Issue #8 Monte Carlo baseline.

## 16. Research readiness assessment

### Completado en #3–#10

- contrato reproducible de datos;
- provenance y versionado conceptual;
- inventario/cobertura parcial de fuentes históricas;
- plan de adquisición/validación;
- modelo matemático exacto;
- metodología descriptiva;
- plan de aleatoriedad/independencia;
- diseño Monte Carlo;
- diseño UX/tecnología;
- protocolo de backtesting/anti-leakage.

### Gaps materiales

1. no se verificó rango histórico completo/primer sorteo accesible;
2. no se verificó export/API oficial completo;
3. no se capturó muestra estática oficial de resultado con los 14 números dentro del batch;
4. no existe dataset raw/processed validado;
5. por diseño, no existen validadores ni pipeline;
6. no se ejecutaron estadísticas, tests, simulaciones ni backtest;
7. Streamlit es sólo candidato; tecnología no adoptada;
8. estrategia técnica de adquisición debe pasar por Work Item y review antes de automatizar.

## 17. Conclusión de readiness

```text
RESEARCH READINESS: RESEARCH_GAPS_REMAIN
WHY:
- methodology is substantially specified;
- data acquisition/coverage evidence is not yet sufficient to claim an analysis-ready dataset;
- no development has been authorized or executed.

READY FOR SUPERVISOR REVIEW OF RESEARCH: YES
READY TO CLAIM END-TO-END DEVELOPMENT READINESS: NO
DEVELOPMENT: NOT STARTED
DEVELOPMENT_GATE: BLOCKED_PENDING_HUMAN_SUPERVISOR
```

Los gaps no autorizan implementación automática. El Supervisor/Humano decide qué Work Item(s) de desarrollo o investigación adicional crear.
