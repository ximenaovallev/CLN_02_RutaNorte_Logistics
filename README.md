# RutaNorte Logistics – Unidad 3: diseño arquitectural del sistema con IA

**Curso:** Componentes de Lógica de Negocio · **Profesor:** Miguel Ángel Gómez Marroquín
**Equipo:** Ximena Ovalle Vallejo · Santiago Almanza Moreno · Javier Hernández Huerta
**Empresa:** RutaNorte Logistics (misma empresa y mismo alcance de la Unidad 2)

**Proceso modelado:** desde la respuesta del cliente a la cotización hasta la entrega de la mercancía o el registro de una incidencia en el destino.
**Patrón elegido:** Domain Model, con consultas simples para la lectura de estado e indicadores ([ADR-001](docs/adr/ADR-001-patron-de-logica-de-negocio.md)).

## Entregables

|#|Entregable|Archivos|
|-|-|-|
|01|Informe de arquitectura (9 etapas, con los insumos de la Unidad 2 en los anexos)|[INFORME.docx](docs/CLN_AU3_02_RutaNorte_Logistics_INFORME.docx) · [INFORME.pdf](docs/CLN_AU3_02_RutaNorte_Logistics_INFORME.docx.pdf)|
|02|Diagrama de dominio y clases|[fuente .puml](diagrams/02_diagrama_clases.puml) · [PNG](diagrams/02_diagrama_clases.png) · [PDF](diagrams/02_diagrama_clases.pdf)|
|03|Diagramas de secuencia|CU-03 asignar recursos: [.puml](diagrams/03a_secuencia_asignar_recursos.puml) · [PNG](diagrams/03a_secuencia_asignar_recursos.png) · [PDF](diagrams/03a_secuencia_asignar_recursos.pdf)<br>CU-04 registrar entrega: [.puml](diagrams/03b_secuencia_registrar_entrega.puml) · [PNG](diagrams/03b_secuencia_registrar_entrega.png) · [PDF](diagrams/03b_secuencia_registrar_entrega.pdf)|
|04|Matriz de trazabilidad|[docs/04\_matriz\_trazabilidad.md](docs/04_matriz_trazabilidad.md)|
|05|Bitácora de IA|[BITACORA\_IA.docx](docs/CLN_AU3_02_RutaNorte_Logistics_BITACORA_IA.docx) · [BITACORA\_IA.pdf](docs/CLN_AU3_02_RutaNorte_Logistics_BITACORA_IA.pdf)|
|06|Código base|[src/rutanorte](src/rutanorte) y [tests](tests)|
|07|Repositorio|Este repositorio|
|08|08 Presentacion y defensa|Defensa.pdf|



**Otros documentos:** [ADR (formato Word y PDF)](docs/adr) · [casos de uso](docs/casos_de_uso.md) · [supuestos pendientes S-01 a S-05](docs/05_supuestos_pendientes.md) · [glosario actualizado](docs/glosario_actualizado.md) · [BPMN](diagrams/bpmn) (fuente `.bpmn` y exportaciones)



## Architecture Decision Records

|ADR|Decisión|
|-|-|
|[ADR-001](docs/adr/ADR-001-patron-de-logica-de-negocio.md)|Domain Model como patrón de lógica de negocio|
|[ADR-002](docs/adr/ADR-002-repository-y-unit-of-work.md)|Repository y Unit of Work|
|[ADR-003](docs/adr/ADR-003-cqrs-basico.md)|CQRS básico|
|[ADR-004](docs/adr/ADR-004-politica-de-capacidad-como-abstraccion.md)|Capacidad de la unidad con `PoliticaDeCapacidad`|
|[ADR-005](docs/adr/ADR-005-limite-del-agregado-orden-de-servicio.md)|Límite del agregado `OrdenDeServicio`|
|[ADR-006](docs/adr/ADR-006-supuestos-pendientes-como-parametros.md)|Supuestos pendientes como parámetros|

## 

## Estructura

```
README.md
pyproject.toml
diagrams/   BPMN, diagrama de clases y dos secuencias (fuentes .bpmn / .puml y exportaciones PNG / PDF)
docs/       informe, bitácora de IA, matriz de trazabilidad, glosario, casos de uso, supuestos y adr/
src/rutanorte/
  dominio/          Cotizacion, OrdenDeServicio, Unidad, Operador, eventos, servicio de dominio, interfaces de Repository
  aplicacion/       commands y queries, handlers (Service Layer), puertos (IUnitOfWork, ...), DTO
  infraestructura/  implementaciones en memoria
  composicion.py    raíz de composición (único lugar que conecta la infraestructura)
tests/      30 pruebas: casos de uso, dominio, transacción y supuestos
```

## Cómo revisar

1. Leer el informe (`docs/…\_INFORME`): sigue las nueve etapas de la guía; los anexos D a G incluyen el BRD, las historias, el glosario y las matrices de la Unidad 2 con sus actualizaciones.
2. Revisar los diagramas en `diagrams/` y los ADR en `docs/adr/`.
3. Ejecutar las pruebas para comprobar el comportamiento del código base.

## Ejecutar las pruebas

```
pip install pytest
python -m pytest
```

Requiere Python 3.10 o superior.

## Generar los diagramas

* `.puml`: pegar en https://www.plantuml.com/plantuml o usar la extensión PlantUML de VS Code.
Por consola: `java -DPLANTUML\_LIMIT\_SIZE=12000 -jar plantuml.jar -tpng diagrams/\*.puml`
* `.bpmn`: abrir en https://demo.bpmn.io, Camunda Modeler o Bizagi Modeler.

## Convenciones

Las clases usan los nombres del glosario en español. En el código los métodos van en `snake\_case`
(`registrar\_entrega`) y en los diagramas en `camelCase` (`registrarEntrega`).

## Pendientes del BRD

Los cinco puntos que el BRD no definió se resolvieron como **supuestos configurables, marcados «pendiente de validar»**
(`docs/05\_supuestos\_pendientes.md`, ADR-006). Ninguno es un hecho del negocio: deben confirmarse con el stakeholder.
La tecnología de persistencia sigue sin definirse (implementación en memoria).

