# Strategy Card — nkaz001/hftbacktest

## 1. Resumen
- Repo: nkaz001/hftbacktest
- URL: https://github.com/nkaz001/hftbacktest
- Estado aparente: serio (framework + ejemplos + docs técnicas)
- Mercado: crypto (principalmente en ejemplos), multi-activo a nivel framework
- Venue/broker: Binance Futures, Bybit (mencionado en README)
- Lenguaje: Rust + Python (Numba para research/backtest)

## 2. Tipo de estrategia
- Principalmente **framework de backtesting/ejecución HFT**, no una sola estrategia.
- Estrategias visibles en ejemplos/docs: market making, high-frequency grid trading, alpha-based MM (order book imbalance/APT), multi-market making.

## 3. Entry logic
- No hay una única regla canónica del repo.
- En el snippet de market making del README: entrada por cotización pasiva bid/ask alrededor de reservation price, con `forecast`, `volatility`, `risk`, `half_spread`.
- En `examples/High-Frequency Grid Trading.ipynb`: entrada por malla de órdenes límite alrededor de mid-price (grid fijo, múltiples niveles).
- Trigger final: ciclo temporal de alta frecuencia (`hbt.elapse(...)`) + actualización de órdenes activas según estado de libro/posición.
- ¿Usa contexto?: sí, microestructura (depth, posición, latencia, cola).
- ¿Usa order book / velas / indicadores / ML?: order book L2/L3 y ticks (fuerte). Velas no son núcleo.

## 4. Exit logic
- En MM/grid, salida por fills de quotes opuestas y/o rebalance por límites de inventario.
- Cancel/replace continuo cuando una orden ya no pertenece a la grilla/quote objetivo.
- En ejemplo README: cancela órdenes que no cumplen nuevo precio objetivo y respeta límites de inventario (max notional position).
- TP/SL clásicos: **no explícitos** como módulo universal en los ejemplos base mostrados.

## 5. Risk logic
- Risk por inventario/posición explícito en ejemplos (`risk` dependiente de posición, límites de exposición, `max_position`/`max_notional_position`).
- Modelado explícito de latencia de feed/orden y queue position para fill simulation.
- Fees/rebates modelables (ejemplo grid usa maker rebate/taker fee).
- Drawdown/circuit breaker: no identificado como regla estándar única de estrategia en el material revisado.

## 6. Timeframe / data
- TF: tick-by-tick / order book event-driven (ns-level loops en ejemplos).
- Datos: L2/L3, trades tick, snapshots + streams.
- Requiere datos especiales: sí (profundidad + latencia, idealmente históricos de microestructura).

## 7. Supuestos de ejecución
- Muy exchange/microstructure-specific.
- Supone relevancia de latencia y queue position.
- Modela slippage/fills con detalle de cola y latencia.
- Adecuado para estrategias de maker/market making HFT más que para swing simple.

## 8. Evidencia real
- Evidencia técnica fuerte de framework real (simulador detallado, ejemplos y notebooks).
- Hay resultados de backtest visibles en notebooks (ej. tabla de métricas en High-Frequency Grid Trading).
- No revisado aquí: validación out-of-sample independiente o track live auditado.
- Clasificación de evidencia: **real y útil (research infra)**, no “README-only”.

## 9. Qué rescataría para nuestro caso
- para XAU:
  - Arquitectura de simulación con fricción realista (latencia/slippage/cola) adaptada a CFDs/futuros.
  - Gestión de inventario y re-cotización defensiva.
- para US500:
  - Motor event-driven y plantillas de risk por exposición neta.
  - Ideas de grid/MM solo si se traduce a microestructura de índice/futuro (no copiar literal crypto).
- para SE/executor layer:
  - Muy valioso: separación señal vs ejecución, lifecycle de órdenes, cancel/replace robusto, simulación realista.
- para MT5:
  - Conceptos reutilizables, implementación directa limitada (MT5 no expone microestructura L2/L3 equivalente en muchos brokers retail).

## 10. Qué NO rescataría
- Dependencia fuerte en rebate structure crypto para justificar edge de grid/MM HFT.
- Supuestos de fill/cola difíciles de reproducir en MT5 retail.
- Cualquier performance number sin recalibración al venue objetivo.

## Clasificación rápida
- **Útil sobre todo por arquitectura + ideas cuant de ejecución.**
- Para estrategia plug-and-play en XAU/US500 retail: utilidad parcial.
