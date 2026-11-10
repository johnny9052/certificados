# language: es
# Traza: CU-05, EAC-04, ADR-004, ADR-005

Característica: Actualizar perfil
  Como estudiante o egresado autenticado
  quiero actualizar mi correo de contacto y mi autorización de notificaciones
  para recibir el aviso cuando mi certificado esté listo.

  Escenario: Actualizar el correo de contacto
    Dado un estudiante autenticado
    Cuando actualiza su correo por una dirección válida
    Entonces el perfil queda con el nuevo correo

  Escenario: Correo inválido
    Dado un estudiante autenticado
    Cuando intenta guardar un correo con formato inválido
    Entonces se rechaza el cambio
    Y se le indica que el correo no es válido
    Y el perfil conserva el correo anterior

  Escenario: Cambiar la autorización de correo
    Dado un estudiante autenticado que autorizó el correo
    Cuando retira la autorización
    Entonces el perfil queda sin autorización de correo
    Y las notificaciones por correo dejan de enviarse

  Escenario: Datos que vienen del SIA no se editan
    Dado un estudiante autenticado
    Cuando intenta modificar su documento o su programa
    Entonces el cambio se rechaza
    Y se le indica que esos datos provienen del SIA

  Escenario: Perfil de otra persona (EAC-04)
    Dado un estudiante autenticado
    Cuando intenta actualizar el perfil de otro estudiante
    Entonces el cambio se rechaza
    Y el intento queda registrado en la bitácora con usuario y hora
