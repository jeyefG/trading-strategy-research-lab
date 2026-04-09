# Ideas para US500 cheap lab (desde repos de prioridad alta)

## Potencialmente útiles
- **State machine de órdenes simple y robusta** (TO_BUY / BUY_SUBMITTED / TO_SELL / SELL_SUBMITTED) para reducir ambigüedad de ejecución y facilitar logs/debug.
- **Timeout explícito de entradas** (cancelar orden si no llena en X minutos) para evitar fills tardíos fuera del contexto de señal.
- **Cierre de riesgo por horario/sesión** (hard flatten pre-cierre) en versiones iniciales del lab barato.
- **Separación señal vs ejecución**: señal puede ser barata (ej. ruptura/retest en M1-M5), pero executor con disciplina de cancel/replace y reglas de exposición.
- **Controles de exposición por inventario/notional** inspirados en MM/HFT, aunque simplificados para índice.
- **Loop event-driven** con tareas periódicas de checkup (mercado abierto, órdenes huérfanas, health checks).

## Ideas a descartar (o dejar para fase avanzada)
- **Grid/MM HFT dependiente de rebates de crypto** como edge principal: difícil transferir directo a US500 retail/CFD.
- **Suposiciones de microestructura L2/L3 y queue position exacta** en brokers donde esa visibilidad no existe.
- **Confiar en señal SMA20 de ejemplo como alpha final** sin validación robusta multi-régimen.
- **Complejidad extrema de simulación HFT** en fase cheap lab inicial (mejor versión frugal primero).

## Priorización sugerida (cheap lab)
1. Executor layer limpio (estados + timeouts + EOD flatten).
2. 1–2 señales simples con hipótesis clara (break-retest / mean reversion intradía).
3. Métricas mínimas reproducibles (expectancy, hit rate, max DD, latency/fill slippage básico).
4. Luego iterar con mejores modelos de ejecución.
