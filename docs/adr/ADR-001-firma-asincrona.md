# ADR-001: Procesar la firma digital de forma asíncrona con una cola de mensajes

## Estado

Aceptado (2026-03-12)

## Contexto

El proveedor de firma digital (RT-03) responde entre 2 y 8 segundos y no garantiza disponibilidad continua. El escenario [EAC-02](../drivers.md#eac-02-fiabilidad-tolerancia-a-fallos) exige que ninguna solicitud se pierda ante una falla del proveedor, y [EAC-01](../drivers.md#eac-01-desempeño-comportamiento-temporal) exige respuesta al usuario en menos de 3 s. Ambos drivers son incompatibles con una llamada sincrónica. Casos de uso involucrados: CU-01 y CU-03. Objetivos de negocio: OB-01 y OB-02.

## Alternativas consideradas

- **A) Llamada sincrónica con timeout.** Simple, sin infraestructura adicional; pero incumple EAC-01 (el proveedor tarda hasta 8 s) y EAC-02 (si el proveedor cae, la solicitud se pierde).
- **B) Cola de mensajes con reintentos.** La API deja cada solicitud en una cola y un proceso en segundo plano (worker) la envía al proveedor, reintentando si falla. Cumple ambos escenarios; agrega un componente de infraestructura (el broker de mensajes) y el usuario no recibe el certificado en el mismo instante.
- **C) Caché de certificados pre-firmados.** Respuesta inmediata, pero inviable: cada certificado es único y fechado.

## Decisión

Se elige la opción B, porque es la única que cumple EAC-01 y EAC-02. El usuario recibe confirmación inmediata de la solicitud y una notificación cuando el certificado está listo. El worker reintenta hasta 5 veces con espera creciente (2, 4, 8 y 16 s) y, si se agotan los intentos, envía la solicitud a la cola de errores.

## Consecuencias

### Positivas

- Se cumplen EAC-01 y EAC-02; el sistema tolera caídas del proveedor y la firma se reintenta sin intervención humana.
- Solo el worker habla con el proveedor de firma (RT-03), lo que aísla la integración.

### Negativas y costos aceptados

- Se introduce un broker de mensajes que hay que operar y monitorear.
- La experiencia de usuario pasa de síncrona a "solicitar y esperar notificación", y eso debe comunicarse con claridad en la interfaz.
- Las solicitudes que agotan los 5 intentos dependen de la revisión del Funcionario de Registro (DT-01 en [arquitectura.md](../arquitectura.md#9-riesgos-y-deuda-técnica)).

### Riesgos asumidos

- R-01: si la cola crece sin control, las solicitudes se acumulan. Mitigación: alerta al superar 200 elementos.

### Vistas que modifica

[Procesos](../vistas/02-procesos.md) (broker y worker), [Física](../vistas/04-fisica.md) (servidor de mensajería), [Lógica](../vistas/01-logica.md) (IntentoFirma) y la secuencia [CU-01](../diagramas/secuencias/cu01-solicitar-certificado.puml) y [CU-03](../diagramas/secuencias/cu03-firmar-digitalmente.puml).
