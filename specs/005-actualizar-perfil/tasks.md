# Tareas: 005 Actualizar perfil

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Actualizar entidad Estudiante con correo y autoriza_correo
  Archivos: `dominio/estudiante.py`
  Escenarios: 1, 2, 3
  Depende de: ninguna

- [ ] T-02 Ampliar puerto RepositorioCertificados con guardar estudiante
  Archivos: `aplicacion/puertos/repositorio_certificados.py`
  Escenarios: 1, 3
  Depende de: T-01

- [ ] T-03 DTO ActualizarPerfilEntrada
  Archivos: `aplicacion/dto/actualizar_perfil_entrada.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: ninguna

- [ ] T-04 DTO PerfilSalida
  Archivos: `aplicacion/dto/perfil_salida.py`
  Escenarios: 1, 3
  Depende de: ninguna

- [ ] T-05 Error DatoInvalido
  Archivos: `aplicacion/errores/dato_invalido.py`
  Escenarios: 2, 4
  Depende de: ninguna

- [ ] T-06 Caso de uso ActualizarPerfil
  Archivos: `aplicacion/actualizar_perfil.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-01, T-02, T-03, T-04, T-05

- [ ] T-07 Actualizar adaptador RepositorioCertificadosMemoria con estudiantes
  Archivos: `infraestructura/repositorio_certificados_memoria.py`
  Escenarios: 1, 3
  Depende de: T-02

- [ ] T-08 Ampliar main.py con los cinco escenarios de perfil
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-06, T-07

- [ ] T-09 Validar los escenarios 1 a 5
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-08
