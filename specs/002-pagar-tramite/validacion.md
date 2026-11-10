# Validación: 002 Pagar el trámite

Estado: **pendiente**. Este archivo se completa en la fase 5 (tarea T-15), cuando exista el código de `src/`. Aquí queda la estructura con el resultado esperado de cada escenario y la evidencia que debe aportarse. Un escenario sin evidencia no se da por cumplido.

## Validación funcional

| Escenario | Resultado esperado | Resultado | Evidencia requerida |
|-----------|--------------------|-----------|---------------------|
| 1. Pago iniciado | Se registra el pago pendiente y se devuelve la URL de pago | Pendiente | Salida de `main.py` con la referencia y la URL |
| 2. Pago aprobado | El pago queda aprobado y la notificación se responde como recibida | Pendiente | Salida de `main.py` con el estado antes y después |
| 3. Pago rechazado | El pago queda rechazado, no existe solicitud y se permite reintentar | Pendiente | Salida de `main.py` con el estado y un segundo pago iniciado |
| 4. La pasarela no responde | No queda pago pendiente sin URL y el mensaje informa la indisponibilidad | Pendiente | Salida de `main.py` con el mensaje y el repositorio sin pagos pendientes |
| 5. Notificación repetida | El estado no cambia y se responde como recibida | Pendiente | Salida de `main.py` con dos notificaciones y un solo cambio de estado |
| 6. Tipo sin costo | Se rechaza e indica que no requiere pago | Pendiente | Salida de `main.py` con el mensaje |

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
