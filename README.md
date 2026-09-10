# Interactive Brokers (IBKR) Trading Bots

Live options and futures automation against the Interactive Brokers TWS API — connection
lifecycle, multi-leg order construction, risk limits and crash recovery.

Everything here was delivered under commercial engagements and is **published with client
identifiers, runtime logs, position state and credentials removed**. No performance results
are published, and where test coverage is thin the README says so.

---

## ibkr-spx-0dte-options-bot

Same-day-expiry SPXW index options — a delta-targeted reversal strategy (initial delta 0.465,
offset 0.035, 5-point strike interval) on a one-minute evaluation loop.

Cleanly separated modules for strategy, orders, TWS connection, risk, state recovery and
Telegram alerts, with every parameter in `spx_config.json` so retuning needs no code change.
The config models the session in **both ET and IST** — it runs US market hours as a
19:00–01:30 overnight session.

**No tests.** For options automation, order and exit paths are exactly what should be covered.

https://github.com/pranay123-stack/ibkr-spx-0dte-options-bot

---

## ibkr-spx-options-engine-v13

A config-driven live SPX options engine: run loop, typed IV-statistics and risk-metric models,
operator kill switches (`close_position.py`, `close_all_positions.py`), and committed
implied-volatility series feeding the IV layer.

Ships the written developer specification (PDF) the implementation was built against.

**96 source files, 1 test.**

https://github.com/pranay123-stack/ibkr-spx-options-engine-v13

---

## ibkr-spx-options-engine-v18

The later iteration of the same engine, submitted separately. Same architecture, revised
strategy and risk handling.

Both versions are published rather than only the newest, because the diff between them *is*
the record of what changed under live feedback.

**96 source files, 1 test.**

https://github.com/pranay123-stack/ibkr-spx-options-engine-v18

---

## The part worth reading

The **auto-restart supervisor** (`run_with_autorestart.py`) distinguishes three exit
conditions — clean exit, crash, and operator Ctrl+C — and only restarts on a crash.

That distinction matters more than it looks: an options engine that blindly restarts after a
deliberate shutdown will re-enter positions the operator just closed, at whatever the market
has moved to. Getting process lifecycle wrong is a P&L bug, not an ops bug.

---

## Related

| Portfolio | Relevance |
|---|---|
| [SPX & SPY Trading Strategies](https://github.com/pranay123-stack/spx-spy-strategies) | The strategy side — 0DTE structures, credit spreads, gamma and theta handling |
| [NASDAQ Futures Trading Strategies](https://github.com/pranay123-stack/nasdaq-futures-strategies) | NQ futures work through the same TWS API |
| [ES Futures Trading Strategies](https://github.com/pranay123-stack/es-futures-strategies) | E-mini S&P execution through IBKR |
| [crypto-exchange-development](https://github.com/pranay123-stack/crypto-exchange-development) | Execution architecture and the verified-vs-self-reported testing standard |

**Tech:** Python, ib_insync, IBKR TWS API, Telegram
