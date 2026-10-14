# ADR-002: Repository en términos del dominio y Unit of Work por caso de uso

**Estado:** Aceptada (la tecnología de base de datos sigue sin definirse; hoy hay implementación en memoria)

## Contexto
El BRD exige centralizar la información del servicio (N1) y conservar un historial de cambios (N2, N4). Un caso de uso como "asignar unidad y operador" modifica la orden y debe confirmarse o revertirse completo. El BRD no define la tecnología de persistencia.

## Alternativas
1. Que los servicios de aplicación ejecuten consultas SQL directamente: rápido, pero acopla aplicación y dominio a la base de datos y rompe DIP.
2. Un repositorio genérico CRUD por tabla: expone la estructura de datos, no necesidades del dominio.
3. **Repository por agregado/entidad con Unit of Work**: interfaces con lenguaje del dominio; la implementación se aísla en infraestructura.

## Decisión
Se definen `ICotizacionRepository`, `IOrdenServicioRepository`, `IUnidadRepository` e `IOperadorRepository` (métodos `obtenerPorId` y `guardar`) en el dominio, e `IUnitOfWork` en la aplicación. La transacción inicia al comenzar el `handle` del caso de uso y termina con `confirmar()`; ante un error de dominio se llama `revertir()` y no se publica ningún evento. Las implementaciones `EnMemoria...` (`EnMemoriaUnitOfWork`, `EnMemoria*Repository`, `EnMemoriaConsultaDeServicios`) viven en infraestructura y simulan la transacción con una instantánea que se restaura si no se confirma.

## Consecuencias
- Beneficio: el dominio no conoce SQL ni ORM; las pruebas usan repositorios en memoria.
- Costo: una capa más de código.
- Riesgo: la implementación en memoria no persiste; cuando el negocio defina la tecnología solo se reemplaza infraestructura.
- Trabajo posterior: decidir cómo se almacenan los archivos de evidencia (pendiente de conservación en el BRD).

## Trazabilidad
N1, N2, N4; US-05, US-08, US-10; restricción del BRD "las evidencias deben permanecer asociadas al servicio"; secuencias 03a y 03b (inicio, guardar, confirmar y revertir); pruebas `test_unit_of_work_revierte_si_no_se_confirma` y `test_cu03_operador_no_disponible_no_cambia_nada`.
