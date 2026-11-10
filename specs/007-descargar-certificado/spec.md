# language: es
# Traza: CU-07, EAC-04, RN-02, ADR-003, ADR-004, ADR-005, ADR-006

Característica: Descargar certificado firmado
  Como estudiante o egresado autenticado
  quiero descargar mi certificado firmado
  para presentarlo ante quien me lo solicita.

  Escenario: Descarga del certificado propio
    Dado un estudiante autenticado con una solicitud emitida
    Cuando descarga su certificado
    Entonces recibe el PDF firmado
    Y la descarga queda registrada en la bitácora

  Escenario: Certificado aún no emitido
    Dado un estudiante autenticado con una solicitud en estado en firma
    Cuando intenta descargar su certificado
    Entonces no recibe ningún archivo
    Y se le indica el estado actual de la solicitud

  Escenario: Certificado de otra persona (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta descargar el certificado de otro estudiante
    Entonces la descarga se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
    Y esto ocurre en el 100 % de los intentos

  Escenario: Archivo no encontrado en el repositorio
    Dado una solicitud emitida cuyo PDF no existe en el repositorio
    Cuando el titular intenta descargarlo
    Entonces se le informa que el certificado no está disponible temporalmente
    Y la inconsistencia queda registrada en la bitácora para soporte

  Escenario: Solicitud inexistente
    Dado un estudiante autenticado
    Cuando intenta descargar una solicitud que no existe
    Entonces se le informa que no se encontró la solicitud
