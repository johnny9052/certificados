# Tareas: 008 Revisar solicitudes con novedades

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Actualizar entidad Solicitud con motivo de rechazo y rechazar
  Archivos: `dominio/solicitud.py`
  Escenarios: 4
  Depende de: ninguna

- [ ] T-02 Ampliar puerto ColaFirmas con listar la cola de errores y reprocesar
  Archivos: `aplicacion/puertos/cola_firmas.py`
  Escenarios: 1, 2, 3
  Depende de: ninguna

- [ ] T-03 DTO ListarNovedadesEntrada
  Archivos: `aplicacion/dto/listar_novedades_entrada.py`
  Escenarios: 1, 2, 6
  Depende de: ninguna

- [ ] T-04 DTO NovedadSalida
  Archivos: `aplicacion/dto/novedad_salida.py`
  Escenarios: 1, 2
  Depende de: ninguna

- [ ] T-05 DTO ResolverNovedadEntrada
  Archivos: `aplicacion/dto/resolver_novedad_entrada.py`
  Escenarios: 3, 4, 5, 6
  Depende de: ninguna

- [ ] T-06 Caso de uso ListarNovedades
  Archivos: `aplicacion/listar_novedades.py`
  Escenarios: 1, 2, 6
  Depende de: T-02, T-03, T-04

- [ ] T-07 Caso de uso ResolverNovedad (reprocesar o rechazar)
  Archivos: `aplicacion/resolver_novedad.py`
  Escenarios: 3, 4, 5, 6
  Depende de: T-01, T-02, T-05

- [ ] T-08 Actualizar adaptador ColaFirmasMemoria con lista y reproceso
  Archivos: `infraestructura/cola_firmas_memoria.py`
  Escenarios: 1, 2, 3
  Depende de: T-02

- [ ] T-09 Ampliar main.py con los seis escenarios de revisión
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-06, T-07, T-08

- [ ] T-10 Validar los escenarios 1 a 6
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-09
