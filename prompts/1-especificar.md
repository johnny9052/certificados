# Fase 1: Especificar

Entrada: `AGENTS.md`, `docs/arquitectura.md` y la fila del caso de uso indicado (CU-NN) en el catálogo de `docs/drivers.md` (sección 1), con los EAC que lo afectan. La carpeta `specs/NNN-nombre/` lleva el mismo número del CU.

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
- No inventar requisitos. Donde falte información, escribir en ese mismo lugar el comentario `# [NECESITA ACLARACIÓN: <pregunta concreta>]` (con `#` delante, para que siga siendo Gherkin válido) y seguir con lo demás.
- Al terminar, listar todas las marcas `[NECESITA ACLARACIÓN]` que dejó.
- Al terminar, detenerse y esperar revisión de la persona.

Revisión de la persona:
- Verificar cada escenario contra el caso de uso y completar en este mismo `spec.md` lo que falte.
- Resolver cada marca `[NECESITA ACLARACIÓN]` y borrarla. La revisión termina cuando no queda ninguna.
- Confirmar que la columna "Se especifica en" de `docs/drivers.md` enlaza a esta carpeta.
