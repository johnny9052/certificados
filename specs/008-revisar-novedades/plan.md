# Plan: 008 Revisar solicitudes con novedades

Traza: CU-08, EAC-02, EAC-04, DT-01, ADR-001, ADR-004, ADR-005. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-001: las solicitudes que agotan los 5 intentos quedan en la cola de errores (DT-01); esta spec la resuelve de forma manual.
- ADR-004: solo el rol Funcionario de Registro accede.
- ADR-005: cada decisión del funcionario se registra.
- Supuesto S-08 (por validar con Registro): el reprocesamiento manual inicia un nuevo ciclo de hasta 5 intentos. El modelo de dominio fija 0..5 intentos por solicitud; si Registro no acepta el ciclo nuevo, se requiere un ADR que reemplace esa regla (ver DT-04 en arquitectura.md).

## Dependencias con otras specs

Requiere la spec 003 (`ColaFirmas`, intentos de firma, `Notificador`), la spec 004 (error `AccesoDenegado`) y la spec 006 (enumeración `RolUsuario`).

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| dominio | `dominio/solicitud.py` | Actualizar entidad Solicitud con motivo de rechazo y rechazar |
| aplicacion | `aplicacion/puertos/cola_firmas.py` | Ampliar puerto ColaFirmas con listar la cola de errores y reprocesar |
| aplicacion | `aplicacion/dto/listar_novedades_entrada.py` | DTO ListarNovedadesEntrada |
| aplicacion | `aplicacion/dto/novedad_salida.py` | DTO NovedadSalida |
| aplicacion | `aplicacion/dto/resolver_novedad_entrada.py` | DTO ResolverNovedadEntrada |
| aplicacion | `aplicacion/listar_novedades.py` | Caso de uso ListarNovedades |
| aplicacion | `aplicacion/resolver_novedad.py` | Caso de uso ResolverNovedad (reprocesar o rechazar) |
| infraestructura | `infraestructura/cola_firmas_memoria.py` | Actualizar adaptador ColaFirmasMemoria con lista y reproceso |
| raíz | `main.py` | Ampliar main.py con los seis escenarios de revisión |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

Sin diagrama de secuencia propio (no primario). Pasos:

1. `RevisarNovedades`: verificar el rol; si no es Funcionario, registrar y rechazar (escenario 6).
2. Listar las solicitudes en la cola de errores (escenarios 1 y 2).
3. Reprocesar: reencolar en la cola de firma y registrar la decisión (escenario 3).
4. Rechazar: exigir motivo (escenario 5), marcar rechazada con el motivo y notificar al estudiante (escenario 4).

## Fuera de alcance

- Reprocesamiento automático de la cola de errores (mejora posterior de DT-01).
- Revisión de solicitudes rechazadas por paz y salvo.
- Controlador REST y autenticación real.
- Pruebas automatizadas.
