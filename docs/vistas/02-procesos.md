# Vista de procesos: diagrama de contenedores

> Nivel 2 de C4: se abre la caja y se hace zoom, qué se ejecuta y cómo se comunica. Diagrama: [02-procesos.puml](../diagramas/02-procesos.puml). Para integradores del sistema. En este contexto un contenedor no es Docker: es cualquier cosa que se ejecuta o guarda datos por separado.

## 1. Presentación principal

Diagrama de contenedores del Sistema de certificados: aplicación web, API de certificados, broker de mensajes, worker de firma, PostgreSQL y repositorio de PDF, con los sistemas externos que cada uno consume.

Diagrama: [diagramas/02-procesos.puml](../diagramas/02-procesos.puml)

## 2. Catálogo de elementos

| Elemento | Responsabilidad | Tecnología | Se comunica con |
|----------|-----------------|------------|-----------------|
| Aplicación web | Interfaz del estudiante y del Funcionario de Registro | Angular | API de certificados (JSON / HTTPS) |
| API de certificados | Reglas del trámite: solicitar, pagar y revisar (monolito modular) | Python | Aplicación web, PostgreSQL, Broker, SIA y Pasarela de pagos |
| Broker de mensajes | Guarda las solicitudes de firma hasta que el worker las procese | RabbitMQ | API (publica) y Worker (entrega) |
| Worker de firma | Firma el PDF, reintenta hasta 5 veces y notifica | Python | Broker, Proveedor de firma, Servicio de correo, Repositorio de PDF |
| PostgreSQL | Solicitudes, pagos, intentos de firma, tipos de certificado y estados | PostgreSQL | API (SQL) y Worker (SQL) |
| Repositorio de PDF | Certificados firmados, listos para descarga | Almacenamiento institucional | Worker (escribe) y API (lee) |

## 3. Justificación

- EAC-01 y EAC-02: broker y worker separados de la API (ADR-001).
- RN-01: una sola API como monolito modular (ADR-002).
- RT-02 y RN-02: PostgreSQL y repositorio institucional (ADR-003).
- RT-03: solo el worker habla con el proveedor de firma.
- OB-04 y EAC-05: la aplicación web es adaptable a pantallas pequeñas (ADR-009).
- ADR-004: la API valida el token del usuario en cada petición.

## 4. Comportamiento

- [cu01-solicitar-certificado.puml](../diagramas/secuencias/cu01-solicitar-certificado.puml): la API responde antes de que termine la firma.
- [cu02-pagar-tramite.puml](../diagramas/secuencias/cu02-pagar-tramite.puml): integración con la pasarela.
- [cu03-firmar-digitalmente.puml](../diagramas/secuencias/cu03-firmar-digitalmente.puml): reintentos y fallas con alt y loop.

## 5. Vistas relacionadas

- [Contexto](../contexto.md): de dónde vienen actores y sistemas externos.
- [Física](04-fisica.md): en qué servidor corre cada contenedor.
- [Desarrollo](03-desarrollo.md): cómo se organiza por dentro la API.
