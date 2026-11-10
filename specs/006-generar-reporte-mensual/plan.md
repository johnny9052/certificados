# Plan: 006 Generar reporte mensual

Traza: CU-06, OB-01, EAC-04, ADR-004, ADR-005. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-004: la autorización de este caso de uso exige el rol Funcionario de Registro.
- ADR-005: los accesos rechazados se registran en la bitácora.
- CU-06 no estructura nada: opera sobre datos ya existentes (drivers.md); se resuelve con una consulta del repositorio, sin infraestructura nueva.

## Dependencias con otras specs

Requiere la spec 001 (entidad `Solicitud`, `RepositorioCertificados`, `Bitacora`) y reutiliza el error `AccesoDenegado` de la spec 004. El estado `emitida` y la cola de errores los produce la spec 003.

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| dominio | `dominio/rol_usuario.py` | Enumeración RolUsuario (estudiante, funcionario de registro) |
| aplicacion | `aplicacion/puertos/repositorio_certificados.py` | Ampliar puerto RepositorioCertificados con conteo de emitidas por tipo y de la cola de errores en un mes |
| aplicacion | `aplicacion/dto/generar_reporte_entrada.py` | DTO GenerarReporteEntrada |
| aplicacion | `aplicacion/dto/reporte_mensual_salida.py` | DTO ReporteMensualSalida |
| aplicacion | `aplicacion/generar_reporte_mensual.py` | Caso de uso GenerarReporteMensual |
| infraestructura | `infraestructura/repositorio_certificados_memoria.py` | Actualizar adaptador RepositorioCertificadosMemoria con los conteos |
| raíz | `main.py` | Ampliar main.py con los cinco escenarios de reporte |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

Sin diagrama de secuencia propio (no primario). Pasos:

1. `GenerarReporteMensual.ejecutar(entrada)`: verificar el rol del solicitante; si no es Funcionario, registrar y rechazar (escenario 5).
2. Validar que el mes no sea futuro (escenario 4).
3. Pedir al repositorio el conteo de solicitudes emitidas por tipo y el de la cola de errores (escenarios 1, 2 y 3).
4. Devolver `ReporteMensualSalida` con totales por tipo y total general.

## Fuera de alcance

- Exportación a PDF o Excel.
- Reportes por programa o por rango de fechas.
- Reportes financieros de pagos.
- Pruebas automatizadas.
