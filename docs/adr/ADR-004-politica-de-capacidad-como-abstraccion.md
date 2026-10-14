# ADR-004: Capacidad de la unidad mediante la abstracción PoliticaDeCapacidad

**Estado:** Propuesta (depende de validar las políticas reales con el stakeholder)

## Contexto
El stakeholder confirmó que la unidad debe tener capacidad suficiente y que deben considerarse las características especiales de la mercancía, pero indicó que los límites "tendrían que definirse según las políticas y capacidades de la empresa". El BRD prohíbe inventar valores.

## Alternativas
1. Codificar límites fijos en el servicio de aplicación: inventa reglas y mezcla negocio con coordinación.
2. Condicionales dentro de `Unidad`: cada política nueva obliga a editar la clase (viola abierto/cerrado).
3. **Interfaz `PoliticaDeCapacidad` en el dominio**, usada por el servicio de dominio `AsignadorDeRecursos`.

## Decisión
Se define `PoliticaDeCapacidad.permite(unidad, mercancia)` y `PoliticaDeCapacidadEmpresa`, que solo aplica la regla confirmada ("la unidad debe tener capacidad suficiente": la carga no supera la capacidad registrada de la unidad). No hay límites propios de la empresa; lo demás queda pendiente. `AsignadorDeRecursos` lanza `RecursosNoDisponibles` si la unidad no es adecuada o el operador no está disponible.

## Consecuencias
- Beneficio: cuando existan las políticas reales solo se completa una implementación; los controles de carga de alto valor (N8) podrán añadirse como otra política.
- Costo: una abstracción que hoy tiene una sola implementación.
- Trabajo posterior: validar el supuesto S-01 (¿existe un tope propio de la empresa?); si existe se configura en `PoliticaDeCapacidadEmpresa`. Los controles de alto valor se resolvieron aparte (S-02, ADR-006).

## Trazabilidad
N8; US-07, US-08, US-16; restricciones del BRD sobre capacidad; BPMN: G2, T6, T7; secuencia 03a; pruebas `test_cu03_unidad_sin_capacidad_suficiente_se_rechaza` y `test_cu03_operador_no_disponible_no_cambia_nada`.
