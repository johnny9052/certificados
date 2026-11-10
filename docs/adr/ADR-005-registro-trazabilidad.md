# ADR-005: Registrar la auditoría en una tabla de PostgreSQL dentro de la misma transacción

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

La preocupación PA-04 pide decidir qué se registra, dónde y por cuánto tiempo. [EAC-04](../drivers.md#eac-04-seguridad-confidencialidad-y-autenticidad) exige registrar con usuario y hora los intentos de acceso ajeno, y el Área de soporte necesita saber dónde se detuvo una solicitud (OB-02). El modelo de dominio ya conserva cada llamada al proveedor como IntentoFirma. El equipo de 4 personas (RN-01) debe evitar infraestructura adicional.

## Alternativas consideradas

- **A) Solo archivos de log de texto.** Sin infraestructura; pero no se consultan ni se vinculan a una solicitud.
- **B) Tabla de auditoría en PostgreSQL, escrita en la misma transacción del caso de uso.** Consultable y consistente con el estado; suma unos milisegundos por petición.
- **C) Eventos de auditoría publicados en una cola y persistidos de forma asíncrona.** Menor latencia; agrega un flujo más que operar y la posibilidad de perder eventos.

## Decisión

Se elige la opción B, porque da trazabilidad consultable por solicitud sin componentes nuevos (RN-01) y su costo en tiempo es pequeño frente al presupuesto de 3 s de EAC-01. Se registran: emisión de certificados, cambios de estado, intentos de firma y accesos rechazados, con usuario, hora y el ID de la solicitud. La retención sigue la política institucional (S-07).

## Consecuencias

### Positivas

- Consultas de soporte por solicitud, usuario y fecha.
- Consistencia entre el cambio de estado y su registro.

### Negativas y costos aceptados

- Cada operación escribe una fila adicional y la tabla crece con el uso.
- Si la auditoría falla, falla la operación completa.

### Riesgos asumidos

- Impacto en el tiempo de respuesta. Mitigación: medir contra EAC-01 y pasar a escritura asíncrona (opción C) solo si el escenario medido lo exige, con un ADR nuevo que reemplace a este.

### Vistas que modifica

[Desarrollo](../vistas/03-desarrollo.md) (Adaptador de bitácora) y [Lógica](../vistas/01-logica.md) (IntentoFirma como rastro).
