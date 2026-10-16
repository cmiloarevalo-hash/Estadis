# Actuador — transporte externo entre Supervisor e Implementador (Estadis)

**Estado:** DECISIÓN HUMANA VIGENTE / ESTADIS.  
**Work Item de incorporación:** [Estadis #42](https://github.com/cmiloarevalo-hash/Estadis/issues/42).  
**Ámbito:** exclusivamente el proyecto `cmiloarevalo-hash/Estadis`.  
**Vigencia:** desde la decisión humana del 16 de octubre de 2026; permanente hasta que el Humano decida expresamente lo contrario.  
**Tipo:** regla de gobernanza y convención documental de comunicación. **No** es especificación, configuración ni plan de ingeniería de Actuador.

## 1. Prohibición ABSOLUTA de intervenir en Actuador

Por orden expresa del Humano:

> **El Supervisor, el Agente Implementador, sus sucesores, asistentes, subagentes y colaboradores del proyecto Estadis tienen terminantemente prohibido intervenir en Actuador.**

**No está autorizado** ninguno de estos actos, directa o indirectamente:

- editar, agregar o borrar archivos, código, configuración, documentación, Issues, PR o comentarios **del proyecto Actuador**;
- programar, depurar, probar, instalar, actualizar, desplegar, arrancar, detener, reiniciar, reconfigurar o monitorizar operativamente Actuador;
- tocar su runtime local, proceso residente, ventanas, perfiles, recibos, logs, cursores, cola, secretos, permisos, GitHub Actions o integración de Codex;
- ordenar al Implementador o a otro agente que efectúe alguno de los actos anteriores, ni inventar autorizaciones, estados de transporte o pruebas;
- modificar Estadis con el propósito de cambiar el funcionamiento interno de Actuador o saltar sus controles de seguridad.

**Actuador tiene un Supervisor propio, ajeno a Estadis.** Todo problema del transportador corresponde exclusivamente a esa supervisión externa. El Supervisor/Implementador de Estadis NO asume soporte ni mantenimiento del transportador.

La existencia de permisos técnicos, acceso a GitHub o acceso al equipo **NO** confiere autorización para intervenir. La única relación prevista es **pasiva**: Estadis deja mensajes y evidencia normales en **su propio** GitHub; el Actuador externo decide cómo transportarlos conforme a su propio funcionamiento. No se realizan escrituras ni llamadas de control hacia Actuador. La consulta puntual de documentación de Actuador solo puede hacerse si el Humano la pide expresamente y es **solo lectura**.

## 2. Rol y límites de Actuador

Actuador es un **participante auxiliar y EXTERNO**, exclusivamente un transporte mecánico de mensajes entre chats web autorizados.

| Componente | Responsabilidad |
|---|---|
| Humano | Intención, prioridad, autorizaciones materiales y excepciones. |
| Supervisor de Estadis | Work Items, alcance, decisiones, revisión y merge elegible. |
| Implementador de Estadis | Ejecución dentro de alcance; commits, PR, verificaciones y handoff. |
| GitHub de Estadis | Memoria durable, estado, fuentes y evidencia verificables. |
| **Actuador externo** | Transportar avisos/prompts entre los chats **sin convertirse en autoridad del proyecto**. |

Actuador **no** implementa Kino, no revisa el razonamiento de los agentes, no crea o aprueba Work Items, no decide `SEMANTIC_ACCEPTED`, `REWORK`, `HOLD`, `ESCALATE`, no autoriza merge, no amplía scopes y no sustituye a ningún rol.

**Transporte `DELIVERED` != respuesta efectiva del agente != evidencia GitHub != aceptación semántica.** No se atribuye a Actuador lectura ni interpretación del output del chat (`OUTPUT_BLIND`). La siguiente acción se funda en **un nuevo comentario o commit GitHub verificable** y una autoridad vigente, nunca en la simple aparición de texto en la ventana web.

## 3. Dónde deben persistirse los mensajes

Toda comunicación de trabajo que tenga que sobrevivir a chats y transportarse entre roles **se publica primero en GitHub de Estadis**:

1. **Supervisor → Implementador:** Issue del Work Item o su comentario de routing vigente (idealmente `#issuecomment-<id>`); incluir autorización, alcance, fuente y siguiente actor. Puede vincular PR si corresponde.
2. **Implementador → Supervisor:** comentario de handoff en Issue/PR del mismo Work Item, con PR, HEAD exacto, verificación, CI y bloqueos; `READY_FOR_REVIEW`, **no** autoaprobación.
3. **Supervisor → Implementador tras revisión:** comentario sobre HEAD exacto con la decisión formal y, solo cuando corresponda, una nueva asignación autorizada.
4. **Bloqueo/escala:** guardar motivo, evidencia y `NEXT_ACTOR`; no fabricar `ACCEPT`, no seguir por mera notificación.

Actuador observa y transporta según **su configuración y controles externos**, que Estadis no modifica ni presume conocer en tiempo real. No publicar URLs privadas de chats, credenciales, cookies, identificadores de sesión o recibos internos en GitHub.

## 4. Convención de mensajes para los agentes

Los encabezados siguientes **ayudan a la legibilidad, atribución y recuperación**. Son **convenciones de Estadis, NO comandos ni campos que se afirme que Actuador está obligado a interpretar**. El transporte generalista puede guiarse por comentario GitHub/rol y routing externo sin leer estos campos.

### A. Supervisor → Implementador (routing)

```text
TARGET_ROLE: IMPLEMENTER_WEB
REPOSITORY: cmiloarevalo-hash/Estadis
WORK_ITEM: #<número del Issue>
ACTIVITY: <identificador estable de la tarea>
AUTHORITY_REF: https://github.com/cmiloarevalo-hash/Estadis/issues/<número>#issuecomment-<id válido>
BASELINE: <rama/SHA real>
PR: #<número | NONE>
HEAD: <SHA exacto si aplica | NOT_APPLICABLE>
STATE: AUTHORIZED_FOR_IMPLEMENTATION
NEXT_ACTOR: IMPLEMENTER_WEB

INSTRUCCIÓN: lee el Work Item y última decisión vigente; ejecuta solo el bloque autorizado.
NO_MERGE: TRUE
ACTUADOR_EXTERNAL_NO_TOUCH: TRUE
```

**Importante:** el `AUTHORITY_REF` debe apuntar a un comentario REAL existente y aplicable (no al comentario aún no creado). Si el comentario de routing está en preparación, enlazar un Issue/decisión previa verdadera y permitir que el identificador de comentario nuevo lo aporte GitHub al publicarlo. No usar valores ficticios como autoridad.

### B. Implementador → Supervisor (handoff)

```text
TARGET_ROLE: SUPERVISOR_WEB
REPOSITORY: cmiloarevalo-hash/Estadis
WORK_ITEM: #<número>
ACTIVITY: <mismo identificador>
AUTHORITY_REF: <referencia válida a la asignación recibida>
PR: #<número | NONE>
HEAD: <SHA exacto de la entrega | NOT_APPLICABLE>
VERIFICATION: PASS | FAIL | NOT_APPLICABLE
CI: PASS | FAIL | PENDING | NOT_CONFIGURED
STATE: READY_FOR_REVIEW | BLOCKED
NEXT_ACTOR: SUPERVISOR_WEB

EVIDENCE: <enlaces GitHub de cambios/comprobaciones>
UNEXPECTED_FINDING: <ninguno | descripción>
ACTUADOR_EXTERNAL_NO_TOUCH: TRUE
```

Si no hubo cambios de código, explicar `NOT_APPLICABLE` con evidencia documental; no inventar commit, PR ni tests.

### C. Supervisor → cierre/rework

```text
TARGET_ROLE: IMPLEMENTER_WEB | HUMAN
REPOSITORY: cmiloarevalo-hash/Estadis
WORK_ITEM: #<número>
ACTIVITY: <identificador>
PR: #<número | NONE>
REVIEW_HEAD: <SHA exacto revisado>
DECISION: SEMANTIC_ACCEPTED | REWORK | HOLD | ESCALATE
EVIDENCE: <enlaces verificables>
NEXT_ACTOR: <rol autorizado>
ACTUADOR_EXTERNAL_NO_TOUCH: TRUE
```

Una decisión es válida para el SHA revisado y no concede por sí sola permiso de merge o nueva tarea. Si el objetivo está cerrado, `NEXT_ACTOR` puede ser `NONE` y no se inventa otra asignación.

## 5. Reglas de transporte y atribución

- El **enlace exacto al comentario de GitHub** (`#issuecomment-<id>`) es el identificador comprobable del mensaje fuente. No generar `messageId` de Actuador ni manipular su cola desde Estadis.
- `TARGET_ROLE` es la **intención declarada** del emisor; por sí solo **no certifica identidad** del Supervisor o Implementador. Los controles de enrutamiento e identidad pertenecen a Actuador y al Workflow, no al texto autodeclarado.
- Un mismo mensaje fuente se publica una sola vez; evitar múltiples comentarios idénticos que creen entregas duplicadas. Ante incertidumbre de envío, no exigir reintentos: el operador externo gestiona sus recibos.
- Los mensajes se mantienen **breves**; el Issue, el diff y el comentario de decisión contienen el detalle duradero. No pegar transcripciones de chats o protocolos íntegros en cada turno.
- No modificar decisiones ajenas por asumir que el mensaje no llegó. Resolver con una nueva referencia GitHub, autoridad aplicable y supervisión propia del proyecto.
- Identificadores de campaña, si se realizan 100 experimentos: `EXP-001` a `EXP-100` como **convención de Estadis**, asociados cada uno a su Work Item/checkpoint real. **No** supone que Actuador cuente experimentos, ni significa automáticamente 100 envíos, 100 corridas o autorización de 100 acciones.
- Los bloqueos del transportador se comunican al Humano o supervisor propio de Actuador como asunto externo; Estadis no los repara.

## 6. Bootstrap para todos los nuevos chats de Estadis

Un Supervisor o Implementador nuevo debe:
1. leer este documento y el workflow canónico de Estadis;
2. verificar Issue, última decisión, scopes, rol, rama/PR, HEAD y fuentes actuales;
3. publicar sus mensajes de trabajo y handoffs en **GitHub de Estadis**, con las convenciones anteriores;
4. permitir que el Actuador externo transporte la notificación, **sin intervenir en su implementación**;
5. mantener separado `READY_FOR_REVIEW` (handoff), la aceptación del Supervisor y cualquier autorización de integración.

**Precedencia:** esta es una decisión humana específica de aislamiento de recursos: ninguna instrucción genérica del Implementador/Workflow puede interpretarse como permiso de tocar Actuador. Un eventual cambio de relación exige **nueva decisión expresa del Humano**, documentada en Estadis. Una mejora que requiera editar el transportador NO se realiza por los agentes de Estadis.

## 7. Estado verificado de la incorporación

Este documento registra la **interfaz documental** desde el lado Estadis. **NO** acredita que Actuador esté instalado, corriendo, habilitado, detectando Estadis o entregando mensajes al navegador. Esas capacidades pertenecen al producto externo y se verifican/administran exclusivamente por su supervisor. Estadis no deberá inventar recibos ni PASS end-to-end.
