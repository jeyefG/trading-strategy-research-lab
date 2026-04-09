# Ideas para XAU live lab (desde repos de prioridad alta)

## Potencialmente útiles
- **Executor-first mindset**: la ventaja puede perderse por ejecución; adoptar controles de orden (timeouts, reintentos, cancel/replace) antes de sofisticar alpha.
- **Gestión de inventario/exposición** aplicada a XAU: límite de tamaño neto, límite por señal, y reglas de no sobre-acumulación.
- **Máquina de estados explícita** para robustez operativa en live:
  - idle -> entry_submitted -> in_position -> exit_submitted -> flat
- **Kill-switches operativos**:
  - flatten por ventana horaria de alto riesgo
  - suspensión por slippage anómalo
  - suspensión por rechazos consecutivos
- **Audit trail/event log** al estilo engine (clave para postmortem en live).

## Precauciones específicas para XAU
- Muchas ideas HFT de crypto dependen de profundidad/latencia/cola no observable en MT5 retail.
- Evitar extrapolar performance de notebooks a XAU live sin recalibrar spread variable, horarios y news shocks.
- Señales demasiado simples (SMA crossover demo) no deben pasar a live sin stress-test.

## Quick wins recomendados
1. Implementar executor layer confiable y medible.
2. Arrancar con una señal simple pero con filtros de contexto (sesión, spread, volatilidad mínima/máxima).
3. Medir calidad de fill y desviación vs precio teórico por operación.
4. Escalar complejidad solo si la base operativa es estable.
