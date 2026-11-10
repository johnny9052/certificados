# Tareas: 007 Descargar certificado firmado

Traza: [spec.md](spec.md) y [plan.md](plan.md). Rutas relativas a `src/`. Cada tarea toca una sola clase o archivo y se implementa en una sesión, con un commit que lleva el ID de la tarea. El orden va de adentro hacia afuera, como las capas de Clean Architecture.

- [ ] T-01 Ampliar puerto AlmacenPdf con leer por ruta
  Archivos: `aplicacion/puertos/almacen_pdf.py`
  Escenarios: 1, 4
  Depende de: ninguna

- [ ] T-02 DTO DescargarCertificadoEntrada
  Archivos: `aplicacion/dto/descargar_certificado_entrada.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: ninguna

- [ ] T-03 DTO DescargarCertificadoSalida
  Archivos: `aplicacion/dto/descargar_certificado_salida.py`
  Escenarios: 1
  Depende de: ninguna

- [ ] T-04 Error SolicitudNoEncontrada
  Archivos: `aplicacion/errores/solicitud_no_encontrada.py`
  Escenarios: 5
  Depende de: ninguna

- [ ] T-05 Error CertificadoNoDisponible
  Archivos: `aplicacion/errores/certificado_no_disponible.py`
  Escenarios: 2, 4
  Depende de: ninguna

- [ ] T-06 Caso de uso DescargarCertificado
  Archivos: `aplicacion/descargar_certificado.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-01, T-02, T-03, T-04, T-05

- [ ] T-07 Actualizar adaptador AlmacenPdfMemoria con lectura
  Archivos: `infraestructura/almacen_pdf_memoria.py`
  Escenarios: 1, 4
  Depende de: T-01

- [ ] T-08 Ampliar main.py con los cinco escenarios de descarga
  Archivos: `main.py`
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-06, T-07

- [ ] T-09 Validar los escenarios 1 a 5
  Produce: `validacion.md` (escenario por escenario, con evidencia, y revisión de la regla de dependencia)
  Escenarios: 1, 2, 3, 4, 5
  Depende de: T-08
