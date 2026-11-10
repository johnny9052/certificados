# Vista física: diagrama de despliegue

> Dónde se ejecuta cada contenedor. Diagrama: [04-fisica.puml](../diagramas/04-fisica.puml). Para ingenieros de infraestructura.

## 1. Presentación principal

Diagrama de despliegue en el centro de datos de la Universidad de Caldas (RT-01): servidor web, servidor de aplicaciones, servidor de mensajería, servidor de base de datos y almacenamiento institucional, con los sistemas externos y sus protocolos.

Diagrama: [diagramas/04-fisica.puml](../diagramas/04-fisica.puml)

## 2. Catálogo de elementos

| Nodo | Contenedores que aloja | Características | Conexiones |
|------|------------------------|-----------------|------------|
| Servidor web [Ubuntu, Nginx] | Aplicación web | Sirve los archivos estáticos de la interfaz | Navegadores de estudiantes y funcionarios (HTTPS) |
| Servidor de aplicaciones [Docker] | API de certificados, Worker de firma | Cada contenedor se reinicia y escala por separado | Pasarela y proveedor de firma (HTTPS), correo (SMTP), SIA (HTTPS) |
| Servidor de mensajería | Broker de mensajes | Colas persistentes: los mensajes sobreviven a un reinicio | API y Worker (AMQP, red interna) |
| Servidor de base de datos | PostgreSQL | Solo accesible desde la red interna | API y Worker (SQL) |
| Almacenamiento institucional | Repositorio de PDF | Ubicado en el país, dentro del centro de datos | Worker (escribe) y API (lee) |

Todos los nodos están en el centro de datos de la Universidad de Caldas.

## 3. Justificación

- RT-01: todos los nodos están en la infraestructura institucional; no hay nube pública.
- RN-02 (Ley 1581): los datos personales y los PDF no salen del país.
- ADR-001 y EAC-02: broker con colas persistentes en un nodo propio; si el proveedor cae, nada se pierde.
- RT-02 y ADR-003: PostgreSQL institucional y almacenamiento separado para los PDF.
- ADR-008: API y worker en contenedores Docker; aplicación web servida por Nginx.
- R-04: la capacidad adicional en temporada de grados se obtiene replicando los contenedores de API y worker.

## 4. Comportamiento

La secuencia de CU-01 cruza nodos: API y broker por la red interna; worker y proveedor por internet ([cu03-firmar-digitalmente.puml](../diagramas/secuencias/cu03-firmar-digitalmente.puml)).

## 5. Vistas relacionadas

- [Procesos](02-procesos.md): cada contenedor de esa vista aparece en un nodo de esta.
- [Contexto](../contexto.md): cada sistema externo aparece con su protocolo.
