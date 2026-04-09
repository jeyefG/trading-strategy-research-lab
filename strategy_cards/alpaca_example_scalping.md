# Strategy Card — alpacahq/example-scalping

## 1. Resumen
- Repo: alpacahq/example-scalping
- URL: https://github.com/alpacahq/example-scalping
- Estado aparente: demo funcional (educativo, corto, útil para patrón de ejecución)
- Mercado: equities (US stocks)
- Venue/broker: Alpaca + Polygon stream
- Lenguaje: Python

## 2. Tipo de estrategia
- Scalping intradía long-only simplificado.
- Trigger técnico simple: cruce/condición sobre SMA de 20 minutos.
- Enfoque clave del repo: arquitectura async concurrente multi-símbolo + máquina de estados de órdenes.

## 3. Entry logic
- Señal de compra calculada al llegar cada barra de 1 minuto (`on_bar`).
- Buy signal descrito en README: lógica sobre SMA 20 minutos.
- Entrada: compra por monto fijo (`lot`) cuando estado `TO_BUY`.
- ¿Usa contexto?: sí (estado de órdenes/posición + minuto actual de sesión).
- ¿Usa order book / velas / indicadores / ML?: velas M1 + indicador SMA; no order book ni ML.

## 4. Exit logic
- Al llenar compra (`on_order_update`, fill), transiciona a `TO_SELL` y manda venta límite inmediata.
- Precio de salida límite: `max(last_trade_price, entry_price)` según README.
- Cancelación de compra si no llena en ~2 minutos.
- Liquidación forzada al final de sesión (~3:55pm ET) por market order.
- TP/SL clásico explícito: TP implícito en vender >= entry; SL duro no claro, salvo liquidación EOD.

## 5. Risk logic
- Position sizing simple por `lot` en USD.
- Riesgo principal: timeout de entrada + cierre EOD.
- Sin control sofisticado de DD o volatility regime en implementación base.
- README advierte que la venta límite puede quedar colgada y generar pérdidas.

## 6. Timeframe / data
- TF: M1 intradía.
- Datos: streaming minute bars + updates de órdenes en tiempo real.
- Requiere datos especiales: no, relativamente estándar para broker API.

## 7. Supuestos de ejecución
- Broker/API-specific (Alpaca/Polygon).
- Supone fills suficientemente rápidos para scalping.
- Slippage mitigado parcialmente con limit sell, pero no modelado de forma avanzada.
- Queue position: no.

## 8. Evidencia real
- Código real ejecutable (no solo README).
- No incluye resultados robustos de backtests walk-forward ni estadísticas largas.
- Es más una referencia de implementación que prueba de edge consistente.
- Clasificación de evidencia: **útil por implementación; evidencia de edge limitada**.

## 9. Qué rescataría para nuestro caso
- para XAU:
  - Patrón de state machine TO_BUY/BUY_SUBMITTED/TO_SELL/SELL_SUBMITTED.
  - Timeout de entrada y cierre por sesión.
- para US500:
  - Excelente base para cheap lab: señal simple + executor disciplinado + concurrencia multi-activo.
- para SE/executor layer:
  - Muy reutilizable: eventos de barra + eventos de orden + checkup periódico + liquidación/cancelación sistemática.
- para MT5:
  - Bastante portable conceptualmente (OnTick/OnTimer/OnTradeTransaction equivalentes), adaptando API.

## 10. Qué NO rescataría
- Señal SMA20 tal cual como edge final (demasiado básica).
- Dependencia directa de restricciones/reglas PDT de equities US.
- Supuesto de salida rápida sin modelar fill quality por venue.

## Clasificación rápida
- **Útil por arquitectura + patrón de ejecución.**
- Como alpha puro: **útil solo por ideas**, no por edge demostrado.
