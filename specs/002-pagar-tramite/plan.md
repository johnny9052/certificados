# Plan: 002 Pagar el trámite

Traza: CU-02, EAC-04, RT-01, ADR-002, ADR-004, ADR-005, ADR-006. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-002: módulo Pagos del monolito modular; los casos de uso `IniciarPago` y `ConfirmarPago` viven en `aplicacion/`.
- ADR-004: la identidad llega ya validada; solo el titular inicia el pago de su trámite.
- ADR-005: cada cambio de estado del pago se registra en la bitácora.
- ADR-006: las fallas de la pasarela se traducen a un error de aplicación; el caso de uso no conoce excepciones externas.

## Dependencias con otras specs

Requiere haber completado las tareas T-04 (entidad `Pago`), T-09 (puerto `PasarelaPagos`) y T-10 (puerto `Bitacora`) de la spec 001; esta spec las amplía.

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| dominio | `dominio/estado_pago.py` | Enumeración EstadoPago (pendiente, aprobado, rechazado) |
| dominio | `dominio/pago.py` | Actualizar entidad Pago con su estado y sus transiciones |
| aplicacion | `aplicacion/puertos/pasarela_pagos.py` | Ampliar puerto PasarelaPagos con crear_transaccion |
| aplicacion | `aplicacion/puertos/repositorio_certificados.py` | Ampliar puerto RepositorioCertificados con guardar y buscar pago por referencia |
| aplicacion | `aplicacion/dto/iniciar_pago_entrada.py` | DTO IniciarPagoEntrada |
| aplicacion | `aplicacion/dto/iniciar_pago_salida.py` | DTO IniciarPagoSalida |
| aplicacion | `aplicacion/dto/notificacion_pago_entrada.py` | DTO NotificacionPagoEntrada |
| aplicacion | `aplicacion/errores/pasarela_no_disponible.py` | Error PasarelaNoDisponible |
| aplicacion | `aplicacion/errores/pago_no_requerido.py` | Error PagoNoRequerido |
| aplicacion | `aplicacion/iniciar_pago.py` | Caso de uso IniciarPago |
| aplicacion | `aplicacion/confirmar_pago.py` | Caso de uso ConfirmarPago |
| infraestructura | `infraestructura/pasarela_pagos_simulada.py` | Actualizar adaptador PasarelaPagosSimulada (crea transacción y simula caída) |
| infraestructura | `infraestructura/repositorio_certificados_memoria.py` | Actualizar adaptador RepositorioCertificadosMemoria con pagos |
| raíz | `main.py` | Ampliar main.py con los seis escenarios de pago |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

[diagramas/secuencias/cu02-pagar-tramite.puml](../../docs/diagramas/secuencias/cu02-pagar-tramite.puml)

Pasos:

1. `IniciarPago`: verificar que el tipo requiere pago, registrar el pago pendiente, crear la transacción por `PasarelaPagos` y devolver la URL de pago; si la pasarela falla, descartar el pago pendiente y lanzar `PasarelaNoDisponible`.
2. `ConfirmarPago`: buscar el pago por su referencia; si ya tiene el estado notificado, no hacer nada; si no, marcarlo aprobado o rechazado y registrar en la bitácora.

## Fuera de alcance

- La creación de la solicitud (CU-01) y la firma (CU-03).
- La pasarela real, el controlador REST y la validación de la firma de la notificación entrante.
- Pruebas automatizadas.
