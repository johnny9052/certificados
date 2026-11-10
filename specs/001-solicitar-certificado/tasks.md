# Tareas: 001 Solicitar certificado

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Entidad Estudiante
  Archivos: `dominio/estudiante.py`
  Escenarios: 1, 5
  Depende de: ninguna

- [ ] T-02 Entidad TipoCertificado (nombre, valor, requiere pago)
  Archivos: `dominio/tipo_certificado.py`
  Escenarios: 1, 4
  Depende de: ninguna

- [ ] T-03 Enumeración EstadoSolicitud
  Archivos: `dominio/estado_solicitud.py`
  Escenarios: 1
  Depende de: ninguna

- [ ] T-04 Entidad Pago
  Archivos: `dominio/pago.py`
  Escenarios: 4
  Depende de: ninguna

- [ ] T-05 Entidad Solicitud y su ciclo de estados
  Archivos: `dominio/solicitud.py`
  Escenarios: 1, 3, 4
  Depende de: T-01, T-02, T-03

- [ ] T-06 Puerto RepositorioCertificados
  Archivos: `aplicacion/puertos/repositorio_certificados.py`
  Escenarios: 1
  Depende de: T-05

- [ ] T-07 Puerto ConsultaAcademica
  Archivos: `aplicacion/puertos/consulta_academica.py`
  Escenarios: 1, 3, 6
  Depende de: T-01

- [ ] T-08 Puerto PublicadorFirmas
  Archivos: `aplicacion/puertos/publicador_firmas.py`
  Escenarios: 1, 2
  Depende de: T-05

- [ ] T-09 Puerto PasarelaPagos (consulta del estado del pago)
  Archivos: `aplicacion/puertos/pasarela_pagos.py`
  Escenarios: 4
  Depende de: T-04

- [ ] T-10 Puerto Bitacora
  Archivos: `aplicacion/puertos/bitacora.py`
  Escenarios: 3, 5
  Depende de: ninguna

- [ ] T-11 DTO SolicitarCertificadoEntrada
  Archivos: `aplicacion/dto/solicitar_certificado_entrada.py`
  Escenarios: 1, 5
  Depende de: ninguna

- [ ] T-12 DTO SolicitarCertificadoSalida
  Archivos: `aplicacion/dto/solicitar_certificado_salida.py`
  Escenarios: 1, 2
  Depende de: ninguna

- [ ] T-13 Error SolicitudRechazada
  Archivos: `aplicacion/errores/solicitud_rechazada.py`
  Escenarios: 3, 4, 5
  Depende de: ninguna

- [ ] T-14 Error VerificacionAcademicaNoDisponible
  Archivos: `aplicacion/errores/verificacion_academica_no_disponible.py`
  Escenarios: 6
  Depende de: ninguna

- [ ] T-15 Caso de uso SolicitarCertificado
  Archivos: `aplicacion/solicitar_certificado.py`
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-05 a T-14

- [ ] T-16 Adaptador RepositorioCertificadosMemoria
  Archivos: `infraestructura/repositorio_certificados_memoria.py`
  Escenarios: 1
  Depende de: T-06

- [ ] T-17 Adaptador ConsultaAcademicaSimulada (con deuda y sin respuesta)
  Archivos: `infraestructura/consulta_academica_simulada.py`
  Escenarios: 3, 6
  Depende de: T-07, T-14

- [ ] T-18 Adaptador PublicadorFirmasMemoria
  Archivos: `infraestructura/publicador_firmas_memoria.py`
  Escenarios: 1, 2, 3
  Depende de: T-08

- [ ] T-19 Adaptador PasarelaPagosSimulada
  Archivos: `infraestructura/pasarela_pagos_simulada.py`
  Escenarios: 4
  Depende de: T-09

- [ ] T-20 Adaptador BitacoraMemoria
  Archivos: `infraestructura/bitacora_memoria.py`
  Escenarios: 5
  Depende de: T-10

- [ ] T-21 Composición en main.py y ejecución de los seis escenarios
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-15 a T-20

- [ ] T-22 Validar los escenarios 1 a 6
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5, 6
  Depende de: T-21
