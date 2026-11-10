# Fase 5: Validar

Entrada: `AGENTS.md`, `docs/arquitectura.md`, `spec.md`, `plan.md`, `tasks.md` y el código de `src/`.

Produce `validacion.md` con:

```text
## Validación funcional
| Escenario | Resultado (Cumple o No cumple) | Evidencia |

## Validación arquitectónica
- Regla de dependencia: ningún import de infraestructura/ dentro de dominio/.
- Cada ADR citado en el plan se ve en el código.
- Se cumplen las convenciones de AGENTS.md.
```

Reglas:
- Un resultado por cada escenario de `spec.md`, con evidencia concreta (salida de ejecución, archivo y línea).
- No dar un escenario por cumplido sin evidencia.
- Si algo no coincide, informarlo; no corregirlo en esta fase.
- Al terminar, detenerse: la persona verifica la evidencia.
