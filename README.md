# Tiny Bot — Beat Buy-and-Hold on Solana

[![make.fun bounty](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/imusic-487/tiny-bot-beat-buy-hold-solana/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/imusic-487/tiny-bot-beat-buy-hold-solana/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

A tiny long-only paper-trading bot for the [Tiny Bot, Easy $2: Beat Buy-and-Hold on Solana](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1) bounty on [make.fun](https://make.fun). It reads the hourly bars of 7 Solana symbols (SOL, WETH, WIF, FARTCOIN, POPCAT, RAY, JITOSOL) and decides when to hold and when to sit in USDC, using two sleeves:

- **Dip sleeve (mean reversion):** buys a symbol whose close fell 2.5% or more over the last 6 bars, up to 24% of equity; sells at +4% from its fill price, at −5%, or after 24 bars held.
- **Rotation sleeve (momentum, kept small):** every 12 bars it ranks symbols by trailing 12-bar return and holds up to 5 of them at 5% of equity each, selling only when a symbol falls out of the top 6, and buying a newcomer only when it beats the worst held symbol by at least 1%. Cheap trade flow that keeps the bot invested in the strongest names during a rally.

## Dev-window grade (kit's own grader)

| | Net PnL | Trades | Max drawdown |
|---|---:|---:|---:|
| **This bot** | **+$274.06** | 38 | 5.41% |
| Buy-and-hold | −$869.34 | 7 | 10.92% |
| Best house baseline (macd-cci) | −$20.22 | 17 | 2.46% |

All guardrails pass (≥8 trades, ≤25% drawdown), and the score stays positive (+$206.96) at double fees. The final window is 3 fresh days of hourly bars after the deadline, so the dev grade is a guide, not a promise.

## Run it

The bot speaks the `agent-fun/bars-replay/v1` line protocol on stdin/stdout via the vendored SDK (`vendor/makefun-bars.ts`, a byte-identical copy of the kit's `sdk/makefun-bars.ts`). Grade it with the kit:

```sh
node cli/makefun.mjs grade-local --kit <kit-dir> --bot . --driver docker
```

Requires Node 22.18+ (the grader runs Node 22.23.4). No dependencies, no network, no secrets — everything the bot runs is in this repository.

## License

MIT — see [LICENSE](LICENSE).
