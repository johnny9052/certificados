# ADR-008: Empaquetar la API y el worker en contenedores Docker sobre el servidor de aplicaciones institucional

## Estado

Aceptado (2026-03-19). Complementa los ADR-001 a ADR-003 vistos en clase.

## Contexto

La preocupación PA-06 pide decidir cómo se construye y se despliega el sistema. La restricción RT-01 exige la infraestructura institucional (sin nube pública) y RN-03 impide licencias comerciales. El Área de Sistemas necesita desplegar y monitorear con facilidad, y la API y el worker deben reiniciarse y escalar por separado (ADR-001). El equipo de 4 personas (RN-01) necesita un entorno repetible entre desarrollo y producción.

## Alternativas consideradas

- **A) Instalación directa en el servidor (procesos del sistema operativo).** Sin herramientas nuevas; pero difícil de reproducir y de actualizar sin dejar el servidor en estados distintos.
- **B) Contenedores Docker en el servidor de aplicaciones institucional, con una imagen por contenedor (API y worker).** Entorno idéntico en desarrollo y producción; cada contenedor se reinicia y escala por separado; requiere que el Área de Sistemas soporte Docker.
- **C) Orquestador completo (Kubernetes).** Máxima flexibilidad; pero supera la capacidad operativa del equipo y del alcance (RN-01).

## Decisión

Se elige la opción B, porque cumple RT-01 con un entorno repetible y escalado independiente de API y worker. La aplicación web Angular se compila a archivos estáticos que sirve Nginx en un servidor web. Broker, base de datos y almacenamiento corren en sus propios nodos.

## Consecuencias

### Positivas

- Despliegue repetible con un solo comando por contenedor.
- API y worker se reinician y escalan de forma independiente.

### Negativas y costos aceptados

- El Área de Sistemas debe disponer y soportar Docker.
- Se mantienen imágenes y su proceso de construcción.

### Riesgos asumidos

- Que Docker no esté disponible en el servidor asignado. Mitigación: confirmarlo con el Área de Sistemas al inicio.

### Vistas que modifica

[Física](../vistas/04-fisica.md) (servidor de aplicaciones Docker y servidor web Nginx).
