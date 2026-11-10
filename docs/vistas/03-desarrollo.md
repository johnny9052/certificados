# Vista de desarrollo: diagrama de componentes

> Nivel 3 de C4: se abre la aplicación y se hace zoom, sus bloques internos y responsabilidades. Diagrama: [03-desarrollo.puml](../diagramas/03-desarrollo.puml). Para programadores. Incluye las capas de Clean Architecture y la vista de usos sin ciclos.

## 1. Presentación principal

Diagrama de componentes del contenedor API de certificados (monolito modular, ADR-002): controlador REST, módulos Solicitudes, Pagos y Revisión, y los adaptadores que implementan los puertos.

Diagrama: [diagramas/03-desarrollo.puml](../diagramas/03-desarrollo.puml)

### Vista de usos (quién necesita a quién)

```text
Módulo Revisión ──usa──> dominio
Módulo Solicitudes ──usa──> dominio
Módulo Pagos ──usa──> dominio
Módulo Solicitudes ──usa (verifica el pago)──> Módulo Pagos
Módulo Revisión ──usa (cambia el estado)──> Módulo Solicitudes
Prohibido: cualquier ciclo (por ejemplo, Pagos usando Solicitudes)
```

- Incrementos: dominio y Pagos pueden entregarse antes que Revisión.
- Impacto: un cambio en Pagos obliga a revisar Solicitudes y, por cadena, Revisión.
- Los ciclos están prohibidos: son deuda técnica arquitectónica.

### Vista de capas (solo hacia adentro)

| Capa | Contenido | Puede usar |
|------|-----------|------------|
| infraestructura | Controlador REST, Adaptador PostgreSQL, Publicador, Adaptador SIA, Adaptador de pagos, Adaptador de bitácora, Validador de token | aplicacion y dominio (implementa los puertos) |
| aplicacion | Casos de uso (Solicitudes, Pagos, Revisión) y puertos de salida | dominio |
| dominio | Entidades y reglas: Solicitud, Pago, Certificado, IntentoFirma, TipoCertificado | nada externo |

`main.py` (composición) conecta las capas: es lo único que las conoce todas. Si `dominio/` importa algo de `infraestructura/`, la arquitectura está rota.

## 2. Catálogo de elementos

| Componente | Tipo | Responsabilidad | Depende de |
|------------|------|-----------------|------------|
| Controlador REST | Adaptador de entrada | Recibe las peticiones HTTP y las traduce a casos de uso; convierte errores en respuestas unificadas (ADR-006) | Módulos de casos de uso, Validador de token |
| Validador de token | Adaptador de entrada | Valida el token del usuario y entrega su identidad (ADR-004) | Identidad institucional |
| Módulo Solicitudes | Caso de uso | CU-01: valida, registra y publica la solicitud de firma | Puertos RepositorioCertificados, ConsultaAcademica, PublicadorFirmas y Bitácora; Módulo Pagos (verifica el pago) |
| Módulo Pagos | Caso de uso | CU-02: inicia y confirma el pago del trámite | Puertos RepositorioCertificados y PasarelaPagos |
| Módulo Revisión | Caso de uso | CU-08: gestión de novedades por el Funcionario de Registro | Puerto RepositorioCertificados |
| Adaptador SIA | Adaptador | Implementa ConsultaAcademica contra la API del SIA | SIA (HTTPS) |
| Publicador | Adaptador | Implementa PublicadorFirmas sobre el broker | Broker de mensajes (AMQP) |
| Adaptador de pagos | Adaptador | Implementa PasarelaPagos | Pasarela de pagos (HTTPS) |
| Adaptador PostgreSQL | Adaptador | Implementa RepositorioCertificados | PostgreSQL (SQL) |
| Adaptador de bitácora | Adaptador | Implementa el puerto Bitácora escribiendo en la tabla de auditoría (ADR-005) | PostgreSQL (SQL) |

Nota: Validador de token y Adaptador de bitácora extienden el ejemplo visto en clase para cubrir ADR-004 y ADR-005.

## 3. Justificación

- ADR-002 y RN-01: monolito modular, un módulo por caso de uso, abarcable por 4 personas en 16 semanas.
- EAC-03 (mantenibilidad): un tipo de certificado nuevo no toca más de 2 módulos (ADR-007).
- Regla de dependencia: los casos de uso definen los puertos; los adaptadores los implementan.
- RT-02: PostgreSQL queda aislado en un solo adaptador (ADR-003).
- RT-04: el SIA queda aislado en un solo adaptador.
- ADR-004, ADR-005 y ADR-006: seguridad, auditoría y errores resueltos en adaptadores.

## 4. Comportamiento

CU-01 dentro de la API: controlador, Módulo Solicitudes, Adaptador SIA y Publicador ([cu01-solicitar-certificado.puml](../diagramas/secuencias/cu01-solicitar-certificado.puml)).

## 5. Vistas relacionadas

- [Procesos](02-procesos.md): es el interior del contenedor API de certificados.
- [Lógica](01-logica.md): los módulos operan sobre las entidades del modelo de dominio.
- Módulos: coincide con las capas descritas arriba.
