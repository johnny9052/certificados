# Vista lógica: modelo de dominio

> Las abstracciones clave del sistema de certificados. Diagrama: [01-logica.puml](../diagramas/01-logica.puml). Para usuarios finales y analistas. Notación: UML (C4 no tiene un diagrama para esta vista).

## 1. Presentación principal

Muestra los conceptos del negocio y cómo se relacionan, no las clases del código. Pocas clases, solo atributos clave y sin métodos ni elementos técnicos. Las multiplicidades guardan reglas: 0..5 intentos refleja el máximo de reintentos del ADR-001. Estas abstracciones viven en `dominio/` (entidades de Clean Architecture).

Diagrama: [diagramas/01-logica.puml](../diagramas/01-logica.puml)

## 2. Catálogo de elementos

| Entidad | Qué representa | Atributos clave | Regla del negocio |
|---------|----------------|-----------------|-------------------|
| Estudiante | Quien solicita: estudiante o egresado | documento, programa | Solo solicita sus propios certificados |
| Solicitud | Un trámite de certificado | fecha, estado, código de verificación | Estados: registrada, pagada, en firma, emitida o rechazada |
| TipoCertificado | Clase de certificado que se ofrece | nombre, valor, requiere pago | Es un dato, no código: se agregan tipos sin programar |
| Pago | El cobro de una solicitud | referencia, valor, estado | Una solicitud tiene a lo sumo un pago |
| Certificado | El documento emitido y firmado | fecha de emisión, ruta del PDF firmado | Solo existe si la firma fue exitosa |
| IntentoFirma | Cada llamada al proveedor de firma | número, fecha, resultado | Máximo 5 por solicitud (ADR-001) |

Ciclo de vida de la Solicitud:

| Estado | Significado | Transiciones válidas |
|--------|-------------|----------------------|
| registrada | La API aceptó la solicitud (paz y salvo verificado) | pagada, rechazada |
| pagada | El pago aprobado quedó asociado (se omite si el tipo no requiere pago) | en firma, rechazada |
| en firma | El worker tomó la solicitud y llama al proveedor | emitida, rechazada |
| emitida | El PDF firmado existe y está disponible | (final) |
| rechazada | Sin paz y salvo, pago rechazado o firma agotada tras revisión | (final) |

## 3. Justificación

- CU-01, CU-02 y CU-03: de ellos salen Solicitud, Pago y Certificado.
- EAC-02: IntentoFirma deja rastro de cada intento; ninguna solicitud se pierde sin explicación.
- EAC-03 y ADR-007: TipoCertificado como dato cumple el límite de 2 módulos por cambio.
- Seguridad (EAC-04): toda Solicitud pertenece a un Estudiante.
- ADR-001: el máximo de 5 intentos se expresa en la multiplicidad 0..5.

## 4. Comportamiento

Los diagramas de secuencia cambian el estado de la Solicitud: registrada, pagada, en firma, emitida. Cada vuelta del loop de firma crea un IntentoFirma ([cu03](../diagramas/secuencias/cu03-firmar-digitalmente.puml)).

## 5. Vistas relacionadas

- [Desarrollo](03-desarrollo.md): estas entidades viven en `dominio/` y los módulos las usan.
- [Procesos](02-procesos.md): se guardan en PostgreSQL y el PDF en el repositorio de PDF.

## Diferencia con el diagrama de clases del diseño

El modelo de dominio muestra conceptos del negocio con atributos clave, sin tipos ni métodos, y lo entiende cualquier stakeholder. El diagrama de clases del diseño (se hace al programar) incluye tipos, visibilidad, métodos y los puertos de `aplicacion/`.
