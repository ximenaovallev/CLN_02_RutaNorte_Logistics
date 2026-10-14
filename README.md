\# CLN\_02\_RutaNorte\_Logistics



\*\*Curso:\*\* Componentes de Lógica de Negocio · \*\*Profesor:\*\* Miguel Ángel Gómez Marroquín

\*\*Actividad:\*\* Diseño arquitectural del sistema con IA (Unidad 3)



\*\*Integrantes\*\*

\- Ximena Montserrat Ovalle Vallejo – 1999238

\- Santiago Almanza Moreno – 2001903

\- Javier Hernández Huerta – 2089204



\## Empresa y alcance

RutaNorte Logistics (transporte terrestre de mercancía entre Monterrey, Saltillo, Reynosa, San Luis Potosí y Querétaro).

Proceso modelado: de la cotización aceptada a la entrega. Continúa el caso de la Unidad 2 sin cambiar el alcance del BRD.



\## Entregables

| # | Entregable | Enlace |

|---|---|---|

| 01 | Informe de arquitectura (PDF) | \[docs/01\_informe\_arquitectura.pdf](docs/01\_informe\_arquitectura.pdf) |

| 02 | Diagrama de dominio y clases | \[fuente .puml](diagrams/02\_diagrama\_clases.puml) · \[PNG](diagrams/02\_diagrama\_clases.png) |

| 03 | Diagramas de secuencia | \[CU-03](diagrams/03a\_secuencia\_asignar\_recursos.puml) (\[PNG](diagrams/03a\_secuencia\_asignar\_recursos.png)) · \[CU-04](diagrams/03b\_secuencia\_registrar\_entrega.puml) (\[PNG](diagrams/03b\_secuencia\_registrar\_entrega.png)) |

| 04 | Matriz de trazabilidad | \[docs/04\_matriz\_trazabilidad.md](docs/04\_matriz\_trazabilidad.md) |

| 05 | Bitácora de IA | \[docs/05\_bitacora\_ia.md](docs/05\_bitacora\_ia.md) |

| 06 | Código base | \[src/](src/) · \[tests/](tests/) |

| 07 | Este repositorio | — |

| 08 | Presentación | \[docs/08\_presentacion.pdf](docs/08\_presentacion.pdf) |



\*\*Otros documentos:\*\* \[BPMN](diagrams/bpmn/01\_bpmn\_rutanorte.bpmn) · \[Casos de uso](docs/casos\_de\_uso.md) · \[Supuestos por validar](docs/05\_supuestos\_pendientes.md) · ADR: \[001](docs/adr/ADR-001-patron-de-logica-de-negocio.md), \[002](docs/adr/ADR-002-repository-y-unit-of-work.md), \[003](docs/adr/ADR-003-cqrs-basico.md), \[004](docs/adr/ADR-004-politica-de-capacidad-como-abstraccion.md), \[005](docs/adr/ADR-005-limite-del-agregado-orden-de-servicio.md), \[006](docs/adr/ADR-006-supuestos-pendientes-como-parametros.md)



\## Cómo revisar

1\. Leer el informe y la matriz de trazabilidad.

2\. Abrir los diagramas: los `.puml` se pegan en [https://www.plantuml.com/plantuml](https://www.planttext.com/); el `.bpmn` se abre en https://demo.bpmn.io.

3\. Ejecutar las pruebas (Python 3.10+): `pip install pytest` y luego `python -m pytest`.



\## Estructura

`diagrams/` fuentes y exportaciones · `docs/` informe, ADR, trazabilidad, bitácora · `src/` dominio, aplicación, infraestructura · `tests/`

