# language: es
# Traza: CU-08, EAC-02, EAC-04, DT-01, ADR-001, ADR-004, ADR-005

Característica: Revisar solicitudes con novedades
  Como Funcionario de Registro
  quiero revisar las solicitudes con novedades y resolverlas
  para que ningún trámite quede detenido sin respuesta.

  Escenario: Listar las solicitudes con novedades
    Dado un Funcionario de Registro autenticado
    Y dos solicitudes en la cola de errores de firma
    Cuando consulta las solicitudes con novedades
    Entonces ve las dos solicitudes con su estudiante, tipo, fecha e intentos de firma

  Escenario: Sin novedades
    Dado un Funcionario de Registro autenticado
    Y ninguna solicitud en la cola de errores
    Cuando consulta las solicitudes con novedades
    Entonces ve una lista vacía y no un error

  Escenario: Reprocesar una solicitud
    Dado una solicitud en la cola de errores
    Cuando el Funcionario de Registro decide reprocesarla
    Entonces la solicitud vuelve a la cola de firma
    Y la decisión queda registrada en la bitácora con usuario y hora

  Escenario: Rechazar una solicitud con motivo
    Dado una solicitud en la cola de errores
    Cuando el Funcionario de Registro la rechaza indicando un motivo
    Entonces la solicitud queda en estado rechazada con ese motivo
    Y se notifica al estudiante

  Escenario: Rechazo sin motivo
    Dado una solicitud en la cola de errores
    Cuando el Funcionario de Registro intenta rechazarla sin motivo
    Entonces la operación se rechaza
    Y se le indica que el motivo es obligatorio

  Escenario: Usuario sin rol de Funcionario (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta consultar las solicitudes con novedades
    Entonces la petición se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
