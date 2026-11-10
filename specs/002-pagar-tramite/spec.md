# language: es
# Traza: CU-02, EAC-04, RT-01, ADR-002, ADR-004, ADR-005, ADR-006

Característica: Pagar el trámite
  Como estudiante o egresado autenticado
  quiero pagar el trámite de mi certificado en línea
  para poder solicitar el certificado sin ir a una caja.

  Escenario: Pago iniciado
    Dado un estudiante autenticado
    Y un tipo de certificado que requiere pago
    Cuando inicia el pago del trámite
    Entonces se registra un pago en estado pendiente con su referencia
    Y recibe la URL de pago de la pasarela

  Escenario: Pago aprobado
    Dado un pago pendiente
    Cuando la pasarela notifica que el pago fue aprobado
    Entonces el pago queda en estado aprobado
    Y la notificación se responde como recibida

  Escenario: Pago rechazado
    Dado un pago pendiente
    Cuando la pasarela notifica que el pago fue rechazado
    Entonces el pago queda en estado rechazado
    Y no se crea ninguna solicitud
    Y el estudiante puede intentar el pago de nuevo

  Escenario: La pasarela no responde al crear la transacción
    Dado un estudiante autenticado
    Y la pasarela no responde en el tiempo máximo de espera
    Cuando inicia el pago del trámite
    Entonces no queda ningún pago pendiente sin URL
    Y se le informa que la pasarela no está disponible y que puede intentarlo más tarde

  Escenario: Notificación repetida
    Dado un pago que ya está en estado aprobado
    Cuando la pasarela vuelve a notificar el mismo resultado
    Entonces el estado del pago no cambia
    Y la notificación se responde como recibida

  Escenario: Tipo de certificado sin costo
    Dado un tipo de certificado que no requiere pago
    Cuando el estudiante intenta iniciar el pago
    Entonces se rechaza la operación
    Y se le indica que ese tipo de certificado no requiere pago
