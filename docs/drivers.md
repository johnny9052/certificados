# Drivers arquitectónicos: Sistema de certificados en línea

> **Enunciado.** Partiendo de [vision.md](vision.md), se extraen los drivers que moldean la arquitectura: casos de uso primarios, escenarios de atributos de calidad, restricciones y preocupaciones arquitectónicas, más la matriz de priorización y la tabla de escenarios (+1).

## 1. Casos de uso y casos de uso primarios

| ID | Caso de uso | Actor | ¿Primario? | Justificación |
|----|-------------|-------|------------|---------------|
| **CU-01** | Solicitar certificado | Estudiante o egresado | Sí | Importancia para el negocio (razón de ser del sistema) y alta frecuencia; ligado a EAC-01 |
| **CU-02** | Pagar el trámite | Estudiante o egresado | Sí | Complejidad e integración externa (pasarela de pagos) con riesgo técnico |
| **CU-03** | Firmar digitalmente | Sistema (worker de firma) | Sí | Complejidad y dependencia de un proveedor externo lento e inestable (RT-03); ligado a EAC-02 |
| **CU-04** | Consultar estado de la solicitud | Estudiante o egresado | No | Simple; consulta datos que ya están en el sistema |
| **CU-05** | Actualizar perfil | Estudiante o egresado | No | CRUD estándar |
| **CU-06** | Generar reporte mensual | Funcionario de Registro | No | Importante para el negocio, pero no estructura nada: opera sobre datos ya estructurados |
| **CU-07** | Descargar certificado firmado | Estudiante o egresado | No | Lectura de un archivo ya almacenado; la seguridad se cubre con EAC-04 |
| **CU-08** | Revisar solicitudes con novedades | Funcionario de Registro | No | Flujo de gestión sin complejidad técnica propia |

Nota: CU-01 a CU-06 son los casos del ejemplo visto en clase; CU-07 y CU-08 completan los flujos de la descarga y del Funcionario de Registro que las vistas ya mencionan.

## 2. Escenarios de atributos de calidad

### EAC-01: Desempeño (comportamiento temporal)

| Parte | Pregunta | Contenido |
|-------|----------|-----------|
| **1. Fuente del estímulo** | ¿Quién o qué lo provoca? | Un egresado autenticado |
| **2. Estímulo** | ¿Qué ocurre? | Solicita la generación de un certificado |
| **3. Artefacto** | ¿Sobre qué parte? | El módulo de generación de documentos (API de certificados) |
| **4. Entorno** | ¿En qué condiciones? | Operación normal, hasta 100 usuarios concurrentes (S-01) |
| **5. Respuesta** | ¿Qué debe hacer el sistema? | Registra la solicitud, la deja en cola de firma y responde al usuario sin esperar la firma |
| **6. Medida de respuesta** | ¿Cómo se verifica? | **Respuesta al usuario en menos de 3 segundos en el 95% de los casos** |

**Escenario redactado.** Un egresado autenticado solicita la generación de un certificado al módulo de generación de documentos, en operación normal con hasta 100 usuarios concurrentes. El sistema registra la solicitud y responde en menos de 3 segundos en el 95% de los casos.

**Traza:** OB-01, CU-01, RT-03

### EAC-02: Fiabilidad (tolerancia a fallos)

| Parte | Pregunta | Contenido |
|-------|----------|-----------|
| **1. Fuente del estímulo** | ¿Quién o qué lo provoca? | El proveedor de firma digital |
| **2. Estímulo** | ¿Qué ocurre? | No responde en 10 segundos, o responde con error, cuando se pide firmar un certificado |
| **3. Artefacto** | ¿Sobre qué parte? | El componente de firma (worker) usado por CU-03 |
| **4. Entorno** | ¿En qué condiciones? | Operación normal, incluso en el pico de temporada de grados con hasta 100 usuarios concurrentes |
| **5. Respuesta** | ¿Qué debe hacer el sistema? | Conserva la solicitud, la reencola con espera creciente hasta 5 intentos y, si se agotan, la deja en la cola de errores para el Funcionario de Registro |
| **6. Medida de respuesta** | ¿Cómo se verifica? | **0 solicitudes perdidas; el 100% de las solicitudes terminan emitidas o en la cola de errores; disponibilidad del 99,5% mensual para recibir solicitudes** |

**Escenario redactado.** Cuando el proveedor de firma no responde en 10 segundos durante la firma de un certificado, el sistema conserva la solicitud y reintenta hasta 5 veces con espera creciente. Si se agotan los intentos, la solicitud pasa a la cola de errores. Ninguna solicitud se pierde y el sistema sigue recibiendo solicitudes el 99,5% del mes.

**Traza:** OB-01, OB-02, RT-03, R-02, CU-03

### EAC-03: Mantenibilidad (modificabilidad)

| Parte | Pregunta | Contenido |
|-------|----------|-----------|
| **1. Fuente del estímulo** | ¿Quién o qué lo provoca? | Un desarrollador del equipo |
| **2. Estímulo** | ¿Qué ocurre? | Debe agregar un nuevo tipo de certificado |
| **3. Artefacto** | ¿Sobre qué parte? | La API de certificados (módulos de casos de uso y dominio) |
| **4. Entorno** | ¿En qué condiciones? | En desarrollo, con el sistema en producción sin interrumpirse |
| **5. Respuesta** | ¿Qué debe hacer el sistema? | Admite el nuevo tipo sin modificar la lógica de los tipos existentes |
| **6. Medida de respuesta** | ¿Cómo se verifica? | **Cambio que toca 2 módulos o menos, en máximo 2 días por persona, sin detener el servicio** |

**Escenario redactado.** Un desarrollador agrega un nuevo tipo de certificado a la API en desarrollo. El cambio toca no más de 2 módulos, toma máximo 2 días por persona y se despliega sin detener el servicio.

**Traza:** OB-03, RN-01, CU-01

### EAC-04: Seguridad (confidencialidad y autenticidad)

| Parte | Pregunta | Contenido |
|-------|----------|-----------|
| **1. Fuente del estímulo** | ¿Quién o qué lo provoca? | Un usuario autenticado |
| **2. Estímulo** | ¿Qué ocurre? | Intenta consultar o descargar el certificado de otra persona |
| **3. Artefacto** | ¿Sobre qué parte? | La API de certificados y el repositorio de PDF |
| **4. Entorno** | ¿En qué condiciones? | Operación normal |
| **5. Respuesta** | ¿Qué debe hacer el sistema? | Rechaza la petición y registra el intento en la bitácora de auditoría |
| **6. Medida de respuesta** | ¿Cómo se verifica? | **El 100% de los intentos de acceso ajeno se rechaza y queda registrado con usuario y hora** |

**Escenario redactado.** Un usuario autenticado intenta descargar el certificado de otra persona. La API rechaza la petición y registra el intento con usuario y hora. Esto ocurre en el 100% de los intentos.

**Traza:** OB-05, RN-02, CU-07

### EAC-05: Capacidad de interacción (operabilidad)

| Parte | Pregunta | Contenido |
|-------|----------|-----------|
| **1. Fuente del estímulo** | ¿Quién o qué lo provoca? | Un egresado que accede desde un teléfono móvil |
| **2. Estímulo** | ¿Qué ocurre? | Solicita su primer certificado sin ayuda |
| **3. Artefacto** | ¿Sobre qué parte? | La aplicación web |
| **4. Entorno** | ¿En qué condiciones? | Operación normal, pantalla pequeña |
| **5. Respuesta** | ¿Qué debe hacer el sistema? | Guía el trámite completo hasta la confirmación de la solicitud |
| **6. Medida de respuesta** | ¿Cómo se verifica? | **Trámite completado en menos de 2 minutos por el 90% de los usuarios de la prueba** |

**Escenario redactado.** Un egresado que accede desde su teléfono solicita su primer certificado sin ayuda. La aplicación web lo guía hasta la confirmación y el 90% de los usuarios de prueba lo logra en menos de 2 minutos.

**Traza:** OB-04, CU-01

## 3. Restricciones

| ID | Restricción | Tipo | Origen | Impacto arquitectónico esperado |
|----|-------------|------|--------|---------------------------------|
| **RT-01** | Desplegar en la infraestructura institucional | Técnica | Área de Sistemas | Descarta soluciones serverless en nube pública |
| **RT-02** | Usar PostgreSQL | Técnica | Estándar institucional | Descarta el modelo documental; obliga a un modelo relacional |
| **RT-03** | Firma digital mediante proveedor externo (responde entre 2 y 8 s, sin disponibilidad garantizada) | Técnica | Contrato con el proveedor | Obliga a aislar la integración y a manejar sus fallas (EAC-01, EAC-02) |
| **RT-04** | Consultar el SIA solo por su API REST | Técnica | Administrador del SIA | Obliga a un componente de integración aislado (puerto y adaptador) |
| **RN-01** | Equipo de 4 personas, 16 semanas | Negocio | El curso | Descarta microservicios por costo operativo |
| **RN-02** | Datos personales no salen del país | Negocio | Ley 1581 de 2012 | Restringe la ubicación del despliegue y del almacenamiento |
| **RN-03** | Sin licencias comerciales | Negocio | Patrocinador | Obliga a tecnologías de código abierto |
| **RN-04** | El equipo domina Python y Angular | Negocio | El curso | Orienta la elección de tecnologías del backend y del frontend |

## 4. Preocupaciones arquitectónicas

| ID | Preocupación | Qué hay que decidir | Por qué importa en este sistema |
|----|--------------|---------------------|---------------------------------|
| **PA-01** | Estructura general | Cómo se divide el sistema y en cuántos procesos | 4 personas y 16 semanas; debe permitir agregar tipos de certificado (EAC-03) |
| **PA-02** | Autenticación y autorización | Cómo se identifica al usuario y se protege cada certificado | Solo el titular accede a su certificado (EAC-04) |
| **PA-03** | Manejo de errores y excepciones | Estrategia unificada ante fallas del SIA, pagos y firma | Ninguna solicitud se pierde sin explicación (EAC-02) |
| **PA-04** | Registro y trazabilidad | Qué se registra, dónde y por cuánto tiempo | Auditoría de emisiones y de accesos; soporte (EAC-04) |
| **PA-05** | Configuración | Cómo se parametrizan tipos de certificado, valores y reintentos sin recompilar | Nuevos tipos sin detener el servicio (OB-03) |
| **PA-06** | Entorno de desarrollo, construcción y despliegue | Cómo se empaqueta y se despliega en la infraestructura institucional | RT-01 y repetibilidad con 4 personas |
| **PA-07** | Migración inicial de datos | Cómo se cargan los tipos de certificado y sus valores iniciales | El sistema debe arrancar con su catálogo de trámites |

## 5. Matriz de priorización

### Alta importancia, alta dificultad: atender primero

CU-01, CU-02, CU-03, EAC-01, EAC-02, RT-03, PA-01

### Alta importancia, baja dificultad: hacer ya

EAC-03, EAC-04, RT-01, RT-02, RT-04, RN-01, RN-02, RN-03, RN-04, PA-02, PA-05, PA-06

### Baja importancia, baja dificultad: hacer si sobra tiempo

CU-04, CU-05, CU-07, CU-08, EAC-05, PA-03, PA-04, PA-07

### Baja importancia, alta dificultad: cuestionar

CU-06

## 6. Escenarios (+1)

Inventario de escenarios que se diagraman. Un escenario sin caso de uso primario o EAC detrás no se diagrama.

| ID | Escenario | Origen | Por qué es escenario | Archivo en secuencias/ |
|----|-----------|--------|----------------------|------------------------|
| **ESC-01** | Solicitar certificado | CU-01 | Caso de uso primario. Muestra que el estudiante recibe respuesta en menos de 3 s: cubre EAC-01 | [cu01-solicitar-certificado.puml](diagramas/secuencias/cu01-solicitar-certificado.puml) |
| **ESC-02** | Pagar el trámite | CU-02 | Caso de uso primario con integración a la pasarela de pagos (riesgo técnico) | [cu02-pagar-tramite.puml](diagramas/secuencias/cu02-pagar-tramite.puml) |
| **ESC-03** | Firmar digitalmente | CU-03 | Caso de uso primario. Proveedor externo (RT-03, ADR-001): cubre EAC-02 | [cu03-firmar-digitalmente.puml](diagramas/secuencias/cu03-firmar-digitalmente.puml) |

Los dos EAC de desempeño y fiabilidad quedan dentro de los tres casos primarios. EAC-03 (modificabilidad), EAC-04 (seguridad) y EAC-05 (interacción) se verifican con una revisión de cambios, con la autorización de la API y con pruebas de usuario respectivamente, por lo que no requieren diagrama de secuencia propio.
