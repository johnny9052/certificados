# ADR-004: Autenticar con el mecanismo institucional y autorizar por titularidad con tokens de corta vida

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

La preocupación PA-02 exige decidir cómo se identifica al usuario y cómo se protege cada certificado. El escenario [EAC-04](../drivers.md#eac-04-seguridad-confidencialidad-y-autenticidad) exige que solo el titular acceda a su certificado y que todo intento ajeno se rechace y se registre. El supuesto S-03 indica que existe un mecanismo de autenticación institucional, y la restricción RN-03 impide licencias comerciales. La API debe poder replicarse sin compartir estado (ADR-002).

## Alternativas consideradas

- **A) Sesiones en servidor.** Simples de revocar; pero obligan a compartir estado entre réplicas de la API.
- **B) Token firmado (JWT) de corta vida emitido tras la autenticación institucional.** La API valida el token sin consultar un almacén de sesiones; revocar antes del vencimiento es más difícil.
- **C) Usuario y contraseña propios del sistema.** Control total; pero duplica credenciales, aumenta la superficie de ataque y la carga de custodia de datos personales.

## Decisión

Se elige la opción B, porque cumple EAC-04 sin estado compartido y reutiliza la identidad institucional (no crea credenciales nuevas). La autorización se aplica en el caso de uso: toda solicitud pertenece a un Estudiante y solo su titular la consulta o descarga.

## Consecuencias

### Positivas

- La API se replica sin sesiones compartidas.
- No se almacenan contraseñas en el sistema.

### Negativas y costos aceptados

- Dependencia del proveedor de identidad institucional (si cae, nadie ingresa).
- Revocar un token antes de su vencimiento es costoso; se acota con vida corta.

### Riesgos asumidos

- Mecanismo institucional distinto al supuesto. Mitigación: validar S-03 con el Administrador del SIA antes de implementar.

### Vistas que modifica

[Desarrollo](../vistas/03-desarrollo.md) (Validador de token en el controlador REST) y [Contexto](../contexto.md) (identidad institucional como parte de la autenticación).
