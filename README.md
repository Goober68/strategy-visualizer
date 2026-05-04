# algo-bt visualizer

Single-file lightweight-charts viewer for algo-backtester runs.
Drop in candles + fast/slow MA overlay + per-trade entry/exit markers,
filter and sort the trade table, click a row to pan the chart.

## Use

1. Generate the composite JSON from a sweep/export run:
   ```
   cd C:/Develop/gh/algo-backtester
   python python/build_visualizer_data.py \
       --bars       data/local/MNQ_2022_01.bar.bin \
       --indicators _Jan_lds.kernel_indicators.csv \
       --trades     _Jan_lds.kernel.json \
       --out        data/local/MNQ_2022_01.viz.json
   ```
2. Open `index.html` in any modern browser (Chrome/Edge/Firefox).
3. Drag-drop the `.viz.json` onto the page (or use the file picker
   in the header).

## Schema (`.viz.json`)

```jsonc
{
  "meta":       {"symbol": "MNQ_DB_TT", "bar_period_sec": 180, ...},
  "aggregate":  {"trades": 247, "profit_trades": 178, "total_profit": 3438, "max_dd": 104, ...},
  "bars":       [{"time": 1641164580, "open": 16386.0, "high": ..., "low": ..., "close": ..., "volume": ...}, ...],
  "indicators": [{"time": 1641164580, "fast_ma": 0, "slow_ma": 0, "atr": 0, "state": "XS_NONE"}, ...],
  "trades":     [{"id": 0, "comment": "XOVD_178", "dir": "LONG", "size": 4,
                  "entry_time": 1641196345, "entry_price": 16395.78, "sl_price": ..., "tp_price": ...,
                  "exit_time":  1641196391, "exit_price":  16398.53, "exit_reason": "SL", "profit": -44.0}, ...]
}
```

## Markers

- **Entry**: ▲ (long) / ▼ (short), labeled `<DIR><SIZE>@<PRICE>` e.g. `L4@16395.78`.
- **Exit**: ● colored by reason — green TP, red SL, amber TRAIL, gray for NONE/EOSTREAM/etc.

## Stack

- [lightweight-charts](https://github.com/tradingview/lightweight-charts) v4.2 (CDN, MIT).
- Vanilla HTML/CSS/JS. No build step. No backend.
