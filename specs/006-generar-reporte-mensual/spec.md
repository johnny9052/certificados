# language: es
# Traza: CU-06, OB-01, EAC-04, ADR-004, ADR-005

Característica: Generar reporte mensual
  Como Funcionario de Registro
  quiero generar el reporte mensual de certificados emitidos
  para informar la gestión del periodo a la Universidad.

  Escenario: Reporte de un mes con certificados emitidos
    Dado un Funcionario de Registro autenticado
    Y certificados emitidos durante marzo de 2026 de tipo notas y de tipo matrícula
    Cuando genera el reporte de marzo de 2026
    Entonces ve el total de certificados emitidos por tipo
    Y ve el total general

  Escenario: Mes sin certificados
    Dado un Funcionario de Registro autenticado
    Y un mes sin certificados emitidos
    Cuando genera el reporte de ese mes
    Entonces ve todos los totales en cero y no un error

  Escenario: Solo cuentan los certificados emitidos
    Dado solicitudes del mes en estados registrada, en firma, emitida y rechazada
    Cuando se genera el reporte
    Entonces solo se cuentan las solicitudes emitidas
    Y el reporte indica además cuántas están en la cola de errores

  Escenario: Mes futuro
    Dado un Funcionario de Registro autenticado
    Cuando pide el reporte de un mes que aún no ha ocurrido
    Entonces se rechaza la petición
    Y se le indica que el mes no es válido

  Escenario: Usuario sin rol de Funcionario (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta generar el reporte mensual
    Entonces la petición se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
