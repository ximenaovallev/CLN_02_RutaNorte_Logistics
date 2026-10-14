# Matriz de trazabilidad (Unidad 3)

Convención de IDs: **N1–N8** necesidades del BRD · **US-01–US-17** historias de la Unidad 2 · **CU-01–CU-05** casos de uso ·
**S/T/G/E/B/DS** elementos del BPMN (`diagrams/bpmn/01_bpmn_rutanorte.bpmn`, mismos IDs que la matriz del reporte de la Unidad 2) · **ADR-00x** decisiones.

## 1. Casos de uso → componentes
| CU | Necesidades | Historias | Elementos BPMN | Componentes (diagrama de clases) | ADR | Pruebas |
|---|---|---|---|---|---|---|
| CU-01 Responder cotización | N1, N2 | US-01, 02, 03, 17 | S1, T1, T2, T3, G1, E1 | `ResponderCotizacionHandler`, `Cotizacion`, `SolicitudDeCotizacion`, `CotizacionAceptada`, `ICotizacionRepository` | 001, 002, 005 | `test_cu01_*` |
| CU-02 Generar orden de servicio | N1, N2 | US-05, 06 | T4 | `GenerarOrdenDeServicioHandler`, `OrdenDeServicio.generarDesde`, `OrdenDeServicioGenerada` | 001, 005 | `test_cu02_*` |
| CU-03 Asignar unidad y operador | N2, N5 (N8 pendiente) | US-07, 08, 10 | T6, G2, T7, T8 | `AsignarRecursosHandler`, `AsignadorDeRecursos`, `PoliticaDeCapacidad`, `Asignacion`, `RecursosAsignados`, `Unidad`, `Operador` | 001, 004 | `test_cu03_*` |
| CU-04 Registrar entrega o incidencia | N2, N4, N5 | US-10, 11, 12, 13 | T16, T17, T18, G4, T19, T20, T21, E4, E5 | `RegistrarEntregaHandler`, `OrdenDeServicio.registrarEntrega / registrarIncidencia`, `Incidencia`, `Evidencia`, `Destinatario`, `EntregaRegistrada`, `IncidenciaRegistrada` | 001, 005 | `test_cu04_*` |
| CU-06 Cancelar servicio (S-04) | N1, N2, N5 | US-05 | S2, T5, G5, T5A, T5B, E2, E2B | `CancelarServicioHandler`, `AutoridadDeCancelacion`, `OrdenDeServicio.cancelar`, `ServicioCancelado` | 006 | `test_s04_*` |
| CU-07 Autorizar carga de alto valor (S-02) | N8 | US-16 | G2 | `AutorizarCargaDeAltoValorHandler`, `Mercancia.altoValor`, `OrdenDeServicio.autorizarAltoValor`, `CargaDeAltoValorAutorizada` | 006 | `test_s02_*` |
| CU-05 Consultar estado e indicadores | N3, N6, N7 | US-14, 15 | T22, T23, DS1 | `ConsultarEstadoDelServicioHandler`, `ConsultarIndicadoresHandler`, `IConsultaDeServicios`, `EstadoDelServicioDTO`, `IndicadoresDTO` | 003 | `test_cu05_*` |

## 2. Reglas del negocio → dónde viven
| Regla (BRD / entrevista) | Ubicación | Prueba |
|---|---|---|
| La cotización solo se modifica mientras está en proceso (US-03) | `Cotizacion` (invariante) | `test_cotizacion_solo_se_modifica_mientras_esta_en_proceso` |
| La orden solo se genera tras la aceptación (US-06) | `OrdenDeServicio.generarDesde` | `test_cu02_orden_se_genera_solo_tras_aceptacion` |
| Unidad con capacidad suficiente y operador disponible (G2) | `AsignadorDeRecursos` + `PoliticaDeCapacidad` | `test_cu03_*` |
| Reasignar conserva el historial (US-08) | `OrdenDeServicio.asignarRecursos` | `test_cu03_reasignar_conserva_el_historial` |
| Todo cambio de estado e incidencia queda en el historial (N2) | `OrdenDeServicio` | `test_toda_incidencia_queda_en_el_historial_sin_cambiar_estado` |
| La entrega exige llegada a destino, receptor y evidencia (US-12, 13) | `OrdenDeServicio.registrarEntrega` | `test_cu04_*` |
| Una incidencia no entrega el servicio; Operaciones decide el siguiente paso (G4) | `OrdenDeServicio.registrarIncidencia` | `test_cu04_destinatario_ausente_*` |
| Las evidencias permanecen asociadas al servicio (N4) | agregado `OrdenDeServicio` | `test_cu04_*` |
| Atención al Cliente consulta sin depender de Operaciones (N6) | lado Query (CQRS) | `test_cu05_*` |

## 3. Elementos del BPMN sin Application Service en esta iteración
| Elementos | Situación |
|---|---|
| T9, T10, T11, T15 (recepción, traslado, llegada), G3, T14 | El dominio los soporta (`registrarRecepcion`, `iniciarTraslado`, `registrarLlegadaADestino`, `registrarIncidencia`); no se diseñó handler porque quedan fuera de los 5 casos de uso detallados. |
| B1, T12, T13, E6 (avería o retraso) | Se registra como `Incidencia` de tipo `UNIDAD_AVERIADA` o `RETRASO`; sin handler propio. |
| S2, T5, G5, T5A, T5B, E2, E2B (cambio o cancelación posterior a la orden) | La cancelación se resolvió con el supuesto S-04 (CU-06). La revisión de cambios (T5B) no tiene handler. |
| T3 modificar cotización | Existe `Cotizacion.modificar`; sin handler en esta iteración. |

## 4. Hallazgos y pendientes
Los pendientes del BRD se resolvieron como supuestos S-01 a S-05 (`05_supuestos_pendientes.md`), todos por validar.
1. **IDs del BPMN:** el BPMN rehecho usaba numeración distinta a la matriz del reporte; se unificó con los IDs del reporte (T1…T23, G1…G4, E1…E7) y se agregaron los nuevos (S2–S4, T5, T5A, T5B, G5, E2B, E8).
2. **Pendientes del BRD:** resueltos como supuestos configurables S-01 (capacidad), S-02 (alto valor, N8/US-16), S-03 (conservación de evidencias), S-04 (autoridad de cancelación) y S-05 (cumplimiento).
3. **Indicadores:** "cumplimiento de entregas" se calcula con el supuesto S-05 (a tiempo respecto de la fecha requerida).
4. **Alcance:** US-04 y US-09 no tienen handler propio en esta iteración. Se agregó el estado `CANCELADO` (cambio de alcance a validar, ver `05_supuestos_pendientes.md`).
5. **Tecnología de persistencia:** no definida; se usa implementación en memoria (ADR-002).
