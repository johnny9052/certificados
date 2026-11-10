# Plan: 004 Consultar estado de la solicitud

Traza: CU-04, EAC-04, ADR-004, ADR-005. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-004: la autorización por titularidad se aplica en el caso de uso.
- ADR-005: los accesos rechazados se registran en la bitácora.
- CU-04 no es primario: no tiene diagrama de secuencia propio; se reutiliza el puerto `RepositorioCertificados`.

## Dependencias con otras specs

Requiere la spec 001 (entidad `Solicitud`, `RepositorioCertificados`, `Bitacora`). El error `AccesoDenegado` se reutiliza en las specs 005, 006, 007 y 008.

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| aplicacion | `aplicacion/puertos/repositorio_certificados.py` | Ampliar puerto RepositorioCertificados con buscar solicitud y listar por estudiante |
| aplicacion | `aplicacion/dto/consultar_estado_entrada.py` | DTO ConsultarEstadoEntrada |
| aplicacion | `aplicacion/dto/consultar_estado_salida.py` | DTO ConsultarEstadoSalida |
| aplicacion | `aplicacion/errores/acceso_denegado.py` | Error AccesoDenegado |
| aplicacion | `aplicacion/consultar_estado.py` | Caso de uso ConsultarEstado |
| infraestructura | `infraestructura/repositorio_certificados_memoria.py` | Actualizar adaptador RepositorioCertificadosMemoria con consultas |
| raíz | `main.py` | Ampliar main.py con los cinco escenarios de consulta |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

Sin diagrama de secuencia propio (no es caso primario ni lleva un EAC). Pasos:

1. `ConsultarEstado.ejecutar(entrada)`: si se pide una solicitud, buscarla; si no pertenece al solicitante, registrar el intento en la bitácora y rechazar (escenario 5).
2. Devolver `ConsultarEstadoSalida` con estado, tipo, fecha, código de verificación y si ya se puede descargar (escenarios 1 y 2).
3. Si se pide el listado, devolver las solicitudes del estudiante ordenadas por fecha descendente; vacío si no hay (escenarios 3 y 4).

## Fuera de alcance

- La descarga del PDF (spec 007).
- Notificaciones de cambio de estado.
- Controlador REST y autenticación real.
- Pruebas automatizadas.
