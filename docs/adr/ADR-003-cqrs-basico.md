# ADR-003: CQRS básico (Commands/Queries) sin bases de datos separadas

**Estado:** Aceptada

## Contexto
Atención al Cliente depende de Operaciones para conocer el estado de un servicio (problema actual del BRD) y necesita consultar estado, eventos, retrasos e incidencias (N6, US-14). Operaciones necesita indicadores históricos de tiempos, retrasos, incidencias y cumplimiento (N7, US-15). Estas lecturas no deben cargar el agregado completo ni exponer el modelo de escritura.

## Alternativas
1. Un solo modelo para escribir y consultar: simple, pero las consultas arrastran reglas e invariantes que no necesitan.
2. CQRS completo con bases separadas y sincronización por eventos: potente, pero excesivo para el alcance actual.
3. **CQRS básico**: Commands con Handler que usan el dominio; Queries con Handler que usan un puerto de lectura y devuelven DTO / View Model.

## Decisión
- Commands: `ResponderCotizacionCommand`, `GenerarOrdenDeServicioCommand`, `AsignarRecursosCommand`, `RegistrarEntregaCommand`.
- Queries: `ConsultarEstadoDelServicioQuery` → `EstadoDelServicioDTO`; `ConsultarIndicadoresQuery` → `IndicadoresDTO`, ambas vía `IConsultaDeServicios`.
- Misma base de datos; los DTO de lectura no se reutilizan para escribir.
- No se separa la modificación de cotización en proceso (US-03): es una operación simple sin lectura especializada.

## Consecuencias
- Beneficio: Atención al Cliente consulta sin depender de Operaciones ni de mensajes de WhatsApp.
- Costo: más clases (comandos, queries, DTO).
- Riesgo: inconsistencia si en el futuro se separan las bases; hoy no aplica.

## Trazabilidad
N3, N6, N7; US-14, US-15; CU-05; BPMN: T22 (consultar estado, eventos, retrasos e incidencias), T23 (consultar historial), DS1 (registro centralizado).
