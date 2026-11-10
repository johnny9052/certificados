# ADR-006: Traducir las fallas de integración a errores de aplicación con respuesta HTTP unificada

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

La preocupación PA-03 pide una estrategia unificada de errores. El sistema depende de tres servicios externos (SIA por RT-04, pasarela de pagos y proveedor de firma por RT-03) que pueden fallar de maneras distintas. [EAC-02](../drivers.md#eac-02-fiabilidad-tolerancia-a-fallos) exige que ninguna solicitud se pierda sin explicación, y la regla de dependencia de ADR-002 prohíbe que el dominio conozca excepciones de infraestructura.

## Alternativas consideradas

- **A) Dejar pasar las excepciones de cada librería.** Sin código extra; pero el dominio y el controlador quedan acoplados a los detalles externos y las respuestas son inconsistentes.
- **B) Los adaptadores traducen sus fallas a excepciones de aplicación definidas en los puertos, y el controlador las convierte en una respuesta HTTP unificada (código, mensaje y ID de solicitud).** Mantiene limpia la regla de dependencia; exige definir y mantener el catálogo de errores.

## Decisión

Se elige la opción B, porque protege la regla de dependencia de ADR-002 y permite mensajes claros al usuario (por ejemplo, la dependencia pendiente cuando no hay paz y salvo). Las fallas del proveedor de firma no llegan al usuario: las absorbe el worker con reintentos (ADR-001) y, al agotarse, la solicitud pasa a la cola de errores.

## Consecuencias

### Positivas

- Dominio y aplicación no conocen excepciones externas.
- Respuestas de error homogéneas y fáciles de diagnosticar.

### Negativas y costos aceptados

- Un catálogo de errores que mantener y más clases de excepción.
- Cada adaptador debe implementar la traducción con cuidado.

### Riesgos asumidos

- Errores no mapeados que llegan como fallas genéricas. Mitigación: respuesta por defecto de error interno con el ID de solicitud para soporte.

### Vistas que modifica

[Desarrollo](../vistas/03-desarrollo.md) (adaptadores y controlador REST) y la secuencia [CU-01](../diagramas/secuencias/cu01-solicitar-certificado.puml) (break y respuestas de error).
