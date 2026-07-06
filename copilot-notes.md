# PDT Command Hub — live copilot field notes
<!-- Edit this file on GitHub to teach every installed copilot immediately (no release needed).
     Apps fetch it at chat start, cache 15 min, cap 8000 chars. Keep it tight: gotchas and
     workflow tips only. Last updated: 2026-07-05 -->

## Pine Script strategy gotchas
- ALWAYS set `margin_long=0, margin_short=0` in `strategy()` for futures or any expensive symbol.
  Pine v6 defaults both to 100%, so when notional value exceeds initial_capital every
  strategy.entry() is silently rejected — the tester shows "This report requires trade data"
  with zero trades and no error. (1 MNQ ≈ $60k notional vs default $25k capital.)
- If a strategy shows zero trades and the logic looks right, add debug counters first
  (table.new with: bars seen / gate condition hits / entry calls / strategy.closedtrades).
  entry calls > 0 with closed trades = 0 means execution-layer rejection (margin, qty, session),
  not entry logic.
- Strategy shorttitle must be 10 characters or fewer or the compile fails.

## Tool availability
- Only the mcp__tradingview tools and Read are available in the Hub. Do NOT attempt Skill,
  Task, Bash, Write, Edit, WebSearch, or WebFetch — they are always denied here and the
  denial wastes a turn. If a capability seems missing, accomplish it with TradingView tools
  (ui_evaluate covers advanced cases) or tell the user it isn't available in the Hub.

## Editor / chart workflow
- Do NOT use pine_new — in a narrow docked Pine editor it reports success without creating a
  script, and your next pine_set_source will OVERWRITE whatever script is open. To create a new
  script: click the script-name dropdown in the Pine editor header, then "Create new" →
  Indicator/Strategy. (If you accidentally overwrite a user's script, TradingView's
  "Version history…" in that same menu can restore it — tell the user.)
- A compiled script is NOT on the chart until "Add to chart" is clicked (ui_click by="text"
  value="Add to chart"; it becomes "Update on chart" once the script is on the chart).
- Saving a script that is already on the chart auto-updates the chart copy — no separate
  update click needed.
- Backtest results (data_get_strategy_results / data_get_trades) exist only for the strategy
  that is ON the chart, and only after it has made at least one trade.
