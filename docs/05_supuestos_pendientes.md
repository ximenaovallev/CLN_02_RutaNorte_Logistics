# Supuestos pendientes de validar (S-01 a S-05)

El BRD dejó cinco puntos sin definir y prohíbe inventar políticas. En lugar de ignorarlos, cada uno se resolvió como un
**supuesto explícito, configurable y marcado «pendiente de validar»** (la guía de la Unidad 3 permite supuestos si se identifican como tales).
Ninguno es un hecho del negocio: antes de entregar al stakeholder deben confirmarse o corregirse.

| ID | Pendiente del BRD | Supuesto adoptado | Dónde se implementa | Cómo se cambia | Pregunta para el stakeholder |
|---|---|---|---|---|---|
| S-01 | Límites de peso/volumen/capacidad | La regla confirmada es "la unidad debe tener capacidad suficiente". No hay tope adicional de la empresa; si existe, se configura. | `PoliticaDeCapacidadEmpresa(peso_maximo_kg, volumen_maximo_m3)` | Parámetros de `construir_aplicacion` | ¿Existe un tope de peso o volumen por servicio, aparte de la capacidad de cada unidad? |
| S-02 | Controles para carga de alto valor | La mercancía se marca `alto_valor`; antes de asignar unidad y operador, Operaciones debe **autorizar** la carga (queda en el historial). | `Mercancia.alto_valor`, `OrdenDeServicio.autorizar_alto_valor`, `AutorizarCargaDeAltoValorHandler` | Reemplazar o ampliar el control en `OrdenDeServicio` | ¿Qué controles se aplican (autorización, seguimiento, escolta) y quién autoriza? ¿Qué define "alto valor"? |
| S-03 | Tiempo de conservación de evidencias | Sin plazo por defecto (no caducan); el plazo es un parámetro. | `PoliticaDeConservacion(dias)`, `OrdenDeServicio.evidencias(politica)` | `dias_de_conservacion` en `construir_aplicacion` | ¿Cuánto tiempo debe conservarse cada tipo de evidencia? |
| S-04 | Autoridad para cancelar | Se registra quién solicita y el motivo (confirmado en la entrevista). Se asume que **Operaciones** autoriza; el servicio pasa a `CANCELADO`; no se cancela un servicio entregado o ya cancelado. | `AutoridadDeCancelacion`, `OrdenDeServicio.cancelar`, `CancelarServicioHandler`, `EstadoServicio.CANCELADO` | `roles_que_cancelan` en `construir_aplicacion` | ¿Qué rol autoriza la cancelación y hasta qué estado puede cancelarse? |
| S-05 | Definición de "cumplimiento de entregas" | Cumplimiento = entregas realizadas en o antes de la **fecha requerida** por el cliente ÷ entregas realizadas (`None` si no hay entregas). | `IndicadoresDTO.cumplimiento_de_entregas`, `EnMemoriaConsultaDeServicios.indicadores` | Cambiar el cálculo en la consulta | ¿Cómo mide el negocio una entrega "cumplida"? ¿Con tolerancia o por horario? |

## Nota sobre el estado CANCELADO
Los ocho estados del caso no incluyen `CANCELADO`. Se agrega como estado terminal porque el glosario define *Cancelación* como "terminación de un servicio ya aceptado" y el BRD exige registrarla. Es un cambio de alcance a validar (regla de continuidad de la guía).

## Pendiente que sigue abierto
La tecnología de persistencia no está definida (ADR-002): se usa implementación en memoria.
