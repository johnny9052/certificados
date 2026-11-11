# Fase 2: Planear

Entrada: `AGENTS.md`, `docs/arquitectura.md`, `docs/vistas/03-desarrollo.md` y el `spec.md` de la funcionalidad indicada.

Produce `plan.md` con estas secciones:

```text
# Plan: NNN <nombre>
## Decisiones que aplican   (ADR y vistas, con su ID)
## Componentes              (rutas de archivos según vistas/03-desarrollo.md)
## Flujo                    (diagrama de secuencia de diagramas/secuencias/)
## Fuera de alcance
```

Reglas:
- Si `spec.md` todavía tiene marcas `[NECESITA ACLARACIÓN]`, no planear: avisar y detenerse.
- El plan no rehace la arquitectura: la aterriza en la funcionalidad.
- No inventar componentes que no estén en las vistas.
- Si el plan necesita una decisión que no existe, detenerse: primero se escribe el ADR.
- Al terminar, detenerse y esperar revisión de la persona.
