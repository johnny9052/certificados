# ADR-007: Modelar los tipos de certificado y sus parámetros como datos en PostgreSQL

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

El objetivo OB-03 pide incorporar nuevos tipos de certificado sin detener el servicio, y [EAC-03](../drivers.md#eac-03-mantenibilidad-modificabilidad) limita el cambio a 2 módulos o menos. La preocupación PA-05 pide decidir cómo se parametrizan tipos, valores y reintentos sin recompilar, y PA-07 cómo se carga la información inicial del sistema (catálogo de tipos y valores). PostgreSQL ya es el almacén del trámite (ADR-003).

## Alternativas consideradas

- **A) Un tipo de certificado por clase en el código.** Permite lógica propia por tipo; pero cada tipo nuevo exige modificar y desplegar código, contra EAC-03.
- **B) Tipo de certificado como dato (nombre, valor, requiere pago, plantilla) en una tabla, con carga inicial mediante migraciones versionadas.** Un tipo nuevo es una fila; la plantilla del PDF se selecciona por dato; exige que las diferencias entre tipos sean parametrizables.
- **C) Archivos de configuración en disco.** Sencillo; pero cambiarlos exige reiniciar y no queda auditoría de los cambios.

## Decisión

Se elige la opción B, porque cumple EAC-03 y OB-03: agregar un tipo no toca código ni detiene el servicio. Los secretos (credenciales del proveedor, de la pasarela y del broker) no van en la base: se entregan por variables de entorno. La carga inicial (PA-07) la hacen migraciones versionadas en el repositorio, con los tipos S-05 (notas, matrícula y egresado) como datos semilla.

## Consecuencias

### Positivas

- Nuevos tipos de certificado sin despliegue de código.
- La carga inicial es repetible y está versionada.

### Negativas y costos aceptados

- Si un tipo requiere lógica distinta (no solo parámetros), esta decisión no basta y habrá que escribir un ADR nuevo.
- Hay que proteger el acceso de escritura a la tabla de tipos.

### Riesgos asumidos

- Plantillas de PDF mal parametrizadas. Mitigación: validar cada tipo nuevo con una solicitud de prueba antes de habilitarlo.

### Vistas que modifica

[Lógica](../vistas/01-logica.md) (TipoCertificado como dato) y [Desarrollo](../vistas/03-desarrollo.md) (módulo Solicitudes).
