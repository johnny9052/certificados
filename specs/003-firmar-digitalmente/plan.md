# Plan: 003 Firmar digitalmente

Traza: CU-03, EAC-02, RT-03, ADR-001, ADR-003, ADR-005, ADR-006. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-001: el worker es el único que habla con el proveedor de firma; reintenta hasta 5 veces con espera creciente y envía a la cola de errores al agotarlos.
- ADR-003: el PDF firmado se guarda en el repositorio de PDF antes de actualizar el estado; PostgreSQL guarda el estado y los intentos.
- ADR-005: cada intento de firma y cada cambio de estado se registran.
- ADR-006: las fallas del proveedor y del correo se traducen a errores de aplicación; una falla del correo no revierte la emisión.
- RT-03: el proveedor responde entre 2 y 8 s y no garantiza disponibilidad; el tiempo máximo de espera por intento es de 10 s.

## Dependencias con otras specs

Requiere la spec 001 completa (entidad `Solicitud`, `RepositorioCertificados`, `PublicadorFirmas`, `Bitacora`) y el atributo `autoriza_correo` del estudiante definido en la spec 005 (mientras no exista, se asume autorizado).

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| dominio | `dominio/certificado.py` | Entidad Certificado (fecha de emisión y ruta del PDF) |
| dominio | `dominio/intento_firma.py` | Entidad IntentoFirma (número, fecha, resultado) |
| dominio | `dominio/solicitud.py` | Actualizar entidad Solicitud con marcar en firma, emitida e intentos |
| aplicacion | `aplicacion/puertos/proveedor_firma.py` | Puerto ProveedorFirma |
| aplicacion | `aplicacion/puertos/almacen_pdf.py` | Puerto AlmacenPdf (guardar) |
| aplicacion | `aplicacion/puertos/notificador.py` | Puerto Notificador |
| aplicacion | `aplicacion/puertos/cola_firmas.py` | Puerto ColaFirmas (reencolar y enviar a la cola de errores) |
| aplicacion | `aplicacion/politica_reintentos.py` | Política de reintentos (5 intentos, esperas de 2, 4, 8 y 16 s) |
| aplicacion | `aplicacion/errores/proveedor_firma_no_disponible.py` | Error ProveedorFirmaNoDisponible |
| aplicacion | `aplicacion/errores/notificacion_fallida.py` | Error NotificacionFallida |
| aplicacion | `aplicacion/firmar_certificado.py` | Caso de uso FirmarCertificado |
| infraestructura | `infraestructura/proveedor_firma_simulado.py` | Adaptador ProveedorFirmaSimulado (éxito, lento y caído) |
| infraestructura | `infraestructura/almacen_pdf_memoria.py` | Adaptador AlmacenPdfMemoria |
| infraestructura | `infraestructura/notificador_correo_simulado.py` | Adaptador NotificadorCorreoSimulado |
| infraestructura | `infraestructura/cola_firmas_memoria.py` | Adaptador ColaFirmasMemoria |
| raíz | `main.py` | Ampliar main.py con los siete escenarios de firma |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

[diagramas/secuencias/cu03-firmar-digitalmente.puml](../../docs/diagramas/secuencias/cu03-firmar-digitalmente.puml)

Pasos:

1. `FirmarCertificado.ejecutar(id_solicitud)`: cargar la solicitud; si ya está emitida, descartar el mensaje (escenario 7).
2. Marcar la solicitud en firma y pedir la firma al proveedor con un tiempo máximo de 10 s.
3. Si responde: guardar el PDF, registrar el intento exitoso, marcar emitida y notificar si el estudiante autorizó el correo (escenarios 1, 5 y 6).
4. Si no responde o falla: registrar el intento fallido y, si quedan intentos, reencolar con la espera de `PoliticaReintentos`; si se agotaron los 5, enviar a la cola de errores (escenarios 2, 3 y 4).

## Fuera de alcance

- La solicitud y el pago (CU-01 y CU-02).
- La revisión de la cola de errores por el Funcionario de Registro (spec 008).
- El proveedor real, RabbitMQ real y el envío de correo real.
- Pruebas automatizadas.
