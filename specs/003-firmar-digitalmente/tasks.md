# Tareas: 003 Firmar digitalmente

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Entidad Certificado (fecha de emisión y ruta del PDF)
  Archivos: `dominio/certificado.py`
  Escenarios: 1
  Depende de: ninguna

- [ ] T-02 Entidad IntentoFirma (número, fecha, resultado)
  Archivos: `dominio/intento_firma.py`
  Escenarios: 1, 2, 4
  Depende de: ninguna

- [ ] T-03 Actualizar entidad Solicitud con marcar en firma, emitida e intentos
  Archivos: `dominio/solicitud.py`
  Escenarios: 1, 2, 4, 7
  Depende de: T-01, T-02

- [ ] T-04 Puerto ProveedorFirma
  Archivos: `aplicacion/puertos/proveedor_firma.py`
  Escenarios: 1, 2
  Depende de: T-03

- [ ] T-05 Puerto AlmacenPdf (guardar)
  Archivos: `aplicacion/puertos/almacen_pdf.py`
  Escenarios: 1
  Depende de: T-01

- [ ] T-06 Puerto Notificador
  Archivos: `aplicacion/puertos/notificador.py`
  Escenarios: 1, 5, 6
  Depende de: T-03

- [ ] T-07 Puerto ColaFirmas (reencolar y enviar a la cola de errores)
  Archivos: `aplicacion/puertos/cola_firmas.py`
  Escenarios: 2, 3, 4
  Depende de: T-03

- [ ] T-08 Política de reintentos (5 intentos, esperas de 2, 4, 8 y 16 s)
  Archivos: `aplicacion/politica_reintentos.py`
  Escenarios: 2, 3, 4
  Depende de: ninguna

- [ ] T-09 Error ProveedorFirmaNoDisponible
  Archivos: `aplicacion/errores/proveedor_firma_no_disponible.py`
  Escenarios: 2, 3, 4
  Depende de: ninguna

- [ ] T-10 Error NotificacionFallida
  Archivos: `aplicacion/errores/notificacion_fallida.py`
  Escenarios: 6
  Depende de: ninguna

- [ ] T-11 Caso de uso FirmarCertificado
  Archivos: `aplicacion/firmar_certificado.py`
  Escenarios: 1, 2, 3, 4, 5, 6, 7
  Depende de: T-04, T-05, T-06, T-07, T-08, T-09, T-10

- [ ] T-12 Adaptador ProveedorFirmaSimulado (éxito, lento y caído)
  Archivos: `infraestructura/proveedor_firma_simulado.py`
  Escenarios: 1, 2, 3, 4
  Depende de: T-04, T-09

- [ ] T-13 Adaptador AlmacenPdfMemoria
  Archivos: `infraestructura/almacen_pdf_memoria.py`
  Escenarios: 1
  Depende de: T-05

- [ ] T-14 Adaptador NotificadorCorreoSimulado
  Archivos: `infraestructura/notificador_correo_simulado.py`
  Escenarios: 1, 5, 6
  Depende de: T-06, T-10

- [ ] T-15 Adaptador ColaFirmasMemoria
  Archivos: `infraestructura/cola_firmas_memoria.py`
  Escenarios: 2, 3, 4
  Depende de: T-07

- [ ] T-16 Ampliar main.py con los siete escenarios de firma
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5, 6, 7
  Depende de: T-11, T-12, T-13, T-14, T-15

- [ ] T-17 Validar los escenarios 1 a 7
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5, 6, 7
  Depende de: T-16
