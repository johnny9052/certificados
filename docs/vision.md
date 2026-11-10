# Documento de visión: Sistema de certificados en línea

> **Enunciado.** La Universidad de Caldas emite certificados académicos (de notas, de matrícula, de egresado y similares) mediante un trámite presencial y manual. Se requiere un sistema en línea en el que el estudiante o egresado solicite el certificado, pague el trámite y descargue un PDF firmado digitalmente por un proveedor externo de firma, sin desplazarse a la Universidad.

## 1. Problema u oportunidad

Hoy el estudiante o egresado debe acudir a Registro, hacer fila, pagar por separado y esperar días hábiles a que un funcionario genere, firme y entregue el documento. El funcionario valida a mano, contra el SIA, que el solicitante tenga paz y salvo y que sus notas sean correctas. El proceso no atiende fuera del horario de oficina, no deja trazabilidad confiable de quién solicitó qué y obliga a mantener personal dedicado, sobre todo en temporada de grados. La oportunidad es automatizar la solicitud, el pago, la validación contra el SIA y la firma digital para que el certificado llegue al usuario sin intervención humana, salvo en los casos con novedades.

## 2. Objetivos de negocio

| ID | Objetivo de negocio (qué gana la Universidad, medible) | Atributo de calidad que implica |
|----|--------------------------------------------------------|---------------------------------|
| **OB-01** | Reducir en 70% el tiempo de atención de solicitudes de certificados respecto del trámite presencial, sin que se pierdan solicitudes, medido en el primer periodo de operación. | Desempeño (comportamiento temporal) y fiabilidad |
| **OB-02** | Atender solicitudes de certificados 24/7 sin personal dedicado a la recepción, con una disponibilidad del 99,5% mensual. | Fiabilidad (disponibilidad y tolerancia a fallos) |
| **OB-03** | Poder incorporar nuevos tipos de certificado o trámites sin detener el servicio. | Mantenibilidad (modificabilidad) |
| **OB-04** | Atender al 40% de egresados que no acceden desde computador, con una experiencia que permita solicitar un certificado en menos de 2 minutos sin ayuda. | Capacidad de interacción (operabilidad) |
| **OB-05** | Garantizar que cualquier tercero pueda comprobar la autenticidad de un certificado emitido, con una firma digital en el 100% de los certificados. | Seguridad (no repudio, autenticidad) |

## 3. Alcance

### Qué SÍ incluye

- Solicitud de certificados por el estudiante o egresado autenticado.
- Verificación de paz y salvo y datos académicos consultando el SIA.
- Pago del trámite a través de una pasarela de pagos externa.
- Generación del PDF y firma digital mediante un proveedor externo de firma.
- Notificación al solicitante por correo cuando el certificado está listo.
- Descarga del certificado firmado.
- Revisión por el Funcionario de Registro de las solicitudes con novedades.
- Código único de verificación en cada certificado.

### Qué NO incluye

- Gestión de notas, matrícula o paz y salvo (siguen en el SIA).
- Desarrollo de la pasarela de pagos ni del proveedor de firma (son servicios externos).
- Página pública de verificación de autenticidad (queda para una fase posterior; solo se genera el código).
- Aplicación móvil nativa (se atiende por la aplicación web adaptable).
- Reportes institucionales avanzados más allá del reporte mensual básico.
- Migración de certificados históricos emitidos en papel.

## 4. Stakeholders

| Nombre / rol | Tipo | Preocupaciones | Poder / interés |
|--------------|------|----------------|-----------------|
| Patrocinador (Vicerrectoría Académica) | Negocio | Costo, plazo, retorno de la inversión | Alto poder, alto interés |
| Estudiante o egresado | Negocio | Rapidez y facilidad de uso, desde cualquier dispositivo | Bajo poder, alto interés |
| Funcionario de Registro | Negocio | Revisar solo los casos con novedades, no repetir trabajo | Bajo poder, alto interés |
| Área jurídica | Negocio | Cumplimiento de la Ley 1581 de 2012 y validez de la firma | Alto poder, bajo interés |
| Desarrollador (equipo del proyecto) | Técnico | Cambiar el sistema sin romperlo, poder probarlo | Bajo poder, alto interés |
| Área de Sistemas (administrador de infraestructura) | Técnico | Desplegar y monitorear en la infraestructura institucional | Alto poder, alto interés |
| Administrador del SIA (también responsable de seguridad de la información) | Técnico | Integración sin fricción, protección de datos y accesos | Alto poder, alto interés |
| Área de soporte | Técnico | Diagnosticar rápido dónde se detuvo una solicitud | Bajo poder, alto interés |

## 5. Restricciones

| ID | Restricción | Quién la impone | Por qué |
|----|-------------|-----------------|---------|
| **RT-01** | Desplegar en la infraestructura institucional de la Universidad | Área de Sistemas | Política institucional de hospedaje y operación |
| **RT-02** | Usar PostgreSQL como motor de base de datos | Área de Sistemas (estándar institucional) | Es el motor que la Universidad ya opera y respalda |
| **RT-03** | La firma digital la realiza un proveedor externo que responde entre 2 y 8 segundos y no garantiza disponibilidad continua | Contrato con el proveedor | La Universidad no controla ese servicio |
| **RT-04** | Consultar los datos académicos solo mediante la API REST actual del SIA, sin conexión directa a su base de datos | Administrador del SIA | Seguridad y estabilidad del sistema académico |
| **RN-01** | Equipo de 4 personas y 16 semanas | El curso | Alcance y calendario del proyecto académico |
| **RN-02** | Los datos personales no pueden salir del país | Ley 1581 de 2012 y Área jurídica | Cumplimiento normativo |
| **RN-03** | No se usan licencias comerciales | Patrocinador | El presupuesto no las contempla |
| **RN-04** | El equipo domina Python y Angular | El curso | Evita curva de aprendizaje inviable en 16 semanas |

Nota: RT-01, RT-02, RT-03, RN-01 y RN-02 son las restricciones usadas en las demás presentaciones del curso; RT-04, RN-03 y RN-04 formalizan restricciones que la visión menciona sin identificador.

## 6. Supuestos y riesgos iniciales

### Supuestos

| ID | Lo que el enunciado no dice | Supuesto adoptado | Validar con |
|----|-----------------------------|-------------------|-------------|
| S-01 | Cuántos usuarios concurrentes habrá | Hasta 100 usuarios concurrentes en operación normal | Registro Académico |
| S-02 | Si todos los certificados requieren pago | El valor y si requiere pago dependen del tipo de certificado (dato configurable) | Patrocinador |
| S-03 | Cómo se autentica el usuario | Mediante el mecanismo de autenticación institucional existente | Administrador del SIA |
| S-04 | Qué ocurre con una solicitud sin paz y salvo | Se rechaza y se indica la dependencia pendiente | Registro Académico |
| S-05 | Qué tipos de certificado existen al inicio | Certificado de notas, de matrícula y de egresado | Registro Académico |
| S-06 | Cómo se paga | La pasarela de pagos ofrece una URL de pago y notifica el resultado por HTTPS | Área de Sistemas |
| S-07 | Cuánto tiempo se conservan los PDF | Se conservan en almacenamiento institucional según la política de retención vigente | Área jurídica |
| S-08 | Qué pasa al reprocesar manualmente una solicitud de la cola de errores | Se inicia un nuevo ciclo de hasta 5 intentos de firma (complemento) | Funcionario de Registro |

### Riesgos

| ID | Riesgo inicial | Objetivo afectado | Mitigación |
|----|----------------|-------------------|------------|
| R-02 | El proveedor de firma tarda más de lo esperado o no está disponible | OB-01, OB-02 | Procesamiento asíncrono con reintentos (ver ADR-001) |
| R-03 | El SIA no responde cuando se verifica el paz y salvo | OB-02 | Tiempo máximo de espera y mensaje claro al usuario; validar con el Administrador del SIA |
| R-04 | Pico de solicitudes en temporada de grados | OB-01, OB-02 | Capacidad adicional solo en temporada; medir antes de escalar |
| R-05 | El equipo no alcanza a integrar los tres sistemas externos en 16 semanas | OB-01 | Alcance reducido y adaptadores simulados en las primeras entregas |

Nota: el riesgo R-01 (crecimiento de la cola de firma) nace de una decisión de diseño y se registra en `arquitectura.md` y en el ADR-001.
