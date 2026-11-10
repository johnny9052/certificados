# Plan: 001 Solicitar certificado

Traza: CU-01, EAC-01, EAC-04, ADR-001, ADR-002, ADR-004, ADR-005, ADR-006. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-001: la firma va a una cola; la API no espera al proveedor de firma. El caso de uso solo publica `SolicitudFirma`.
- ADR-002: monolito modular con Clean Architecture; módulo Solicitudes.
- ADR-004: la identidad llega ya validada; el caso de uso verifica que el solicitante sea el titular.
- ADR-005: cada cambio de estado y cada acceso rechazado se registra en la bitácora.
- ADR-006: las fallas del SIA se traducen a un error de aplicación; el caso de uso no conoce excepciones externas.
- RT-04: el SIA se consulta solo por el puerto `ConsultaAcademica`.

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`.

| Capa | Archivo | Clase o puerto |
|------|---------|----------------|
| dominio | `dominio/estudiante.py` | `Estudiante` |
| dominio | `dominio/tipo_certificado.py` | `TipoCertificado` |
| dominio | `dominio/estado_solicitud.py` | `EstadoSolicitud` |
| dominio | `dominio/pago.py` | `Pago` |
| dominio | `dominio/solicitud.py` | `Solicitud` |
| aplicacion | `aplicacion/puertos/repositorio_certificados.py` | `RepositorioCertificados` |
| aplicacion | `aplicacion/puertos/consulta_academica.py` | `ConsultaAcademica` |
| aplicacion | `aplicacion/puertos/publicador_firmas.py` | `PublicadorFirmas` |
| aplicacion | `aplicacion/puertos/pasarela_pagos.py` | `PasarelaPagos` (solo consulta del estado del pago) |
| aplicacion | `aplicacion/puertos/bitacora.py` | `Bitacora` |
| aplicacion | `aplicacion/dto/solicitar_certificado_entrada.py` | `SolicitarCertificadoEntrada` |
| aplicacion | `aplicacion/dto/solicitar_certificado_salida.py` | `SolicitarCertificadoSalida` |
| aplicacion | `aplicacion/errores/solicitud_rechazada.py` | `SolicitudRechazada` |
| aplicacion | `aplicacion/errores/verificacion_academica_no_disponible.py` | `VerificacionAcademicaNoDisponible` |
| aplicacion | `aplicacion/solicitar_certificado.py` | `SolicitarCertificado` |
| infraestructura | `infraestructura/repositorio_certificados_memoria.py` | `RepositorioCertificadosMemoria` |
| infraestructura | `infraestructura/consulta_academica_simulada.py` | `ConsultaAcademicaSimulada` (permite simular deuda y falta de respuesta) |
| infraestructura | `infraestructura/publicador_firmas_memoria.py` | `PublicadorFirmasMemoria` |
| infraestructura | `infraestructura/pasarela_pagos_simulada.py` | `PasarelaPagosSimulada` |
| infraestructura | `infraestructura/bitacora_memoria.py` | `BitacoraMemoria` |
| raíz | `main.py` | Composición: crea los adaptadores y los inyecta en el caso de uso |

El controlador REST, el validador de token y los adaptadores reales (PostgreSQL, RabbitMQ, SIA y pagos) quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

[diagramas/secuencias/cu01-solicitar-certificado.puml](../../docs/diagramas/secuencias/cu01-solicitar-certificado.puml)

Orden del caso de uso `SolicitarCertificado.ejecutar(entrada)`:

1. Verificar que el solicitante sea el titular; si no, registrar el intento en la bitácora y rechazar (escenario 5).
2. Consultar paz y salvo por `ConsultaAcademica`; si el SIA no responde, lanzar `VerificacionAcademicaNoDisponible` (escenario 6); si hay deuda, rechazar con la dependencia pendiente (escenario 3).
3. Si el tipo requiere pago, consultar el estado del pago por `PasarelaPagos`; sin pago aprobado, rechazar (escenario 4).
4. Crear la `Solicitud` en estado registrada, con su código de verificación, y guardarla.
5. Publicar `SolicitudFirma` por `PublicadorFirmas` y registrar en la bitácora.
6. Devolver `SolicitarCertificadoSalida` con el código de verificación (escenarios 1 y 2).

En el diagrama la consulta del SIA y la del pago ocurren en paralelo (fragmento par). En esta etapa se ejecutan en secuencia dentro del caso de uso; el paralelismo es una optimización que se evalúa contra EAC-01 si la medida no se cumple.

## Fuera de alcance

- La firma (CU-03) y el pago (CU-02).
- Controlador REST, autenticación real, PostgreSQL, RabbitMQ y llamadas reales a servicios externos.
- Pruebas automatizadas (la validación es escenario por escenario, con evidencia).
