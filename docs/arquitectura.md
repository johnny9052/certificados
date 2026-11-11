# Documento de arquitectura: Sistema de certificados en línea

> Versión 1.0 | Estado: completo hasta SDD (la sección 9 conserva deuda técnica por registrar durante la implementación)

Este documento es el mapa del proyecto. Enlaza a los demás archivos y no los copia: un dato escrito en dos lugares termina diciendo dos cosas distintas. Su estructura es una adaptación de arc42.

## 1. Introducción y objetivos

El sistema permite que un estudiante o egresado de la Universidad de Caldas solicite, pague y descargue certificados académicos firmados digitalmente por un proveedor externo, sin desplazarse. Los cinco objetivos de negocio (OB-01 a OB-05) buscan reducir el tiempo de atención, operar 24/7, incorporar nuevos tipos de certificado sin detener el servicio, atender el acceso móvil y garantizar la autenticidad de cada certificado.

Detalle: [Visión](vision.md)

Funcionalidades: [catálogo de casos de uso](drivers.md#1-casos-de-uso-y-casos-de-uso-primarios) (CU-01 a CU-08), cada uno enlazado a su carpeta de `specs/`.

## 2. Restricciones

Ocho restricciones: infraestructura institucional (RT-01), PostgreSQL (RT-02), proveedor de firma externo (RT-03), SIA solo por API REST (RT-04), equipo de 4 personas y 16 semanas (RN-01), datos dentro del país (RN-02), sin licencias comerciales (RN-03) y equipo que domina Python y Angular (RN-04).

Detalle: [Restricciones](drivers.md#3-restricciones)

## 3. Contexto del sistema

El sistema se relaciona con dos personas (estudiante o egresado, Funcionario de Registro) y cuatro sistemas externos (SIA, pasarela de pagos, proveedor de firma, servicio de correo).

Detalle: [Contexto](contexto.md)

## 4. Estrategia de solución

| Driver | Enfoque | ADR |
|--------|---------|-----|
| EAC-01, EAC-02, RT-03 | Firma asíncrona con cola de mensajes, reintentos y cola de errores | [ADR-001](adr/ADR-001-firma-asincrona.md) |
| PA-01, RN-01, EAC-03 | Monolito modular con Clean Architecture; worker de firma como proceso aparte | [ADR-002](adr/ADR-002-estructura-general.md) |
| RT-02, RN-02, RT-01 | PostgreSQL para el trámite y repositorio institucional para los PDF | [ADR-003](adr/ADR-003-persistencia.md) |
| PA-02, EAC-04 | Autenticación institucional, token de corta vida y autorización por titularidad | [ADR-004](adr/ADR-004-autenticacion-autorizacion.md) |
| PA-04, EAC-04 | Auditoría en tabla de PostgreSQL dentro de la transacción | [ADR-005](adr/ADR-005-registro-trazabilidad.md) |
| PA-03, EAC-02 | Fallas de integración traducidas a errores de aplicación y respuesta HTTP unificada | [ADR-006](adr/ADR-006-manejo-errores.md) |
| PA-05, PA-07, EAC-03, OB-03 | Tipos de certificado como datos y carga inicial por migraciones | [ADR-007](adr/ADR-007-tipos-certificado-configurables.md) |
| PA-06, RT-01, RN-03 | Contenedores Docker sobre el servidor de aplicaciones institucional | [ADR-008](adr/ADR-008-despliegue-contenedores.md) |
| EAC-05, OB-04 | Aplicación web adaptable en Angular | [ADR-009](adr/ADR-009-aplicacion-web-adaptable.md) |

## 5. Vistas (4+1)

[Lógica](vistas/01-logica.md) | [Procesos](vistas/02-procesos.md) | [Desarrollo](vistas/03-desarrollo.md) | [Física](vistas/04-fisica.md)

Escenarios (+1): [diagramas/secuencias/](diagramas/secuencias/) y tabla de escenarios en [drivers.md](drivers.md#6-escenarios-1).

### Matriz de cobertura: drivers y ADR por vista

Ninguna fila vacía: todo driver y todo ADR se ven en al menos una vista.

| Driver o ADR | Contexto (marco) | Lógica | Procesos | Desarrollo | Física |
|--------------|:----------------:|:------:|:--------:|:----------:|:------:|
| CU-01 a CU-03 (casos de uso primarios) | ✓ | ✓ | ✓ | ✓ | |
| EAC-01: respuesta en menos de 3 s | | | ✓ | | |
| EAC-02: ninguna solicitud se pierde | | ✓ | ✓ | | ✓ |
| EAC-03: máximo 2 módulos por cambio | | ✓ | | ✓ | |
| EAC-04: acceso solo del titular | | ✓ | | ✓ | |
| EAC-05: trámite en menos de 2 minutos | | | ✓ | | |
| RT-01: infraestructura institucional | | | | | ✓ |
| RT-02: PostgreSQL | | | ✓ | ✓ | ✓ |
| RT-03: proveedor de firma externo | ✓ | | ✓ | | |
| RT-04: SIA solo por API REST | ✓ | | | ✓ | |
| RN-01: 4 personas, 16 semanas | | | ✓ | ✓ | |
| RN-02: datos dentro del país | | | ✓ | | ✓ |
| ADR-001 Firma asíncrona | | ✓ | ✓ | | ✓ |
| ADR-002 Estructura general | | | ✓ | ✓ | |
| ADR-003 Persistencia | | | ✓ | | ✓ |
| ADR-004 Autenticación y autorización | ✓ | | ✓ | ✓ | |
| ADR-005 Registro y trazabilidad | | ✓ | | ✓ | |
| ADR-006 Manejo de errores | | | | ✓ | |
| ADR-007 Tipos de certificado como datos | | ✓ | | ✓ | |
| ADR-008 Despliegue en contenedores | | | | | ✓ |
| ADR-009 Aplicación web adaptable | | | ✓ | | |

Nota: EAC-02 también se ve en el escenario [CU-03](diagramas/secuencias/cu03-firmar-digitalmente.puml). RN-03 y RN-04 son restricciones de tecnología sin elemento propio en una vista; se verifican en las tecnologías indicadas en los catálogos.

## 6. Conceptos transversales

| Concepto | Cómo se resuelve | Dónde se ve |
|----------|------------------|-------------|
| Autenticación y autorización | Token de corta vida y autorización por titularidad en el caso de uso | [ADR-004](adr/ADR-004-autenticacion-autorizacion.md), [Desarrollo](vistas/03-desarrollo.md) |
| Manejo de errores | Adaptadores traducen fallas a errores de aplicación; respuesta HTTP unificada | [ADR-006](adr/ADR-006-manejo-errores.md) |
| Registro y trazabilidad | Tabla de auditoría e IntentoFirma por cada llamada al proveedor | [ADR-005](adr/ADR-005-registro-trazabilidad.md), [Lógica](vistas/01-logica.md) |
| Configuración | Tipos y valores como datos; secretos por variables de entorno | [ADR-007](adr/ADR-007-tipos-certificado-configurables.md) |
| Entorno de construcción y despliegue | Imágenes Docker para API y worker; Nginx para la aplicación web | [ADR-008](adr/ADR-008-despliegue-contenedores.md), [Física](vistas/04-fisica.md) |
| Estructura general | Monolito modular con Clean Architecture | [ADR-002](adr/ADR-002-estructura-general.md) |
| Migración inicial de datos | Migraciones versionadas con tipos de certificado semilla | [ADR-007](adr/ADR-007-tipos-certificado-configurables.md) |

## 7. Decisiones de diseño

| ADR | Decisión | Estado | Drivers |
|-----|----------|--------|---------|
| [ADR-001](adr/ADR-001-firma-asincrona.md) | Firma asíncrona con cola | Aceptado | EAC-01, EAC-02, RT-03 |
| [ADR-002](adr/ADR-002-estructura-general.md) | Monolito modular con Clean Architecture | Aceptado | RN-01, EAC-03, PA-01 |
| [ADR-003](adr/ADR-003-persistencia.md) | PostgreSQL y repositorio de PDF institucional | Aceptado | RT-02, RN-02, RT-01 |
| [ADR-004](adr/ADR-004-autenticacion-autorizacion.md) | Token de corta vida y autorización por titularidad | Aceptado | EAC-04, PA-02 |
| [ADR-005](adr/ADR-005-registro-trazabilidad.md) | Auditoría en tabla de PostgreSQL | Aceptado | EAC-04, PA-04 |
| [ADR-006](adr/ADR-006-manejo-errores.md) | Errores de aplicación y respuesta unificada | Aceptado | EAC-02, PA-03 |
| [ADR-007](adr/ADR-007-tipos-certificado-configurables.md) | Tipos de certificado como datos | Aceptado | EAC-03, OB-03, PA-05, PA-07 |
| [ADR-008](adr/ADR-008-despliegue-contenedores.md) | Contenedores Docker | Aceptado | RT-01, RN-03, PA-06 |
| [ADR-009](adr/ADR-009-aplicacion-web-adaptable.md) | Aplicación web adaptable | Aceptado | EAC-05, OB-04 |

Los ADR-001 a ADR-003 corresponden al ejemplo visto en clase; los ADR-004 a ADR-009 completan la cobertura de los demás drivers.

## 8. Requisitos de calidad

| EAC | Atributo | Prioridad | ADR | Se verifica en |
|-----|----------|-----------|-----|----------------|
| EAC-01 | Desempeño | Alta | ADR-001 | [specs/001](../specs/001-solicitar-certificado/spec.md), escenario 2 |
| EAC-02 | Fiabilidad | Alta | ADR-001, ADR-006 | [specs/003](../specs/003-firmar-digitalmente/spec.md), escenarios de reintentos y cola de errores |
| EAC-03 | Mantenibilidad | Alta | ADR-002, ADR-007 | Revisión del cambio al agregar un tipo (cuenta de módulos tocados) |
| EAC-04 | Seguridad | Alta | ADR-004, ADR-005 | [specs/001](../specs/001-solicitar-certificado/spec.md), escenario 5; también specs 004 a 008 |
| EAC-05 | Capacidad de interacción | Media | ADR-009 | Prueba de usuario con el 90% en menos de 2 minutos |

Un EAC sin escenario en ninguna spec queda en evidencia: nadie lo está verificando. Las specs 001 a 008 cubren EAC-01, EAC-02 y EAC-04 (una por caso de uso, ver [README](../README.md)); EAC-03 y EAC-05 de verificaciones fuera de las specs.

## 9. Riesgos y deuda técnica

| ID | Riesgo o deuda | Origen | Impacto | Respuesta |
|----|----------------|--------|---------|-----------|
| R-01 | La cola de firma crece sin control y las solicitudes se acumulan | ADR-001 (riesgo asumido) | Retrasos en la entrega de certificados | Alerta al superar 200 elementos |
| R-02 | El proveedor de firma no garantiza disponibilidad continua | RT-03 (restricción) | Solicitudes sin firmar mientras el proveedor esté caído | Cola con reintentos y espera creciente (ADR-001) |
| R-03 | El SIA no responde al verificar el paz y salvo | RT-04 y [vision.md](vision.md) | Solicitudes rechazadas o demoradas | Tiempo máximo de espera y mensaje claro al usuario |
| R-04 | Pico de solicitudes en temporada de grados | [vision.md](vision.md) | Degradación de EAC-01 | Replicar contenedores solo en temporada; medir antes de escalar |
| DT-01 | Las solicitudes que agotan los 5 intentos pasan a la cola de errores y dependen del Funcionario de Registro | Secuencia de la firma: cola de errores | En temporada de grados, el Funcionario de Registro se satura | Se acepta en esta etapa; automatizar el reprocesamiento si la cola de errores crece |
| DT-02 | Archivos PDF huérfanos si el worker falla entre guardar el PDF y actualizar la base | ADR-003 | Inconsistencia entre almacenes | Proceso periódico de conciliación (pendiente) |
| DT-03 | El mecanismo de autenticación institucional es un supuesto (S-03) | ADR-004 | Posible rediseño de la entrada | Validar con el Administrador del SIA antes de implementar |
| DT-04 | El reprocesamiento manual de la cola de errores (CU-08) inicia un nuevo ciclo de hasta 5 intentos, lo que se aparta del 0..5 intentos del modelo de dominio | Spec 008, supuesto S-08 | Registro no acepta el ciclo nuevo | Validar S-08 con Registro; si no se acepta, ADR que reemplace la regla |

## 10. Glosario

| Término | Definición | Nombre en el código |
|---------|------------|---------------------|
| Estudiante | Quien solicita: estudiante o egresado. Solo solicita sus propios certificados | `Estudiante` |
| Solicitud | Un trámite de certificado, con fecha, estado y código de verificación | `Solicitud` |
| Estado de la solicitud | Registrada, pagada, en firma, emitida o rechazada | `EstadoSolicitud` |
| Tipo de certificado | Clase de certificado que se ofrece (nombre, valor, requiere pago) | `TipoCertificado` |
| Pago | El cobro de una solicitud | `Pago` |
| Certificado | El documento emitido y firmado, con la ruta de su PDF | `Certificado` |
| Intento de firma | Cada llamada al proveedor de firma; máximo 5 por solicitud | `IntentoFirma` |
| Código de verificación | Código único que permitirá comprobar la autenticidad de un certificado | `codigo_verificacion` |
| Paz y salvo | Constancia de que el estudiante no tiene deudas con la universidad | `paz_y_salvo` (dato que devuelve `ConsultaAcademica`) |
| Consulta académica | Puerto para consultar notas y paz y salvo en el SIA | `ConsultaAcademica` |
| Funcionario de Registro | Revisa las solicitudes con novedades | `FuncionarioRegistro` |
| Cola de firma | Cola del broker donde esperan las solicitudes de firma; el puerto que publica en ella es `PublicadorFirmas` | cola `firmas` |
| Cola de errores | Cola donde quedan las solicitudes que agotaron los intentos de firma | cola `errores_firma` |
