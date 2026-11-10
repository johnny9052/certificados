# Validación: 001 Solicitar certificado

Estado: **pendiente**. Este archivo se completa en la fase 5 (tarea T-22), cuando exista el código de `src/`. Aquí queda la estructura con el resultado esperado de cada escenario y la evidencia que debe aportarse. Un escenario sin evidencia no se da por cumplido.

## Validación funcional

| Escenario | Resultado esperado | Resultado | Evidencia requerida |
|-----------|--------------------|-----------|---------------------|
| 1. Solicitud aceptada | `SolicitarCertificado` crea la solicitud en estado registrada, con código de verificación, y la deja en la cola de firma | Pendiente | Salida de `main.py` con el código devuelto y el mensaje publicado |
| 2. Respuesta inmediata (EAC-01) | La respuesta no espera la firma; el caso de uso nunca llama al proveedor de firma | Pendiente | Revisión del código (no hay referencia al proveedor en `aplicacion/`) y tiempo medido en la ejecución simulada |
| 3. Estudiante sin paz y salvo | Se rechaza y el mensaje indica la dependencia pendiente; no se publica nada | Pendiente | Salida de `main.py` con el mensaje y cola de firma vacía |
| 4. Tipo con pago sin pago aprobado | Se rechaza e indica que debe pagar; no se publica nada | Pendiente | Salida de `main.py` con el mensaje y cola de firma vacía |
| 5. Solicitud a nombre de otra persona (EAC-04) | Se rechaza y queda un registro en la bitácora con usuario y hora | Pendiente | Salida de `main.py` con la entrada de bitácora |
| 6. El SIA no responde | No se acepta; se informa que la verificación no está disponible; no se publica nada | Pendiente | Salida de `main.py` con el mensaje y cola de firma vacía |

## Validación arquitectónica

| Verificación | Resultado | Evidencia requerida |
|--------------|-----------|---------------------|
| Regla de dependencia: ningún import de `infraestructura/` ni de `aplicacion/` dentro de `dominio/` | Pendiente | Búsqueda de imports en `src/dominio/` o grafo de imports |
| `aplicacion/` no importa `infraestructura/` | Pendiente | Búsqueda de imports en `src/aplicacion/` |
| ADR-001: la API solo encola; nunca llama al proveedor de firma | Pendiente | Ausencia de cualquier puerto o adaptador del proveedor en el caso de uso |
| ADR-006: el caso de uso no conoce excepciones externas | Pendiente | Revisión de las excepciones capturadas en `solicitar_certificado.py` |
| Convenciones de `AGENTS.md`: una clase por archivo, sin `__init__.py`, DTO en entrada y salida, nombres según el glosario | Pendiente | Listado de archivos de `src/` |
| Cada tarea de `tasks.md` tiene su commit con el ID de la tarea | Pendiente | `git log` |

## Hallazgos y ajustes

Ninguno todavía. Si un escenario no se cumple: si el código no hace lo que dice la especificación, se corrige con una tarea nueva en `tasks.md`; si la especificación estaba incompleta o equivocada, primero se corrige `spec.md` (y el ADR, si cambia una decisión) y después el código.
