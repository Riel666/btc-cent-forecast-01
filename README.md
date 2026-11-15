# btc-cent-forecast-01

[![make.fun · btc-cent-forecast-01](https://make.fun/badge/btc-cent-forecast-01.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![rank](https://make.fun/badge/btc-cent-forecast-01/Riel666/btc-cent-forecast-01/rank.svg)](https://make.fun/bounty/btc-cent-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)

Entry for make.fun's **btc-cent-forecast-01**: forecast Bitcoin's close
240 minutes after each checkpoint from the 4,320 one-minute closes before
it. `main.py` (Python 3.13, standard library only) reads every case in
`/input` and writes one `{"forecasts":[{"case": id, "price": x}]}` object
per case to `/output`.

## The model

Persistence, chosen by measurement. Before settling on it, the obvious
candidate signals were backtested over 46,080 one-minute BTC closes
(30 days, 346 rolling checkpoints) and over the competition's practice
cases:

| candidate | MAE, liquid series | MAE, practice cases |
| --- | --- | --- |
| persistence (last close) | $457.32 | $470.55 |
| median of last 15 closes | worse | $538.41 |
| shrunk momentum / mean reversion | worse | worse |
| spike filters | worse | $503.59 |

Four-hour BTC moves are, to everything a case file can show, a centred
random walk; under mean absolute error the optimal forecast of a centred
walk is its last value, the conditional median. This program computes
exactly that from the case file, keeps the price on the recording's
5-decimal grid, and skips (rather than guesses at) any case it cannot
read, which the rules score as the checkpoint close anyway.

Deterministic: name order, no randomness, no clock, no network.
