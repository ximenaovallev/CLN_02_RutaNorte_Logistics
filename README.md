# RutaNorte Logistics – Unidad 3: arquitectura y código base

Continuación de la Unidad 2 (misma empresa y mismo alcance). Proceso modelado: de la cotización aceptada a la entrega.

## Estructura
```
README.md
pyproject.toml
diagrams/
  bpmn/01_bpmn_rutanorte.bpmn (+ .png)    BPMN 2.0 (IDs iguales a la matriz de la U2)
  02_diagrama_clases.puml (+ .png)        dominio / aplicación / infraestructura
  03a_secuencia_asignar_recursos.puml     CU-03 (+ .png)
  03b_secuencia_registrar_entrega.puml    CU-04 (+ .png)
docs/
  casos_de_uso.md
  04_matriz_trazabilidad.md
  05_supuestos_pendientes.md   S-01 … S-05 (por validar con el stakeholder)
  glosario_actualizado.md      lenguaje ubicuo: términos corregidos y nuevos
  adr/ADR-001 … ADR-006
src/rutanorte/
  dominio/          Cotizacion, OrdenDeServicio, Unidad, Operador, eventos, servicio de dominio, interfaces de Repository
  aplicacion/       commands/queries, handlers (Service Layer), puertos (IUnitOfWork, ...), DTO
  infraestructura/  implementaciones en memoria
  composicion.py    raíz de composición
tests/              30 pruebas: casos de uso, dominio, transacción y supuestos
```

## Ejecutar las pruebas
```
pip install pytest
python -m pytest
```
Requiere Python 3.10 o superior.

## Generar los diagramas
- `.puml`: pegar en https://www.plantuml.com/plantuml o usar la extensión PlantUML de VS Code.
  Por consola: `java -DPLANTUML_LIMIT_SIZE=12000 -jar plantuml.jar -tpng diagrams/*.puml`
- `.bpmn`: abrir en https://demo.bpmn.io, Camunda Modeler o Bizagi Modeler.

## Convenciones
Las clases usan los nombres del glosario en español. En el código los métodos van en `snake_case`
(`registrar_entrega`) y en los diagramas en `camelCase` (`registrarEntrega`).

## Pendientes del BRD
Los cinco puntos que el BRD no definió se resolvieron como **supuestos configurables, marcados «pendiente de validar»**
(`docs/05_supuestos_pendientes.md`, ADR-006). La tecnología de persistencia sigue sin definirse (implementación en memoria).
