# ADR-009: Atender el acceso móvil con una aplicación web adaptable en Angular

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

El objetivo OB-04 busca atender al 40% de egresados que no acceden desde computador, y [EAC-05](../drivers.md#eac-05-capacidad-de-interacción-operabilidad) exige completar el trámite en menos de 2 minutos desde un teléfono. El equipo domina Angular (RN-04), dispone de 4 personas y 16 semanas (RN-01) y el alcance excluye la aplicación móvil nativa.

## Alternativas consideradas

- **A) Aplicación móvil nativa (Android e iOS).** Mejor integración con el dispositivo; pero exige dos bases de código y publicación en tiendas, inviable en 16 semanas.
- **B) Aplicación web adaptable (Angular, diseño responsive).** Una sola base de código para computador y teléfono, sin instalación; sin funciones nativas.
- **C) Interfaz solo para computador.** La más rápida; incumple OB-04.

## Decisión

Se elige la opción B, porque cumple OB-04 y EAC-05 con el conocimiento del equipo (RN-04) y dentro del plazo (RN-01). El diseño parte de pantalla pequeña y el trámite se guía paso a paso hasta la confirmación, con un mensaje claro de que el certificado llegará por notificación (consecuencia de ADR-001).

## Consecuencias

### Positivas

- Una sola base de código para todos los dispositivos.
- Sin instalación ni publicación en tiendas.

### Negativas y costos aceptados

- Sin notificaciones push ni funciones nativas.
- El rendimiento depende del navegador y de la conexión del usuario.

### Riesgos asumidos

- Que la prueba de usuario no alcance la medida de EAC-05. Mitigación: probar con usuarios reales antes de cerrar la interfaz y ajustar.

### Vistas que modifica

[Procesos](../vistas/02-procesos.md) (contenedor Aplicación web).
