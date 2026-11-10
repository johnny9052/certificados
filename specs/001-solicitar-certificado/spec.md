# language: es
# Traza: CU-01, EAC-01, EAC-04, RT-04, ADR-001, ADR-004, ADR-006

Característica: Solicitar certificado
  Como estudiante o egresado autenticado
  quiero solicitar un certificado académico en línea
  para evitar el trámite presencial y recibir mi certificado firmado sin desplazarme.

  Escenario: Solicitud aceptada
    Dado un estudiante autenticado con paz y salvo
    Y un tipo de certificado "notas" cuyo pago ya está aprobado
    Cuando solicita un certificado de notas
    Entonces la solicitud queda registrada
    Y el estudiante recibe el código de verificación
    Y la solicitud queda en la cola de firma

  Escenario: Respuesta inmediata (EAC-01)
    Dado el sistema con 100 usuarios concurrentes
    Cuando un estudiante solicita un certificado
    Entonces recibe respuesta en menos de 3 s en el 95 % de los casos
    Y la respuesta no espera la firma digital (ADR-001)

  Escenario: Estudiante sin paz y salvo
    Dado un estudiante con una deuda en biblioteca
    Cuando solicita un certificado
    Entonces la solicitud se rechaza
    Y se le indica la dependencia pendiente
    Y no se publica nada en la cola de firma

  Escenario: Tipo de certificado con pago sin pago aprobado
    Dado un estudiante autenticado con paz y salvo
    Y un tipo de certificado que requiere pago, sin pago aprobado
    Cuando solicita ese certificado
    Entonces la solicitud se rechaza
    Y se le indica que debe pagar el trámite
    Y no se publica nada en la cola de firma

  Escenario: Solicitud a nombre de otra persona (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta solicitar un certificado a nombre de otro estudiante
    Entonces la solicitud se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
    Y esto ocurre en el 100 % de los intentos

  Escenario: El SIA no responde
    Dado un estudiante autenticado
    Y el SIA no responde en el tiempo máximo de espera
    Cuando solicita un certificado
    Entonces la solicitud no se acepta
    Y se le informa que la verificación académica no está disponible y que puede intentarlo más tarde
    Y no se publica nada en la cola de firma
