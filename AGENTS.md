# AGENTS.md: Sistema de certificados en línea

Reglas permanentes del proyecto. Se le entregan al agente al inicio de cada sesión junto con `docs/arquitectura.md`. Es corto: enlaza a `docs/` en lugar de copiar su contenido.

## Fuentes de verdad

- `docs/arquitectura.md`: mapa de visión, drivers, ADR, vistas y glosario.
- `docs/drivers.md`, sección 1: catálogo de casos de uso. Es la lista de funcionalidades del sistema.
- `specs/`: una carpeta por caso de uso del catálogo, con el mismo número (CU-03 → `specs/003-...`), con `spec.md`, `plan.md`, `tasks.md` y `validacion.md`. La columna "Se especifica en" del catálogo enlaza cada caso de uso con su carpeta.
- Si el código contradice una especificación, gana la especificación. Si la especificación parece incorrecta, detenerse y preguntar.
- Los ADR no se editan: una decisión nueva se escribe como ADR nuevo que reemplaza al anterior.

## Arquitectura

- Monolito modular (ADR-002), con Clean Architecture por dentro.
- Todo el codigo va en la carpeta `src/` 
- `dominio/` no importa nada de `aplicacion/` ni de `infraestructura/`.
- Puertos como `Protocol` en `aplicacion/puertos/` (los define el caso de uso; los implementa `infraestructura/`).
- La firma es asíncrona (ADR-001): la API solo encola; nunca llama al proveedor de firma. Solo el worker lo hace.
- PostgreSQL (ADR-003); en esta etapa, adaptadores en memoria.
- Errores de integración traducidos por los adaptadores a errores de aplicación (ADR-006).
- Toda solicitud pertenece a un Estudiante y solo su titular la consulta (ADR-004).
- Se audita cada cambio de estado y cada acceso rechazado (ADR-005).

## Convenciones de código

- Python 3.12; una clase por archivo; sin `__init__.py`.
- DTO en la entrada y la salida de los casos de uso.
- Nombres según el glosario (sección 10 de `docs/arquitectura.md`).
- Sin base de datos real ni servicios externos reales en esta etapa: adaptadores en memoria o simulados.
- Sin carpeta de pruebas automatizadas en esta etapa: la validación se hace escenario por escenario, con evidencia.

## Forma de trabajar

- Una tarea de `tasks.md` a la vez.
- Al terminar: marcar la tarea, listar los archivos cambiados y detenerse.
- No inventar requisitos: lo que no esté en los documentos se marca en `spec.md` con `# [NECESITA ACLARACIÓN: <pregunta>]` o se pregunta.
- No agregar librerías que ningún ADR haya aprobado.
- Un commit por tarea, con el ID de la tarea en el mensaje.
