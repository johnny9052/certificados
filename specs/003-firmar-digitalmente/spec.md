# language: es
# Traza: CU-03, EAC-02, RT-03, ADR-001, ADR-003, ADR-005, ADR-006

Característica: Firmar digitalmente
  Como sistema (worker de firma)
  quiero firmar los certificados solicitados con el proveedor de firma, reintentando ante fallas
  para entregar certificados firmados sin perder ninguna solicitud.

  Escenario: Firma exitosa
    Dado una solicitud en la cola de firma
    Y un proveedor de firma que responde en menos de 10 s
    Cuando el worker procesa la solicitud
    Entonces se guarda el PDF firmado en el repositorio de PDF
    Y se registra un intento de firma exitoso
    Y la solicitud queda en estado emitida
    Y se notifica al estudiante que su certificado está listo

  Escenario: El proveedor no responde (EAC-02)
    Dado una solicitud en la cola de firma
    Y un proveedor de firma que no responde en 10 s
    Cuando el worker procesa la solicitud
    Entonces se registra un intento de firma fallido
    Y la solicitud se reencola con espera creciente
    Y la solicitud no se pierde

  Escenario: Espera creciente entre intentos
    Dado una solicitud cuyo proveedor falla repetidamente
    Cuando se producen intentos fallidos consecutivos
    Entonces las esperas entre intentos son de 2, 4, 8 y 16 segundos

  Escenario: Reintentos agotados (EAC-02)
    Dado una solicitud con 5 intentos de firma fallidos
    Cuando se agota el último intento
    Entonces la solicitud se envía a la cola de errores
    Y no se realiza un sexto intento
    Y la solicitud queda disponible para revisión del Funcionario de Registro

  Escenario: Estudiante sin autorización de correo
    Dado una solicitud firmada de un estudiante que no autorizó el correo
    Cuando el worker termina de firmar
    Entonces no se envía ninguna notificación por correo
    Y el certificado queda emitido y disponible para descarga

  Escenario: Falla el envío del correo
    Dado una solicitud firmada de un estudiante que autorizó el correo
    Y un servicio de correo que falla
    Cuando el worker intenta notificar
    Entonces el certificado queda emitido de todas formas
    Y la falla del correo se registra en la bitácora

  Escenario: Mensaje duplicado
    Dado una solicitud que ya está en estado emitida
    Cuando el worker recibe de nuevo su mensaje
    Entonces el mensaje se descarta sin llamar al proveedor de firma
