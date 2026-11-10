# Tareas: 004 Consultar estado de la solicitud

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Ampliar puerto RepositorioCertificados con buscar solicitud y listar por estudiante
  Archivos: `aplicacion/puertos/repositorio_certificados.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: ninguna

- [ ] T-02 DTO ConsultarEstadoEntrada
  Archivos: `aplicacion/dto/consultar_estado_entrada.py`
  Escenarios: 1, 3, 5
  Depende de: ninguna

- [ ] T-03 DTO ConsultarEstadoSalida
  Archivos: `aplicacion/dto/consultar_estado_salida.py`
  Escenarios: 1, 2, 3, 4
  Depende de: ninguna

- [ ] T-04 Error AccesoDenegado
  Archivos: `aplicacion/errores/acceso_denegado.py`
  Escenarios: 5
  Depende de: ninguna

- [ ] T-05 Caso de uso ConsultarEstado
  Archivos: `aplicacion/consultar_estado.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-01, T-02, T-03, T-04

- [ ] T-06 Actualizar adaptador RepositorioCertificadosMemoria con consultas
  Archivos: `infraestructura/repositorio_certificados_memoria.py`
  Escenarios: 1, 2, 3, 4
  Depende de: T-01

- [ ] T-07 Ampliar main.py con los cinco escenarios de consulta
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-05, T-06

- [ ] T-08 Validar los escenarios 1 a 5
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-07
