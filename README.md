# Sistema de certificados en línea: solución del caso visto en clase

Repositorio de referencia del caso de los certificados académicos firmados digitalmente, desarrollado a lo largo de la unidad de Arquitectura de Soluciones de Software de la Universidad de Caldas. Muestra cómo queda el caso resuelto desde la visión hasta SDD, con la misma estructura de carpetas que deben usar los equipos en sus proyectos.

## Cómo leerlo (en este orden)

| Paso | Archivo | Tema de clase |
|------|---------|---------------|
| 1 | [docs/vision.md](docs/vision.md) | Visión: objetivos, alcance, stakeholders, restricciones, supuestos y riesgos |
| 2 | [docs/drivers.md](docs/drivers.md) | Drivers: casos de uso, escenarios de calidad, restricciones, preocupaciones, matriz y escenarios (+1) |
| 3 | [docs/adr/](docs/adr/) | ADR: una decisión por archivo, con alternativas y consecuencias |
| 4 | [docs/contexto.md](docs/contexto.md) y [docs/vistas/](docs/vistas/) | Vistas 4+1 con C4 y UML |
| 5 | [docs/diagramas/](docs/diagramas/) | Diagramas como código (PlantUML): vistas y secuencias arquitectónicas |
| 6 | [docs/arquitectura.md](docs/arquitectura.md) | Documento de arquitectura: el mapa que enlaza todo |
| 7 | [AGENTS.md](AGENTS.md) y [prompts/](prompts/) | SDD: reglas permanentes y una instrucción por fase |
| 8 | [specs/001-solicitar-certificado/](specs/001-solicitar-certificado/) | SDD: spec, plan, tareas y validación del CU-01 |

## Cadena de trazabilidad

OB-01 a OB-05 (visión) → EAC y restricciones (drivers) → ADR-001 a ADR-009 → vistas y secuencias → arquitectura.md → spec 001 → tareas T-01 a T-22 → código en `src/` → validación.

Cada eslabón cita al anterior por su ID.

## Estructura

```text
certificados/
├── AGENTS.md
├── README.md
├── docs/
│   ├── vision.md
│   ├── drivers.md
│   ├── contexto.md
│   ├── arquitectura.md
│   ├── adr/                  ADR-001 a ADR-009
│   ├── vistas/               01-logica, 02-procesos, 03-desarrollo, 04-fisica (.md)
│   └── diagramas/            contexto, 01 a 04 (.puml) y secuencias/ (cu01, cu02, cu03)
├── specs/
│   └── 001-solicitar-certificado/   spec.md, plan.md, tasks.md, validacion.md
├── prompts/                  mensaje-de-arranque.md y 1 a 5 (una por fase)
└── src/                      vacía hasta la fase de implementación (.gitkeep)
```

## Cómo ver los diagramas

Los diagramas son archivos `.puml` (PlantUML, texto versionable). Se pueden ver con la extensión de PlantUML en VS Code o pegándolos en un editor en línea como editor.plantuml.com. Las vistas de C4 usan la librería estándar de PlantUML (`!include <C4/...>`).

## Estado

| Elemento | Estado |
|----------|--------|
| Visión, drivers, ADR, contexto, vistas, secuencias y arquitectura.md | Completos |
| AGENTS.md y prompts/ | Completos |
| Spec, plan y tareas de la funcionalidad 001 | Completos |
| Código en `src/` y `validacion.md` | Pendientes (fases 4 y 5 de SDD) |

## Notas para quien consulta

- Los ADR-001 a ADR-003, los casos de uso CU-01 a CU-06, los EAC-01 y EAC-02 y las restricciones RT-01 a RT-03, RN-01 y RN-02 corresponden al ejemplo visto en clase. El resto (ADR-004 a ADR-009, EAC-03 a EAC-05, RT-04, RN-03, RN-04, CU-07, CU-08, PA y escenarios adicionales de la spec) completa la cobertura de los drivers y se marca como complemento en cada archivo.
- Este caso es distinto del proyecto de prácticas empresariales que cada equipo desarrolla por su cuenta.
