# Validación: 003 Firmar digitalmente

Estado: **pendiente**. Este archivo se completa en la fase 5 (tarea T-17), cuando exista el código de `src/`. Aquí queda la estructura con el resultado esperado de cada escenario y la evidencia que debe aportarse. Un escenario sin evidencia no se da por cumplido.

## Validación funcional

| Escenario | Resultado esperado | Resultado | Evidencia requerida |
|-----------|--------------------|-----------|---------------------|
| 1. Firma exitosa | PDF guardado, intento exitoso, estado emitida y notificación enviada | Pendiente | Salida de `main.py` con el estado, la ruta del PDF y el correo simulado |
| 2. El proveedor no responde (EAC-02) | Intento fallido registrado, solicitud reencolada y no perdida | Pendiente | Salida de `main.py` con el intento fallido y el mensaje en la cola |
| 3. Espera creciente | Esperas de 2, 4, 8 y 16 s | Pendiente | Salida de `main.py` con las esperas calculadas por `PoliticaReintentos` |
| 4. Reintentos agotados (EAC-02) | Solicitud en la cola de errores tras 5 intentos, sin sexto intento | Pendiente | Salida de `main.py` con 5 intentos y la cola de errores |
| 5. Sin autorización de correo | Sin correo; certificado emitido | Pendiente | Salida de `main.py` sin notificación y con estado emitida |
| 6. Falla el envío del correo | Certificado emitido y falla registrada en la bitácora | Pendiente | Salida de `main.py` con el estado y la entrada de bitácora |
| 7. Mensaje duplicado | Se descarta sin llamar al proveedor | Pendiente | Salida de `main.py` con cero llamadas al proveedor simulado |

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
