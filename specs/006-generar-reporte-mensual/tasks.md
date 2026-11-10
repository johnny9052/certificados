# Tareas: 006 Generar reporte mensual

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Enumeración RolUsuario (estudiante, funcionario de registro)
  Archivos: `dominio/rol_usuario.py`
  Escenarios: 5
  Depende de: ninguna

- [ ] T-02 Ampliar puerto RepositorioCertificados con conteo de emitidas por tipo y de la cola de errores en un mes
  Archivos: `aplicacion/puertos/repositorio_certificados.py`
  Escenarios: 1, 2, 3
  Depende de: ninguna

- [ ] T-03 DTO GenerarReporteEntrada
  Archivos: `aplicacion/dto/generar_reporte_entrada.py`
  Escenarios: 1, 2, 4, 5
  Depende de: T-01

- [ ] T-04 DTO ReporteMensualSalida
  Archivos: `aplicacion/dto/reporte_mensual_salida.py`
  Escenarios: 1, 2, 3
  Depende de: ninguna

- [ ] T-05 Caso de uso GenerarReporteMensual
  Archivos: `aplicacion/generar_reporte_mensual.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-01, T-02, T-03, T-04

- [ ] T-06 Actualizar adaptador RepositorioCertificadosMemoria con los conteos
  Archivos: `infraestructura/repositorio_certificados_memoria.py`
  Escenarios: 1, 2, 3
  Depende de: T-02

- [ ] T-07 Ampliar main.py con los cinco escenarios de reporte
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-05, T-06

- [ ] T-08 Validar los escenarios 1 a 5
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-07
