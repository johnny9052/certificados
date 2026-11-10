# Plan: 007 Descargar certificado firmado

Traza: CU-07, EAC-04, RN-02, ADR-003, ADR-004, ADR-005, ADR-006. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-003: el PDF vive en el repositorio de archivos institucional; la base guarda solo la ruta (RN-02: no sale del país).
- ADR-004: solo el titular descarga su certificado.
- ADR-005: cada descarga y cada acceso rechazado se registran.
- ADR-006: el archivo faltante se traduce a un error de aplicación (DT-02 de arquitectura.md).

## Dependencias con otras specs

Requiere la spec 003 (puerto `AlmacenPdf`, entidad `Certificado`) y la spec 004 (error `AccesoDenegado` y búsqueda de solicitudes en el repositorio).

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| aplicacion | `aplicacion/puertos/almacen_pdf.py` | Ampliar puerto AlmacenPdf con leer por ruta |
| aplicacion | `aplicacion/dto/descargar_certificado_entrada.py` | DTO DescargarCertificadoEntrada |
| aplicacion | `aplicacion/dto/descargar_certificado_salida.py` | DTO DescargarCertificadoSalida |
| aplicacion | `aplicacion/errores/solicitud_no_encontrada.py` | Error SolicitudNoEncontrada |
| aplicacion | `aplicacion/errores/certificado_no_disponible.py` | Error CertificadoNoDisponible |
| aplicacion | `aplicacion/descargar_certificado.py` | Caso de uso DescargarCertificado |
| infraestructura | `infraestructura/almacen_pdf_memoria.py` | Actualizar adaptador AlmacenPdfMemoria con lectura |
| raíz | `main.py` | Ampliar main.py con los cinco escenarios de descarga |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

Sin diagrama de secuencia propio (no primario; la seguridad la cubre EAC-04). Pasos:

1. `DescargarCertificado.ejecutar(entrada)`: buscar la solicitud; si no existe, lanzar `SolicitudNoEncontrada` (escenario 5).
2. Si no pertenece al solicitante, registrar el intento y rechazar (escenario 3).
3. Si no está emitida, informar el estado actual (escenario 2).
4. Leer el PDF por `AlmacenPdf`; si no existe, registrar la inconsistencia y lanzar `CertificadoNoDisponible` (escenario 4).
5. Registrar la descarga y devolver `DescargarCertificadoSalida` con el contenido (escenario 1).

## Fuera de alcance

- Generación del PDF y firma (spec 003).
- Página pública de verificación de autenticidad.
- Controlador REST y entrega HTTP del archivo.
- Pruebas automatizadas.
