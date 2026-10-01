# Issue #9 — Diseño del prototipo de calculadora estadística Kino

**Master Work Item:** #13  
**Checkpoint:** #9  
**Fecha:** 2026-10-01  
**Parent checkpoint:** #8 commit `50c9cfa13595e92cb3a0ba9a495cb4a6def8284b`  
**Modo:** RESEARCH_GATE tecnológico; no se construye aplicación.

## 1. Objetivo del prototipo futuro

Un prototipo mínimo debe permitir consultar el proyecto Kino sin mezclar tres capas epistemológicamente distintas:

```text
MATEMÁTICA TEÓRICA
HISTORIAL VALIDADO
SIMULACIÓN / BASELINE
```

No debe:
- recomendar apuestas;
- calificar números como “más probables” por frecuencia histórica;
- ocultar gaps/provenance;
- presentar simulación como evidencia predictiva;
- ejecutar modelos no autorizados.

## 2. Usuarios y casos de uso

### Usuario A — investigador/auditor

Necesita:
- inspeccionar reglas y provenance;
- ver cobertura del dataset;
- reproducir métricas;
- distinguir hechos oficiales de resultados derivados.

### Usuario B — usuario estadístico general

Necesita:
- introducir una combinación de 14 números;
- consultar probabilidades teóricas exactas;
- ver contexto histórico descriptivo;
- comparar métricas con un baseline aleatorio;
- leer advertencias de interpretación.

### Usuario C — Supervisor técnico

Necesita:
- identificar dataset_version;
- ver SHA/versión de método;
- inspeccionar fuentes y limitaciones;
- comprobar que no hay claims predictivos sin out-of-sample evidence.

## 3. Una sola vista conceptual

### Encabezado fijo

Mostrar:
- “Kino Chile — calculadora estadística, no sistema de predicción”;
- reglas 14 de 25;
- dataset_version/periodo;
- estado de cobertura y gaps;
- enlaces a metodología/fuentes.

### Columna/sección 1 — Matemática teórica

Entradas:
- combinación opcional de 14 números.

Salidas:
- (C(25,14));
- distribución exacta de aciertos;
- probabilidades 10–14;
- explicación de montos variables versus probabilidad.

Fuente: Issue #5.

### Columna/sección 2 — Historial

Salidas:
- periodo disponible;
- frecuencia por número;
- suma/rango/pares;
- repeticiones;
- intervalos;
- provenance;
- disclaimers “descriptivo, no predictivo”.

Fuente: Issue #6.

### Columna/sección 3 — Simulación / baseline

Salidas futuras:
- distribución nula de métricas;
- percentiles/bandas;
- posición del histórico;
- seed/algoritmo/N;
- MCSE.

Fuente: Issue #8.

## 4. Interacciones mínimas

- selector de periodo/regla de régimen;
- entrada validada de exactamente 14 números distintos 1–25;
- selector de métrica;
- controles de simulación sólo después del DEVELOPMENT_GATE;
- botón/expander “Fuentes y metodología”;
- export de un reporte reproducible como mejora futura, no MVP obligatorio.

No incluir ranking de “números recomendados”.

## 5. Mensajes de cautela obligatorios

### General
> Resultados históricos no alteran la probabilidad teórica de una combinación bajo el modelo uniforme.

### Frecuencias
> “Frecuente” y “atrasado” describen el historial; no predicen el próximo sorteo.

### Tests
> No rechazar una hipótesis nula no demuestra aleatoriedad perfecta.

### Simulación
> Monte Carlo aproxima distribuciones complejas; los resultados exactos prevalecen cuando existe solución combinatoria cerrada.

### Datos
> Cobertura incompleta, gaps o fuentes contradictorias deben mostrarse explícitamente.

## 6. RESEARCH_GATE tecnológico

### Punto de decisión

¿Qué framework mínimo podría soportar una primera data app reproducible en Python, con tablas, controles y gráficos, sin construir un frontend separado?

### Opción A — Streamlit

Evidencia oficial actual:
- Streamlit se define como framework open-source Python para construir y desplegar data apps.
- Community Cloud despliega una app seleccionando repositorio, branch y entrypoint de GitHub.
- Documentación recomienda fijar versión de Streamlit para evitar upgrades inesperados.
- Despliegue adicional documentado para Docker/Kubernetes y otros proveedores.

Fuentes:
- https://docs.streamlit.io/
- https://docs.streamlit.io/deploy
- https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy
- https://docs.streamlit.io/deploy/streamlit-community-cloud/manage-your-app

Repositorio:
- https://github.com/streamlit/streamlit
- licencia: Apache-2.0
- lenguaje principal observado: Python
- activo al 2026-10-01.

Ventajas:
- alineado directamente con data apps;
- curva conceptual baja para una vista estadística;
- integra widgets, tablas y charts sin frontend separado;
- ruta simple desde GitHub a despliegue demostrativo.

Riesgos/limitaciones:
- modelo reactivo de rerun requiere disciplina para cálculos costosos;
- Community Cloud tiene límites que pueden cambiar;
- conectar GitHub implica permisos/OAuth que deben evaluarse en Work Item de despliegue;
- no adoptar Community Cloud automáticamente por existir.

### Opción B — Gradio

Evidencia oficial actual:
- Gradio se presenta como framework Python para interfaces de ML;
- permite interfaces y Blocks;
- share links temporales;
- hosting permanente documentado vía Hugging Face Spaces.

Fuentes:
- https://www.gradio.app/
- https://gradio.app/guides/sharing-your-app
- https://www.gradio.app/guides/quickstart

Repositorio:
- https://github.com/gradio-app/gradio
- licencia: Apache-2.0
- lenguaje principal observado: Python
- activo al 2026-10-01.

Ventajas:
- prototipado rápido;
- componentes ricos;
- deploy sencillo a Spaces;
- API/client ecosystem.

Riesgos/limitaciones:
- enfoque principal ML/demo es menos natural para una calculadora estadística orientada a tablas/EDA;
- share link no es despliegue permanente;
- Hugging Face añade otra plataforma/cuenta y su gobernanza.

### Opción C — Plotly Dash

Evidencia oficial actual:
- Dash se describe como framework para data apps/dashboard Python;
- su guía oficial de publicación contempla Plotly Cloud y Dash Enterprise.

Fuente:
- https://dash.plotly.com/deployment

Repositorio:
- https://github.com/plotly/dash
- licencia: MIT
- lenguaje principal observado: Python
- activo al 2026-10-01.

Ventajas:
- gran control de componentes/callbacks;
- buen encaje con dashboards;
- ruta a aplicaciones más estructuradas.

Riesgos/limitaciones:
- más superficie arquitectónica para un MVP;
- callbacks/layout requieren mayor disciplina que un script de data app;
- opciones empresariales pueden ser innecesarias para este proyecto inicial.

## 7. Matriz comparativa

| Criterio | Streamlit | Gradio | Dash |
|---|---|---|---|
| Encaje principal | data apps | ML demos/apps | data apps/dashboards |
| Python-only MVP | alto | alto | alto |
| Complejidad inicial | baja | baja | media |
| Tablas/EDA | muy natural | posible | natural |
| Controles estadísticos | natural | posible | natural |
| Deployment simple documentado | Community Cloud | HF Spaces | Plotly Cloud |
| Licencia repo | Apache-2.0 | Apache-2.0 | MIT |
| Plataforma adicional para hosting | Streamlit Cloud si se usa | Hugging Face si se usa | Plotly Cloud si se usa |
| Recomendación de investigación | **candidato preferido** | alternativa | alternativa si aumenta complejidad UI |

Esta matriz no constituye autorización de dependencia.

## 8. Propuesta técnica futura

### Propuesta

**Streamlit como candidato inicial**, porque:
- el producto es una data app estadística, no una interfaz de modelo ML;
- el MVP requiere una sola vista, controles, tablas y gráficos;
- minimiza frontend dedicado;
- permite mantener matemática/datos/UI separados en módulos Python futuros.

### Estado de decisión

```text
PROPOSED APPROACH: Streamlit
ADOPTED: NO
DEPENDENCY AUTHORIZED: NO
DEPLOYMENT AUTHORIZED: NO
SUPERVISOR/HUMAN REVIEW: REQUIRED
```

No se crea `requirements.txt`, `pyproject.toml`, `app.py` ni código.

## 9. Arquitectura mínima futura

Conceptualmente:

```text
validated data contract / dataset
          ↓
pure domain services
├─ exact math
├─ descriptive stats
├─ randomness evidence
└─ Monte Carlo baseline
          ↓
presentation adapter
          ↓
single-view UI
```

Reglas:
- UI no contiene fórmulas como única implementación;
- domain logic debe ser verificable fuera de UI;
- resultados identifican dataset_version y method_version;
- ingestión no ocurre dentro de la app;
- no hay scraping “on demand” al abrir la vista.

## 10. Reproducibilidad y mantenimiento

Futura implementación debe:
- fijar versiones de dependencias;
- registrar Python/framework version;
- separar datos de código;
- evitar llamadas web implícitas;
- definir límites de caché;
- mantener cálculos deterministas cuando no son simulación;
- para simulación, persistir RNG metadata;
- documentar despliegue y rollback.

## 11. Privacidad y seguridad

El MVP no necesita cuentas, PII ni credenciales para análisis local de datos públicos.

Si se evalúa un servicio de hosting:
- revisar permisos GitHub;
- no exponer secretos en repo;
- no conceder scopes por conveniencia;
- recordar `TECHNICAL PERMISSION != WORKFLOW AUTHORITY`.

## 12. Estado

```text
USERS/USE CASES: SPECIFIED
SINGLE-VIEW UX: SPECIFIED
TECHNOLOGY OPTIONS: RESEARCHED
PROPOSED CANDIDATE: STREAMLIT
TECHNOLOGY ADOPTION: PENDING HUMAN/SUPERVISOR
PROTOTYPE: NOT BUILT
DEPENDENCIES: NOT ADDED
DEPLOYMENT: NOT STARTED
DEVELOPMENT_GATE: ACTIVE
```
