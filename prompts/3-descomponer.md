# Fase 3: Descomponer

Entrada: `AGENTS.md`, `docs/arquitectura.md`, `spec.md` y `plan.md` de la funcionalidad indicada.

Produce `tasks.md` con este formato:

```text
- [ ] T-NN Título corto
      Archivos: rutas que se crean o cambian
      Escenarios: los de spec.md que atiende
      Depende de: T-NN o "ninguna"
```

Reglas:
- Cada tarea toca una sola clase o archivo.
- Orden de adentro hacia afuera: dominio, puertos, casos de uso, adaptadores, composición.
- Cada escenario de `spec.md` aparece en al menos una tarea.
- Incluir una tarea final de validación que produce `validacion.md`.
- No escribir código en esta fase.
- Al terminar, detenerse y esperar revisión.
