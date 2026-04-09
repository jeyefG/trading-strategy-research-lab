# Strategy Card — barter-rs/barter-rs

## 1. Resumen
- Repo: barter-rs/barter-rs
- URL: https://github.com/barter-rs/barter-rs
- Estado aparente: serio como ecosistema/engine; estrategia por defecto es demostrativa
- Mercado: multi-venue (arquitectura general)
- Venue/broker: integraciones múltiples (vía crates del ecosistema)
- Lenguaje: Rust

## 2. Tipo de estrategia
- Principalmente **framework/engine modular** (strategy + risk manager plug-and-play).
- El `DefaultStrategy` documentado es **naive/demo** y no genera órdenes algorítmicas.

## 3. Entry logic
- En `DefaultStrategy`: no genera entradas (`generate_algo_orders` sin señales reales).
- El repo está pensado para que el usuario conecte su propia estrategia.
- ¿Usa contexto?: a nivel engine sí (state management O(1), audit stream), pero no una regla alpha por defecto.
- ¿Usa order book / velas / indicadores / ML?: configurable vía módulos; no hay alpha canónica en el core revisado.

## 4. Exit logic
- `DefaultStrategy` sí contempla cierre naive de posiciones con market orders (según docs).
- También hay comandos de engine para cancelar órdenes y cerrar posiciones.
- TP/SL/trailing/time stop: no definidos como regla de estrategia estándar en el default.

## 5. Risk logic
- Arquitectura incluye componente `RiskManager` conectable.
- En default/demo no se observa lógica de riesgo sofisticada de alpha.
- Riesgo real depende de implementación concreta del usuario.

## 6. Timeframe / data
- Event-driven, adaptable a live/paper/backtest.
- Puede consumir trades y orderbook L1, según README y ejemplos de sistema.
- Timeframe de señal: no fijo en el core.

## 7. Supuestos de ejecución
- Foco en performance, concurrencia, robustez tipada.
- Diseñado para múltiples venues y ejecución live/mock.
- Supuestos concretos de slippage/latency dependen de módulos elegidos.

## 8. Evidencia real
- Evidencia fuerte de arquitectura de software (crates, estado, auditoría, comandos).
- README incluye disclaimer legal explícito: uso educativo/investigación, no producción garantizada.
- Evidencia de edge de estrategia: no (porque no trae alpha “ganador” por defecto).
- Clasificación de evidencia: **real y útil para arquitectura**, no para señal lista.

## 9. Qué rescataría para nuestro caso
- para XAU:
  - Contratos limpios entre estrategia/riesgo/ejecución.
- para US500:
  - Estructura para cheap lab robusto con replay/paper/live bajo una misma interfaz.
- para SE/executor layer:
  - Muy alto valor: state engine indexado, audit stream, comandos externos, control de trading state.
- para MT5:
  - Reutilizable en diseño (capas, eventos, auditoría), no directamente plug-and-play por stack diferente.

## 10. Qué NO rescataría
- `DefaultStrategy` como base de alpha (no genera entradas reales).
- Cualquier expectativa de profitability por usar el framework sin research de señal.

## Clasificación rápida
- **Útil solo por arquitectura** (muy útil para SE/executor).
