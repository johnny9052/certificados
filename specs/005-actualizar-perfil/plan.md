# Plan: 005 Actualizar perfil

Traza: CU-05, EAC-04, ADR-004, ADR-005. Especificación: [spec.md](spec.md).

## Decisiones que aplican

- ADR-004: solo el titular modifica su perfil (autorización por titularidad).
- ADR-005: los accesos rechazados se registran en la bitácora.
- Documento y programa provienen del SIA (RT-04) y no se editan en este sistema.
- Nota: `correo` y `autoriza_correo` son atributos de apoyo de `Estudiante`; no cambian el modelo de dominio de la [vista lógica](../../docs/vistas/01-logica.md), que muestra solo atributos clave.

## Dependencias con otras specs

Requiere la spec 001 (entidad `Estudiante`, `RepositorioCertificados`, `Bitacora`) y reutiliza el error `AccesoDenegado` de la spec 004. Su atributo `autoriza_correo` lo consume la spec 003.

## Componentes (vistas/03-desarrollo.md)

Rutas relativas a `src/`. Una clase por archivo, sin `__init__.py`. Los archivos que ya existen por una spec anterior se amplían, no se duplican.

| Capa | Archivo | Cambio |
|------|---------|--------|
| dominio | `dominio/estudiante.py` | Actualizar entidad Estudiante con correo y autoriza_correo |
| aplicacion | `aplicacion/puertos/repositorio_certificados.py` | Ampliar puerto RepositorioCertificados con guardar estudiante |
| aplicacion | `aplicacion/dto/actualizar_perfil_entrada.py` | DTO ActualizarPerfilEntrada |
| aplicacion | `aplicacion/dto/perfil_salida.py` | DTO PerfilSalida |
| aplicacion | `aplicacion/errores/dato_invalido.py` | Error DatoInvalido |
| aplicacion | `aplicacion/actualizar_perfil.py` | Caso de uso ActualizarPerfil |
| infraestructura | `infraestructura/repositorio_certificados_memoria.py` | Actualizar adaptador RepositorioCertificadosMemoria con estudiantes |
| raíz | `main.py` | Ampliar main.py con los cinco escenarios de perfil |

El controlador REST, el validador de token y los adaptadores reales quedan para una etapa posterior: en esta etapa se usan adaptadores en memoria o simulados.

## Flujo

Sin diagrama de secuencia propio (CRUD estándar, no primario). Pasos:

1. `ActualizarPerfil.ejecutar(entrada)`: verificar que el solicitante sea el titular; si no, registrar y rechazar (escenario 5).
2. Rechazar cualquier intento de modificar documento o programa (escenario 4).
3. Validar el formato del correo; si es inválido, lanzar `DatoInvalido` sin guardar (escenario 2).
4. Guardar el correo y la autorización y devolver el perfil actualizado (escenarios 1 y 3).

## Fuera de alcance

- Cambio de contraseña o credenciales (la identidad es institucional, ADR-004).
- Verificación del correo mediante un mensaje de confirmación.
- Controlador REST y autenticación real.
- Pruebas automatizadas.
