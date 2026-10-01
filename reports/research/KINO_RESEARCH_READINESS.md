# Master #13 — Informe de readiness de investigación Kino

**Fecha:** 2026-10-01  
**Base:** `main@3e2d4d25aec6f34176b8b21c99cb8a8ebd83ef52`  
**Branch:** `research-kino-issues-3-10`  
**Scope:** investigación documental #3–#10.

## Resultado ejecutivo

Los ocho checkpoints de investigación especifican el sistema conceptual necesario para una fase posterior: contrato de datos, adquisición, matemática exacta, estadística descriptiva, pruebas de aleatoriedad, Monte Carlo, prototipo y backtesting.

No se desarrolló software.

Conclusión:

```text
RESEARCH CHECKPOINTS: COMPLETE
RESEARCH READINESS: RESEARCH_GAPS_REMAIN
DEVELOPMENT: NOT STARTED
DEVELOPMENT_GATE: BLOCKED_PENDING_HUMAN_SUPERVISOR
```

## Checkpoints

| Issue | Resultado | Artefacto principal |
|---|---|---|
| #3 | contrato de datos/provenance | `docs/data/KINO_DATA_CONTRACT_RESEARCH.md` |
| #4 | fuentes/cobertura/adquisición | `docs/research/kino/HISTORICAL_SOURCES_AND_ACQUISITION_RESEARCH.md` |
| #5 | modelo exacto | `docs/math/KINO_EXACT_MODEL_RESEARCH.md` |
| #6 | metodología descriptiva | `docs/research/kino/DESCRIPTIVE_STATISTICS_METHODOLOGY.md` |
| #7 | test plan aleatoriedad/independencia | `docs/research/kino/RANDOMNESS_INDEPENDENCE_TEST_PLAN.md` |
| #8 | diseño Monte Carlo | `docs/research/kino/MONTE_CARLO_BASELINE_DESIGN.md` |
| #9 | UX/opciones tecnológicas | `docs/research/kino/PROTOTYPE_UX_TECH_RESEARCH.md` |
| #10 | backtesting/readiness | `docs/research/kino/BACKTESTING_AND_READINESS.md` |

## Decisiones conceptuales consolidadas

1. **Datos:** raw inmutable → processed trazable → derived separado.
2. **Provenance:** source URL + retrieval + snapshot/hash + activity/agent conceptual.
3. **Reglas:** 14 de 25 sin reposición; régimen histórico explícito.
4. **Matemática:** exacta primero; Monte Carlo no sustituye combinatoria.
5. **Descripción:** no prediction-by-frequency.
6. **Inferencia:** dependencia intradraw y multiplicidad son requisitos de diseño.
7. **Simulación:** reproducibilidad incluye RNG algorithm/version + seed.
8. **UX:** separar matemática / historial / simulación.
9. **Tecnología:** Streamlit candidato, **no adoptado**.
10. **Backtest:** baseline previo + temporal CV + sealed lockbox + data-snooping control.

## Mapa de implementación futura — no autorizado todavía

Orden sugerido de dependencias:

```text
A. pilot official acquisition + source snapshots
↓
B. raw/processed contract materialization + validators
↓
C. validated/versioned historical dataset
↓
D. exact math service
↓
E. descriptive statistics service
↓
F. randomness/independence suite
↓
G. Monte Carlo baseline/calibration
↓
H. read-only statistical UI
↓
I. pre-registered backtesting experiment
```

Cada bloque requiere Authorized Scope y revisión propia. No debe construirse todo en un único Work Item.

## Gaps que deben resolverse

- cobertura histórica oficial exacta;
- mecanismo de adquisición autorizado;
- muestra/resultados raw y validación;
- tratamiento operativo de páginas oficiales cambiantes/contradictorias;
- decisión tecnológica;
- criterios de tamaño/potencia ligados al T real;
- política de despliegue si se desea app pública.

## Riesgos

- source drift y páginas oficiales sin versionado;
- errores manuales reconocidos por la propia fuente;
- gaps/duplicados;
- cherry-picking/multiplicidad;
- leakage temporal;
- data snooping;
- confusión entre descripción y predicción;
- sobreconfianza en Monte Carlo para probabilidades exactas;
- permisos técnicos de hosting/GitHub confundidos con autoridad.

## Criterio para un futuro claim predictivo

No se admite claim de mejora hasta cumplir, como mínimo:

```text
VALIDATED DATASET
+ PRE-REGISTERED CANDIDATE/Baseline
+ TEMPORAL OUT-OF-SAMPLE TEST
+ DATA-SNOOPING/MULTIPLICITY CONTROL
+ EFFECT SIZE + UNCERTAINTY
+ REPRODUCIBLE EVIDENCE
```

Un resultado negativo es una conclusión válida.
