# Tareas: 002 Pagar el trámite

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Enumeración EstadoPago (pendiente, aprobado, rechazado)
  Archivos: `dominio/estado_pago.py`
  Escenarios: 1, 2, 3, 5
  Depende de: ninguna

- [ ] T-02 Actualizar entidad Pago con su estado y sus transiciones
  Archivos: `dominio/pago.py`
  Escenarios: 1, 2, 3, 5
  Depende de: T-01

- [ ] T-03 Ampliar puerto PasarelaPagos con crear_transaccion
  Archivos: `aplicacion/puertos/pasarela_pagos.py`
  Escenarios: 1, 4
  Depende de: T-02

- [ ] T-04 Ampliar puerto RepositorioCertificados con guardar y buscar pago por referencia
  Archivos: `aplicacion/puertos/repositorio_certificados.py`
  Escenarios: 1, 2, 3, 5
  Depende de: T-02

- [ ] T-05 DTO IniciarPagoEntrada
  Archivos: `aplicacion/dto/iniciar_pago_entrada.py`
  Escenarios: 1, 6
  Depende de: ninguna

- [ ] T-06 DTO IniciarPagoSalida
  Archivos: `aplicacion/dto/iniciar_pago_salida.py`
  Escenarios: 1
  Depende de: ninguna

- [ ] T-07 DTO NotificacionPagoEntrada
  Archivos: `aplicacion/dto/notificacion_pago_entrada.py`
  Escenarios: 2, 3, 5
  Depende de: ninguna

- [ ] T-08 Error PasarelaNoDisponible
  Archivos: `aplicacion/errores/pasarela_no_disponible.py`
  Escenarios: 4
  Depende de: ninguna

- [ ] T-09 Error PagoNoRequerido
  Archivos: `aplicacion/errores/pago_no_requerido.py`
  Escenarios: 6
  Depende de: ninguna

- [ ] T-10 Caso de uso IniciarPago
  Archivos: `aplicacion/iniciar_pago.py`
  Escenarios: 1, 4, 6
  Depende de: T-03, T-04, T-05, T-06, T-08, T-09

- [ ] T-11 Caso de uso ConfirmarPago
  Archivos: `aplicacion/confirmar_pago.py`
  Escenarios: 2, 3, 5
  Depende de: T-04, T-07

- [ ] T-12 Actualizar adaptador PasarelaPagosSimulada (crea transacción y simula caída)
  Archivos: `infraestructura/pasarela_pagos_simulada.py`
  Escenarios: 1, 4
  Depende de: T-03, T-08

- [ ] T-13 Actualizar adaptador RepositorioCertificadosMemoria con pagos
  Archivos: `infraestructura/repositorio_certificados_memoria.py`
  Escenarios: 1, 2, 3, 5
  Depende de: T-04

- [ ] T-14 Ampliar main.py con los seis escenarios de pago
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-10, T-11, T-12, T-13

- [ ] T-15 Validar los escenarios 1 a 6
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-14
