# Validación: 004 Consultar estado de la solicitud

Estado: **pendiente**. Este archivo se completa en la fase 5 (tarea T-08), cuando exista el código de `src/`. Aquí queda la estructura con el resultado esperado de cada escenario y la evidencia que debe aportarse. Un escenario sin evidencia no se da por cumplido.

## Validación funcional

| Escenario | Resultado esperado | Resultado | Evidencia requerida |
|-----------|--------------------|-----------|---------------------|
| 1. Consulta de una solicitud propia | Muestra estado, tipo, fecha y código de verificación | Pendiente | Salida de `main.py` con los datos mostrados |
| 2. Solicitud emitida | Muestra emitida e indica que puede descargarla | Pendiente | Salida de `main.py` con el mensaje |
| 3. Listado de solicitudes propias | Tres solicitudes, de la más reciente a la más antigua | Pendiente | Salida de `main.py` con el orden |
| 4. Sin solicitudes | Lista vacía sin error | Pendiente | Salida de `main.py` |
| 5. Solicitud de otra persona (EAC-04) | Se rechaza y queda registrado | Pendiente | Salida de `main.py` con la entrada de bitácora |

## Validación arquitectónica

| Verificación | Resultado | Evidencia requerida |
|--------------|-----------|---------------------|
| Regla de dependencia: ningún import de `infraestructura/` ni de `aplicacion/` dentro de `dominio/` | Pendiente | Búsqueda de imports en `src/dominio/` o grafo de imports |
| `aplicacion/` no importa `infraestructura/` | Pendiente | Búsqueda de imports en `src/aplicacion/` |
| Los casos de uso conocen solo puertos y errores de aplicación, no excepciones externas (ADR-006) | Pendiente | Revisión de las excepciones capturadas en los casos de uso de esta spec |
| Convenciones de `AGENTS.md`: una clase por archivo, sin `__init__.py`, DTO en entrada y salida, nombres según el glosario | Pendiente | Listado de archivos de `src/` |
| Cada tarea de `tasks.md` tiene su commit con el ID de la tarea | Pendiente | `git log` |

## Hallazgos y ajustes

Ninguno todavía. Si un escenario no se cumple: si el código no hace lo que dice la especificación, se corrige con una tarea nueva en `tasks.md`; si la especificación estaba incompleta o equivocada, primero se corrige `spec.md` (y el ADR, si cambia una decisión) y después el código.
