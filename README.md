# algo-bt visualizer

Single-file lightweight-charts viewer for algo-engine replay runs.
Drop in candles + fast/slow MA overlay + per-trade entry/exit markers,
filter and sort the trade table, click a row to pan the chart.

## Use (modern path, post-restructure)

1. Replay a `.bar.bin` + `.tick.bin` pair through the new strategy
   + OptimisticLimitFillModel via `runXovdV1.exe`:
   ```
   cd C:/Develop/gh/algo-engine
   build/backtester/runXovdV1.exe \
       --bars  data/local/MNQ_2022_01.bar.bin \
       --ticks data/local/MNQ_2022_01.tick.bin \
       --out   data/local/MNQ_2022_01.events.jsonl
   ```
2. Compose the viz JSON:
   ```
   python tools/build_visualizer_data.py \
       --bars   data/local/MNQ_2022_01.bar.bin \
       --events data/local/MNQ_2022_01.events.jsonl \
       --out    data/local/MNQ_2022_01.viz.json
   ```
3. Open `index.html` in any modern browser (Chrome/Edge/Firefox).
4. Drag-drop the `.viz.json` onto the page (or use the file picker
   in the header).

## Use (legacy path, pre-restructure exports)

`build_visualizer_data.py` still accepts `--indicators X.csv --trades
X.kernel.json` for old runs from the deleted `xovd_v1_export.cpp`.
That path is unchanged; see the script's `--help`.

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
