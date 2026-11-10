# ADR-002: Estructurar la API como un monolito modular con Clean Architecture

## Estado

Aceptado (2026-03-12)

## Contexto

El equipo es de 4 personas y dispone de 16 semanas (RN-01); domina Python y Angular (RN-04) y debe desplegar en la infraestructura institucional (RT-01). La preocupación PA-01 exige decidir cómo se divide el sistema, y el escenario [EAC-03](../drivers.md#eac-03-mantenibilidad-modificabilidad) exige que agregar un tipo de certificado no toque más de 2 módulos. Un sistema distribuido en varios servicios tendría un costo operativo (despliegue, observabilidad, consistencia) que un equipo de 4 personas no puede sostener.

## Alternativas consideradas

- **A) Microservicios** (solicitudes, pagos, revisión). Escalan y se despliegan por separado; pero multiplican la operación, exigen consistencia eventual entre servicios y superan la capacidad del equipo en 16 semanas.
- **B) Monolito modular con Clean Architecture.** Una sola API con un módulo por caso de uso (Solicitudes, Pagos, Revisión) y capas dominio, aplicación e infraestructura. Simple de desarrollar, probar y desplegar; transacciones locales; el costo es que escala todo o nada.
- **C) Monolito en capas tradicional (controlador, servicio, repositorio).** Menos clases, pero el dominio queda acoplado a la persistencia y a los frameworks, lo que dificulta la prueba y el cambio de adaptadores (SIA, pagos, firma).

## Decisión

Se elige la opción B, porque cumple RN-01 (abarcable por 4 personas en 16 semanas) y favorece EAC-03: la regla de dependencia (dominio no importa nada de aplicacion ni de infraestructura) aísla los cambios. El worker de firma es un proceso separado (ADR-001), pero la lógica del trámite vive en una sola API. Un servicio solo se separa cuando un escenario de escalabilidad medido lo justifique.

## Consecuencias

### Positivas

- Estructura abarcable por el equipo, con un módulo por caso de uso.
- Los casos de uso se prueban con adaptadores en memoria (DIP).
- Cambiar un adaptador (por ejemplo, la pasarela de pagos) no toca el dominio.

### Negativas y costos aceptados

- Más clases y archivos que un monolito tradicional; cada salto agrega indirección.
- La API escala como un todo: en temporada de grados se replica toda la API, no solo el módulo Solicitudes.

### Riesgos asumidos

- Que los módulos se acoplen con el tiempo (ciclos de dependencia). Mitigación: verificar el grafo de imports (por ejemplo, con import-linter) en la validación.

### Vistas que modifica

[Procesos](../vistas/02-procesos.md) (contenedor API monolito modular) y [Desarrollo](../vistas/03-desarrollo.md) (módulos, capas y vista de usos).
