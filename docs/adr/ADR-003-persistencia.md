# ADR-003: Persistir los datos del trámite en PostgreSQL y los PDF firmados en un repositorio de archivos institucional

## Estado

Aceptado (2026-03-12)

## Contexto

La restricción RT-02 obliga a usar PostgreSQL, lo que implica un modelo relacional para solicitudes, pagos, intentos de firma y estados. La restricción RN-02 (Ley 1581 de 2012) exige que los datos personales no salgan del país, y RT-01 exige desplegar en la infraestructura institucional. Falta decidir dónde se guardan los certificados firmados (archivos PDF), que son documentos con datos personales.

## Alternativas consideradas

- **A) Guardar el PDF dentro de PostgreSQL (campo binario).** Un solo almacén y respaldo único; pero la base crece rápido, los respaldos se vuelven pesados y la descarga compite con las consultas del trámite.
- **B) Repositorio de archivos en el almacenamiento institucional, con la ruta del PDF en PostgreSQL.** Separa documentos de datos transaccionales, escala de forma independiente y queda dentro del país; agrega un segundo almacén que hay que respaldar y mantener consistente con la base.
- **C) Almacenamiento de objetos en nube pública.** Escalable y barato, pero incumple RT-01 y RN-02.

## Decisión

PostgreSQL es impuesto por RT-02 y se acepta sin alternativas: guarda solicitudes, pagos, intentos de firma, tipos de certificado y bitácora. Para los PDF se elige la opción B, porque cumple RT-01 y RN-02 y mantiene liviana la base de datos. El acceso a ambos almacenes se aísla detrás de puertos (RepositorioCertificados y un puerto de almacenamiento de documentos), y PostgreSQL queda confinado a un solo adaptador.

## Consecuencias

### Positivas

- Se cumplen RT-02, RT-01 y RN-02; los datos personales y los PDF permanecen en el centro de datos institucional.
- Cambiar el motor o el almacenamiento toca un solo adaptador.

### Negativas y costos aceptados

- Dos almacenes que respaldar y mantener consistentes: si el worker guarda el PDF y falla antes de actualizar la base, queda un archivo huérfano.
- Descartar el modelo documental limita la flexibilidad del esquema de los tipos de certificado.

### Riesgos asumidos

- Archivos huérfanos o rutas rotas. Mitigación: el worker guarda el PDF primero y actualiza el estado después; un proceso periódico detecta inconsistencias.

### Vistas que modifica

[Procesos](../vistas/02-procesos.md) (PostgreSQL y repositorio de PDF), [Física](../vistas/04-fisica.md) (servidor de base de datos y almacenamiento institucional) y [Desarrollo](../vistas/03-desarrollo.md) (Adaptador PostgreSQL).
