# Workflow canónico de Estadis — Supervisor + GitHub + Agente implementador

> **Repositorio:** cmiloarevalo-hash/Estadis
>
> **Estado:** propuesta canónica de Issue #1; pendiente de revisión semántica del Supervisor para el SHA del PR.
>
> **Fuente de adaptación:** WORKFLOW_CANONICO_SUPERVISOR_GITHUB_IMPLEMENTADOR_AI_STUDIO.md en la base 8cf7d059692e6a7ab6eeec85bad5979e9a6ecde0, blob c347210353ce8dc8150343b886f59e5cd66645ed.
>
> **Bootstrap residente:** AGENTE_IMPLEMENTADOR.md, blob 1edb714a7cef4e110e84ce09a30ba375f84bc78e.
>
> **Work Item de adaptación:** Issue #1 y decisión RESUME del Supervisor en issuecomment-5935621278.
>
> **Regla de adaptación:** se conserva el significado operativo y la separación de autoridad del protocolo de referencia. Identificadores, rutas, comandos, integraciones, Issues, PR, SHA, documentación de producto e infraestructura pertenecientes al proyecto de procedencia no se importan como hechos de Estadis.

## 0. Estado inicial de capacidades en Estadis

En la base de Issue #1 el repositorio contiene únicamente el prompt residente del implementador y la fuente canónica importada para esta adaptación. Por tanto:

- documentación de producto/ingeniería adicional: **NOT CONFIGURED**;
- comandos de build/test del producto: **NOT CONFIGURED**;
- CI persistente: **NOT CONFIGURED**;
- contrato AI_STUDIO_OPERATOR: **NOT CONFIGURED**;
- integración operativa de Google AI Studio: **NOT CONFIGURED**;
- reglas específicas de deployment/publish: **NOT CONFIGURED**;
- allowlists de GitHub App para operadores externos: **NOT CONFIGURED**.

NOT CONFIGURED no elimina la regla correspondiente. Significa que no existe todavía infraestructura verificable que permita afirmar que está activa. Su incorporación futura requiere un Work Item autorizado.

---

# 1. Objetivo

Este workflow organiza el trabajo reproducible en cmiloarevalo-hash/Estadis entre:

- **Humano (Camilo):** intención del proyecto, prioridades, permisos y decisiones excepcionales.
- **Supervisor técnico (Chat Web GPT):** define y delimita Work Items, revisa evidencia, emite decisiones formales y ejecuta el merge cuando corresponda.
- **Agente implementador:** implementa únicamente el Work Item autorizado, publica branch/commit/PR y evidencia, y devuelve control al Supervisor.
- **GitHub:** memoria persistente, fuente de evidencia y coordinación.

El objetivo es que una sesión nueva pueda reconstruir el estado del trabajo desde GitHub sin depender del transcript de una conversación anterior.

Flujo principal:

~~~text
Humano
  ↓ intención/prioridades
Supervisor
  ↓ define Work Item
GitHub Issue
  ↓
Agente implementador
  ↓ implementa + verifica
Branch + Commit + PR
  ↓
Supervisor
  ↓ revisión del SHA exacto
SEMANTIC_ACCEPTED | REWORK | HOLD | ESCALATE
~~~

---

# 2. Principio fundamental

> **Las conversaciones son temporales. GitHub y los documentos persistidos del proyecto son la memoria compartida.**

El estado de una tarea debe poder reconstruirse desde:

~~~text
Repositorio
+ Issue
+ branch / PR
+ commits
+ documentación relevante
+ evidencia de verificación / CI
+ comentarios de review
~~~

Lo afirmado por un modelo es un reporte. El repositorio es la evidencia.

---

# 3. Autoridad y separación de roles

## 3.1 Humano

Conserva autoridad sobre:

- intención del proyecto;
- prioridades;
- decisiones de producto;
- cambios materiales de scope;
- credenciales y permisos;
- costes y trade-offs materiales;
- excepciones de workflow.

## 3.2 Supervisor técnico

Responsabilidades:

- convertir intención en Work Items verificables;
- definir Objective, Acceptance Criteria, Authorized Scope, Relevant Sources, Verification y Base;
- estudiar arquitectura y contexto sólo cuando sea necesario;
- revisar Issue, diff, PR, SHA, tests/evidencia y CI cuando exista;
- detectar scope creep, cambios accidentales y sobreingeniería;
- emitir una decisión formal;
- ejecutar el merge únicamente después de verificar elegibilidad.

El Supervisor no debe basar una aceptación sólo en el reporte del implementador.

## 3.3 Agente implementador

Responsabilidades:

- leer el Issue y la última decisión aplicable;
- recuperar sólo el contexto necesario;
- verificar base, branch y estado del repositorio;
- implementar únicamente dentro de Semantic Scope y Path Scope;
- ejecutar la verificación requerida;
- revisar el diff completo;
- crear commit;
- publicar branch;
- abrir o actualizar PR;
- informar el SHA exacto y evidencia.

No debe:

- redefinir arquitectura por iniciativa propia;
- ampliar scope silenciosamente;
- corregir problemas no relacionados;
- introducir infraestructura futura no solicitada;
- autoaprobarse;
- emitir SEMANTIC_ACCEPTED, REWORK, HOLD o ESCALATE;
- ejecutar merge.

---

# 4. GitHub como memoria persistente

GitHub persiste:

~~~text
Issue
→ intención, criterios, scope, fuentes, verificación y base

Branch
→ trabajo aislado

Commit
→ estado exacto de la propuesta

Pull Request
→ vehículo de integración

Diff
→ evidencia concreta del cambio

Tests / CI
→ evidencia mecánica cuando exista

Comentarios
→ decisiones, REWORK, bloqueos y handoffs

Git history
→ trazabilidad
~~~

Una instrucción material que afecte ejecución debe quedar persistida en GitHub cuando deba sobrevivir al cambio de sesión.

---

# 5. Unidad de trabajo: GitHub Issue

Cada tarea material debe existir como Work Item con, como mínimo:

~~~markdown
## Objective
Qué debe conseguir la tarea.

## Acceptance Criteria
Qué condiciones verificables deben cumplirse.

## Authorized Scope
Qué rutas o módulos puede modificar.

## Relevant Sources
Qué documentos o evidencia debe leer.

## Verification
Qué pruebas o comprobaciones debe ejecutar.

## Base
Branch o SHA desde el que debe comenzar.
~~~

Default recomendado:

~~~text
1 Issue
→ 1 objetivo
→ 1 branch
→ 1 PR
~~~

Puede existir una excepción, pero debe ser explícita y no puede inferirse por conveniencia del implementador.

---

# 6. Semantic Scope y Path Scope

## 6.1 Semantic Scope

Proviene de:

~~~text
Objective + Acceptance Criteria
~~~

Define qué comportamiento está autorizado a cambiar.

## 6.2 Path Scope

Proviene de:

~~~text
Authorized Scope
~~~

Define qué rutas están autorizadas.

Un cambio es válido sólo si satisface ambos scopes.

Que una ruta esté autorizada no concede permiso para cambiar cualquier comportamiento contenido en ella.

---

# 7. Bootstrap obligatorio del Agente implementador

Antes de modificar debe poder responder:

1. ¿Cuál es el objetivo del Issue?
2. ¿Cuáles son los Acceptance Criteria?
3. ¿Qué comportamiento está autorizado a cambiar?
4. ¿Qué rutas están autorizadas?
5. ¿Qué fuentes debo leer?
6. ¿Qué módulo/documento es responsable?
7. ¿Qué interfaces o contratos podría afectar?
8. ¿Qué verificación debo ejecutar?
9. ¿Cuál es la base exacta y cuál será la branch de trabajo?
10. ¿Existen cambios previos ajenos a esta tarea?

Si falta una respuesta material, el agente no improvisa. Persiste el hallazgo y devuelve control al Supervisor.

---

# 8. Política de lectura

El Agente implementador no debe leer automáticamente todo el repositorio.

Orden general:

~~~text
Issue y última decisión aplicable
↓
workflow canónico
↓
fuentes expresamente relevantes
↓
README si existe y es necesario
↓
documentación técnica pertinente si existe
↓
código/tests afectados
↓
contexto adicional sólo ante una dependencia real
~~~

Las rutas de documentación concretas se obtienen del estado real de Estadis y del Work Item. No se presuponen directorios heredados de otro proyecto.

---

# 9. Base, branch y aislamiento

La Base del Work Item es vinculante.

Reglas:

- crear la branch desde la base indicada por el Supervisor;
- si la base se expresa como SHA, usar ese SHA exacto;
- no asumir que el HEAD actual de main sustituye silenciosamente un SHA fijado;
- no mezclar cambios de otra tarea;
- si existen cambios previos ajenos, detenerse o aislarlos de forma verificable antes de continuar;
- el nombre de branch debe permitir asociarla al Work Item.

Un cambio de base material requiere decisión persistida del Supervisor.

---

# 10. Implementación

Durante la implementación:

- modificar únicamente lo necesario;
- preservar comportamiento no relacionado;
- evitar refactors oportunistas;
- no introducir frameworks, dependencias o servicios sin autorización;
- respetar contratos existentes;
- mantener el diff pequeño y revisable;
- no convertir un hallazgo secundario en cambio de alcance.

Para trabajo documental, las mismas reglas aplican: no se inventan capacidades, rutas, comandos ni estados operativos inexistentes.

---

# 11. Hallazgos durante el trabajo

Clasificación:

~~~text
Relacionado + dentro de scope
→ puede corregirse.

Relacionado + requiere ampliar scope
→ detenerse y solicitar decisión.

No relacionado
→ reportar sin modificar.
~~~

No existe autorización implícita del tipo “ya que estoy aquí también arreglé...”.

---

# 12. Verificación

Antes de publicar:

~~~text
implementar
↓
ejecutar la verificación requerida
↓
corregir si corresponde
↓
volver a verificar
↓
revisar diff completo
~~~

Debe comprobarse:

- archivos modificados;
- archivos nuevos;
- cambios accidentales;
- alcance semántico;
- alcance por rutas;
- contratos afectados;
- documentación necesaria;
- referencias heredadas inválidas;
- comandos/infraestructura declarados versus estado real;
- evidencia asociada al SHA que se entrega.

Si no existe suite de tests aplicable al Work Item, la verificación puede ser documental/estructural, pero debe quedar descrita de forma reproducible.

---

# 13. Publicación

Secuencia:

~~~text
commit
↓
push/publicación de branch
↓
Pull Request
~~~

El PR debe permitir identificar:

- Work Item;
- branch;
- base;
- HEAD SHA;
- cambios realizados;
- verificación ejecutada;
- estado de CI;
- limitaciones o hallazgos inesperados;
- fuentes/evidencia relevante.

---

# 14. Handoff del Agente implementador

Formato mínimo:

~~~text
WORK ITEM: #<issue>
PR: #<pr>
COMMIT: <sha>
VERIFICATION: PASS | FAIL
CI: PASS | FAIL | PENDING | NOT CONFIGURED
STATE: READY_FOR_REVIEW
UNEXPECTED FINDING: <none | descripción>
~~~

El Agente implementador no declara SEMANTIC_ACCEPTED.

---

# 15. Revisión del Supervisor

El Supervisor revisa de forma independiente:

- Issue;
- Objective;
- Acceptance Criteria;
- Semantic Scope;
- Path Scope;
- base;
- branch;
- HEAD SHA;
- diff completo;
- archivos cambiados;
- contratos/interfaces afectados;
- verificación;
- CI cuando exista;
- documentación;
- PR;
- comentarios/hallazgos.

La revisión produce exactamente uno de estos estados formales:

~~~text
SEMANTIC_ACCEPTED
REWORK
HOLD
ESCALATE
~~~

---

# 16. Significado de los estados formales

## SEMANTIC_ACCEPTED

El SHA revisado satisface la intención del Work Item.

La aceptación vale sólo para ese SHA.

No equivale automáticamente a merge ni autoriza al implementador a fusionar.

## REWORK

Existe una corrección necesaria dentro del mismo objetivo.

Default:

~~~text
mismo Issue
misma branch
mismo PR
nuevo commit
nueva verificación
nueva revisión
~~~

## HOLD

Existe un impedimento objetivo que impide continuar sin que corresponda todavía una decisión humana de producto/scope.

Ejemplos:

- dependencia externa temporalmente no disponible;
- evidencia técnica material inaccesible;
- conflicto operativo que impide verificar.

## ESCALATE

Se requiere decisión humana.

Ejemplos:

- cambio material de scope;
- decisión de producto;
- credencial o permiso;
- coste;
- excepción de seguridad o workflow;
- trade-off material.

---

# 17. Decisión vigente y revisión por SHA

Una decisión sólo vale para el SHA al que se asocia explícitamente.

Ejemplo:

~~~text
SHA A → REWORK
SHA B → SEMANTIC_ACCEPTED
SHA C → nuevo commit
~~~

La aceptación de B no alcanza a C.

Cuando existan múltiples comentarios, la decisión vigente es la última decisión del Supervisor que esté asociada explícitamente al HEAD actual.

---

# 18. REWORK

El comentario de REWORK debe dejar trazabilidad suficiente:

~~~text
Problema observado:
...

Resultado requerido:
...

Evidencia:
...

Scope:
permanece / cambia
~~~

Si el scope cambia, el Supervisor debe actualizar o aclarar el Work Item antes de que el implementador aplique el cambio.

---

# 19. CI

## Estado actual de Estadis

~~~text
CI: NOT CONFIGURED
~~~

En la base de Issue #1 no existe un workflow de CI persistente ni comandos canónicos de build/test que este documento pueda declarar como hechos.

## Regla canónica

Cuando exista CI:

- debe producir evidencia reproducible;
- la evidencia debe asociarse al SHA exacto revisado;
- PASS significa evidencia mecánica favorable, no aprobación semántica;
- FAIL o PENDING impiden afirmar que el CI requerido está satisfecho;
- cualquier configuración inicial o ampliación de CI requiere un Work Item autorizado;
- no se inventan comandos de build/test antes de que el repositorio los defina.

Estados permitidos:

~~~text
PASS
FAIL
PENDING
NOT CONFIGURED
~~~

---

# 20. Integración y merge

Secuencia conceptual:

~~~text
READY_FOR_REVIEW
↓
SEMANTIC_ACCEPTED para HEAD exacto
↓
verificación de condiciones de integración
↓
MERGE_ELIGIBLE
↓
merge por el Supervisor
~~~

Antes del merge, el Supervisor verifica:

- HEAD sigue siendo exactamente el SHA aceptado;
- base/target siguen vigentes;
- no existen conflictos relevantes;
- CI requerido, cuando exista, está válido para ese SHA;
- no apareció un blocker nuevo.

El Agente implementador no ejecuta merge.

---

# 21. Cambio de sesión del Agente implementador

Una sesión nueva reconstruye desde GitHub:

~~~text
Repositorio
↓
Issue
↓
última decisión aplicable
↓
base
↓
branch
↓
HEAD
↓
PR
↓
documentación relevante
↓
código/tests/evidencia afectados
~~~

Después repite las diez preguntas de bootstrap.

No depende del transcript previo.

---

# 22. Cambio de sesión del Supervisor

Una sesión nueva del Supervisor debe poder reconstruir:

1. producto/proyecto relevante;
2. Work Item activo;
3. objetivo y criterios;
4. scopes autorizados;
5. base;
6. HEAD actual;
7. cambios reales;
8. evidencia correspondiente al SHA;
9. última decisión válida para ese SHA;
10. decisión que corresponde tomar ahora.

---

# 23. Investigación externa: RESEARCH_GATE

Se activa antes de proponer o implementar una decisión técnica o estratégica material cuya validez dependa de información externa susceptible de cambio.

Ejemplos:

- versión o comportamiento actual de SDK/API/runtime;
- librerías/frameworks/herramientas;
- integración con proveedor;
- autenticación/seguridad dependiente de terceros;
- formatos/protocolos externos;
- límites, cuotas, precios o planes;
- estrategia de despliegue;
- alternativas cuya conveniencia dependa del ecosistema actual.

No se activa para una operación local/mecánica completamente determinada por el Issue y el repositorio.

Cuando se activa, el agente debe:

1. identificar el punto de decisión;
2. indicar qué dato externo actual necesita confirmar;
3. priorizar fuentes primarias/oficiales;
4. separar hechos del proyecto, hechos externos verificados, inferencias e incertidumbres;
5. persistir evidencia y propuesta en el Work Item;
6. devolver control al Supervisor antes de aplicar una estrategia no determinada por el Work Item.

La investigación produce evidencia y propuesta. No equivale a autorización.

---

# 24. STRATEGIC_RATIONALE

Para decisiones materiales sujetas a RESEARCH_GATE se persiste una justificación mínima:

~~~text
STRATEGIC_RATIONALE

WORK ITEM: #<issue>

DECISION POINT:
...

WHY CURRENT RESEARCH IS REQUIRED:
...

PROJECT CONSTRAINTS:
...

CURRENT EXTERNAL EVIDENCE:
- fuente primaria, fecha/versión, hecho relevante

OPTIONS CONSIDERED:
A. ...
B. ...

PROPOSED APPROACH:
...

RATIONALE:
...

RISKS / UNCERTAINTIES:
...

SUPERVISOR VERIFICATION:
PENDING | VERIFIED

RATIONALE STATUS:
DRAFT | VERIFIED | SUPERSEDED
~~~

Estos campos no crean nuevos estados formales de workflow.

---

# 25. TECHNICAL PERMISSION != WORKFLOW AUTHORITY

Regla canónica:

~~~text
TECHNICAL PERMISSION != WORKFLOW AUTHORITY
~~~

Que una cuenta, token, OAuth App, GitHub App, integración o herramienta tenga capacidad técnica para ejecutar una acción no significa que este workflow la autorice.

La autoridad proviene de:

~~~text
rol
+ Work Item
+ Semantic Scope
+ Path Scope
+ decisión vigente
~~~

Consecuencias:

- tener permiso de push no autoriza push fuera del rol/scope;
- tener permiso de merge no autoriza al implementador a hacer merge;
- tener capacidad de editar workflows no autoriza hacerlo sin Work Item;
- una allowlist escrita sólo en documentación no debe describirse como enforcement técnico si no existe control verificable;
- ninguna herramienta externa obtiene autoridad implícita por disponer de scopes amplios.

---

# 26. Operadores externos y AI Studio

## Estado actual de Estadis

~~~text
AI_STUDIO_OPERATOR: NOT CONFIGURED
AI_STUDIO_ALLOWED_REPOSITORY_WRITES: NONE
ALLOWED WRITE PATHS FOR AI STUDIO: NONE
~~~

No existe en la base de Issue #1 un contrato residente AI_STUDIO_OPERATOR ni configuración persistida que permita tratar Google AI Studio como operador activo de Estadis.

Por tanto, este workflow no presupone:

- checkout administrado de AI Studio;
- preview root;
- publicación;
- integración GitHub nativa;
- GitHub App;
- rutas de evidencia;
- deployment;
- comandos de runtime;
- permisos de repositorio para AI Studio.

## Regla preservada si se configura en el futuro

Cualquier incorporación futura de AI Studio u otro operador externo requiere Work Item separado y debe permanecer subordinada a este workflow.

Como mínimo:

- no sustituye al Agente implementador;
- no crea una ruta paralela de escritura al repositorio;
- no puede ampliar scope ni estados formales;
- no puede autoaprobar cambios;
- cualquier evidencia dependiente del código debe quedar asociada al SHA exacto;
- PASS/FAIL del operador es evidencia, no SEMANTIC_ACCEPTED/REWORK;
- repository/product-code write permanece prohibido salvo decisión humana explícita y actualización canónica previa;
- si se habilita una publicación externa, su protocolo, target, SHA gate, evidencia, seguridad y rollback deben definirse en el Work Item correspondiente.

Hasta entonces, cualquier operación AI Studio que pretenda actuar como parte del workflow debe considerarse no autorizada por falta de configuración canónica.

---

# 27. Seguridad de credenciales y permisos

Las credenciales no se persisten en Issues, PR, commits ni documentación.

Cuando una tarea requiera credenciales/permisos:

- el Work Item debe describir la necesidad sin exponer secretos;
- el agente no inventa ni reutiliza permisos por conveniencia;
- un permiso técnico existente no sustituye la autorización de workflow;
- si el requisito material no puede cumplirse sin una decisión humana, corresponde ESCALATE.

---

# 28. Evidencia y trazabilidad

La evidencia debe ser:

- mínima;
- verificable;
- asociada al Work Item;
- asociada al SHA cuando dependa del contenido del repositorio;
- suficientemente concreta para que otra sesión pueda reconstruir el resultado.

Ejemplos:

- diff del PR;
- commit SHA;
- salida de una verificación reproducible;
- estado CI para el SHA;
- referencia a documentación oficial cuando se activa RESEARCH_GATE;
- comentario del Supervisor asociado al HEAD.

No debe usarse como sustituto de evidencia:

- “funciona” sin comprobación;
- memoria del chat;
- una captura sin contexto cuando existe una fuente persistente mejor;
- un PASS referido a un SHA distinto.

---

# 29. Reglas esenciales

1. GitHub es la memoria compartida.
2. El Issue define la tarea.
3. Semantic Scope y Path Scope deben cumplirse simultáneamente.
4. El Agente implementador implementa; el Supervisor revisa.
5. El Agente implementador no se autoaprueba ni hace merge.
6. Branch/commit/PR preservan el estado exacto del trabajo.
7. Una decisión de review sólo vale para el SHA revisado.
8. Tests y CI son evidencia, no aprobación.
9. REWORK dentro del mismo objetivo permanece por defecto en el mismo Issue/branch/PR.
10. Una sesión nueva reconstruye desde GitHub, no desde el transcript anterior.
11. RESEARCH_GATE se usa cuando una decisión material depende de información externa mutable.
12. TECHNICAL PERMISSION != WORKFLOW AUTHORITY.
13. Capacidades inexistentes se marcan NOT CONFIGURED; no se inventan.
14. Issue posterior dependiente no comienza hasta que se cumpla su dependencia explícita.

---

# 30. Estado especial de Issue #1 y transición del bootstrap

Issue #1 instala/adapta este workflow. Hasta que el Supervisor emita SEMANTIC_ACCEPTED para el HEAD de su PR:

- AGENTE_IMPLEMENTADOR.md continúa siendo el bootstrap residente;
- este archivo es una propuesta para revisión, no una autoaceptación;
- no se inicia Issue #2 ni ninguna tarea que dependa de la aceptación de #1.

Después de que el Supervisor acepte semánticamente este documento para un SHA y lo integre en main:

- este archivo pasa a ser la referencia operativa principal del workflow de Estadis;
- AGENTE_IMPLEMENTADOR.md permanece como bootstrap del rol implementador;
- cualquier cambio posterior a este workflow requiere un Work Item autorizado y nueva revisión por SHA.

---

# 31. Adaptaciones realizadas respecto de la fuente de referencia

La fuente de referencia fue persistida específicamente para permitir esta adaptación. Su contenido operativo se trató así:

| Elemento de la fuente | Tratamiento en Estadis |
|---|---|
| Roles Humano / Supervisor / Implementador / GitHub | PRESERVED |
| GitHub como memoria persistente | PRESERVED |
| Issue como Work Item | PRESERVED |
| Semantic Scope + Path Scope | PRESERVED |
| branch / commit / PR | PRESERVED |
| bootstrap y cambio de sesión | PRESERVED |
| revisión y decisión por SHA | PRESERVED |
| SEMANTIC_ACCEPTED / REWORK / HOLD / ESCALATE | PRESERVED |
| CI como evidencia | PRESERVED; infraestructura concreta = NOT CONFIGURED |
| merge reservado al Supervisor | PRESERVED |
| RESEARCH_GATE + STRATEGIC_RATIONALE | PRESERVED |
| TECHNICAL PERMISSION != WORKFLOW AUTHORITY | PRESERVED |
| AI Studio como rol subordinado/no-write | PRESERVED como regla potencial; operador concreto = NOT CONFIGURED |
| nombres de repositorios ajenos | REMOVED como hechos operativos |
| Issues/PR/SHA históricos del proyecto de procedencia | REMOVED como autoridad de Estadis |
| rutas de documentación inexistentes | REMOVED como supuestos; se resuelven desde cada Work Item |
| comandos npm/build/test heredados | REMOVED; comandos de producto = NOT CONFIGURED |
| workflow GitHub Actions heredado | REMOVED como hecho; CI = NOT CONFIGURED |
| contrato AI_STUDIO_OPERATOR.md heredado | REMOVED como dependencia; contrato = NOT CONFIGURED |
| checkout/preview/deploy específicos de AI Studio | REMOVED como hechos; requieren Work Item futuro |
| restricciones de GitHub App dirigidas a otro repositorio | REMOVED |
| namespace de escritura/evidencia de otro proyecto | REMOVED |
| GoFlow y comandos auxiliares heredados | NO REQUIRED por este workflow |

No se importó ninguna capacidad sólo porque apareciera en la fuente de referencia.

---

# 32. Verificación mínima de este documento

Para Issue #1, la verificación requerida debe confirmar como mínimo:

1. el diff se limita al Authorized Scope;
2. no comienza análisis estadístico ni trabajo de producto;
3. no introduce comandos de build/test no existentes;
4. no declara CI como configurado;
5. no declara AI Studio como configurado;
6. no contiene nombres operativos de otros repositorios;
7. no usa Issues, PR o SHA históricos de otro proyecto como autoridad vigente;
8. conserva roles y separación Supervisor/Implementador;
9. conserva Semantic Scope + Path Scope;
10. conserva revisión por SHA;
11. conserva los cuatro estados formales;
12. conserva CI como evidencia, no aprobación;
13. conserva bootstrap/handoff/cambio de sesión;
14. conserva RESEARCH_GATE;
15. conserva TECHNICAL PERMISSION != WORKFLOW AUTHORITY;
16. el PR identifica el HEAD exacto que el Supervisor debe revisar.

---

# Resultado

Estadis queda gobernado por un protocolo que conserva el significado operacional del workflow de referencia sin importar como hechos infraestructura o decisiones históricas de otro proyecto.

La cadena canónica es:

~~~text
Humano
  ↓
Supervisor
  ↓
GitHub Issue
  ↓
Agente implementador
  ↓
Branch + Commit + PR + evidencia
  ↓
Supervisor revisa HEAD exacto
  ↓
SEMANTIC_ACCEPTED | REWORK | HOLD | ESCALATE
~~~

CI y operadores externos pueden incorporarse más adelante mediante Work Items separados. Mientras no exista evidencia persistida de su configuración, permanecen explícitamente NOT CONFIGURED.
