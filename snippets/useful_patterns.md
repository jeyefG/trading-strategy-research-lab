# Useful Patterns reutilizables (prioridad alta)

## 1) Break-retest
- Patrón propuesto para lab (no extraído literal de un repo específico):
  - Detectar ruptura de nivel intradía (high/low sesión o rango previo).
  - Esperar retest con rechazo (vela + volumen/tape proxy).
  - Entrada en continuación solo si spread/slippage dentro de umbral.
- Reuso principal: US500/XAU con executor por estados.

## 2) Momentum fade
- Idea utilizable:
  - Después de impulso extremo de corto plazo, buscar agotamiento para mean-revert parcial.
  - Necesita filtro de régimen para evitar hacer fade en tendencia fuerte.
- Relevancia: más para US500 intradía que para HFT puro.

## 3) VWAP bounce
- Idea utilizable:
  - Operar rebotes/rechazos alrededor de VWAP diaria como ancla de valor intradía.
  - Confirmar con pendiente de VWAP o distancia-z para evitar ruido.
- No aparece como estrategia explícita en los 3 repos altos, pero encaja bien en cheap lab.

## 4) Mean reversion
- Desde los repos revisados:
  - El grid/MM de hftbacktest es una forma de captura de reversion microestructural alrededor del mid.
- Versión portable:
  - Reversion de desviación respecto a referencia (VWAP/EMA/rango) + salida por target corto + time stop.

## 5) Execution / risk patterns
- **State machine de órdenes** (ejemplo Alpaca):
  - estados explícitos por símbolo
  - transición por eventos de barra y fills
- **Timeout de orden de entrada** (Alpaca): cancelar señal si no llena a tiempo.
- **Immediate exit placement tras fill** (Alpaca): reducir exposición no gestionada.
- **Inventory / exposure caps** (hftbacktest): no exceder net exposure.
- **Cancel/replace disciplinado** (hftbacktest): mantener sólo órdenes válidas para el estado actual.
- **Risk component desacoplado** (barter-rs): risk manager separado de strategy.

## 6) Event-driven loop ideas
- Un loop central de eventos + job periódico:
  - eventos de mercado
  - eventos de órdenes/ejecución
  - checkup cada N segundos (salud, horario, órdenes huérfanas)
- Comandos externos al engine (barter-rs idea):
  - enable/disable trading
  - close positions
  - cancel all
- Auditoría estructurada (barter-rs idea): stream de eventos para réplica de estado y monitoreo externo.

## Nota de aplicación
- En MT5/XAU/US500, portar **patrones de ejecución y control**, no copiar supuestos HFT de microestructura crypto.
