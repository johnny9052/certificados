# language: es
# Traza: CU-04, EAC-04, ADR-004, ADR-005

Característica: Consultar estado de la solicitud
  Como estudiante o egresado autenticado
  quiero consultar el estado de mis solicitudes de certificado
  para saber si ya fue firmada y puedo descargarla.

  Escenario: Consulta del estado de una solicitud propia
    Dado un estudiante autenticado con una solicitud en estado en firma
    Cuando consulta el estado de esa solicitud
    Entonces ve el estado "en firma"
    Y ve el tipo de certificado y la fecha de la solicitud
    Y ve el código de verificación

  Escenario: Solicitud emitida
    Dado un estudiante autenticado con una solicitud emitida
    Cuando consulta el estado de esa solicitud
    Entonces ve el estado "emitida"
    Y se le indica que puede descargar el certificado

  Escenario: Listado de solicitudes propias
    Dado un estudiante autenticado con tres solicitudes
    Cuando consulta sus solicitudes
    Entonces ve las tres, de la más reciente a la más antigua

  Escenario: Estudiante sin solicitudes
    Dado un estudiante autenticado sin solicitudes
    Cuando consulta sus solicitudes
    Entonces ve una lista vacía y no un error

  Escenario: Solicitud de otra persona (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta consultar la solicitud de otro estudiante
    Entonces la consulta se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
