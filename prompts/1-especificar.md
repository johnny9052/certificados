# Fase 1: Especificar

Entrada: `AGENTS.md`, `docs/arquitectura.md`, `docs/drivers.md` y el caso de uso (CU) indicado.

Produce `specs/NNN-nombre/spec.md` con este formato:

```text
# language: es
# Traza: CU-NN, EAC-NN, ADR-NNN
Característica: <nombre del caso de uso>
  <rol, acción y beneficio>

  Escenario: <flujo básico>
    Dado ...
    Cuando ...
    Entonces ...
  Escenario: <flujo alterno o excepción>
  Escenario: <un EAC que toca el CU, con su medida>
```

Reglas:
- El flujo básico del CU es un escenario; cada flujo alterno o excepción es otro; cada EAC que toca el CU es otro con su medida.
- Cada escenario se comprueba con un sí o un no, con datos concretos.
- La traza cita los IDs de `docs/drivers.md` y de los ADR.
- Describir comportamiento, no cómo se programa (el cómo va en `plan.md`).
- No inventar requisitos: lo que falte se pregunta.
- Al terminar, detenerse y esperar revisión de la persona.
