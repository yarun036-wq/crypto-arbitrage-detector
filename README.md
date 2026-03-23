# Crypto Arbitrage Detector

A low-latency C++ project that compares crypto quotes across exchanges in different countries, converts every venue into USD, adjusts for trading costs, and identifies the best cross-exchange arbitrage opportunity.

This project is intentionally focused on the decision engine, not brokerage connectivity. The main goal is to show low-latency market-data normalization, cost-aware venue comparison, and fast opportunity detection.

## Problem Statement

The same crypto asset can trade at different prices on different venues. Raw prices are not directly comparable because:

- exchanges use different quote currencies
- trading fees differ
- slippage matters
- fixed execution costs matter

This engine solves that by converting everything into a common USD basis and comparing true executable buy and sell values.

## Core Features

- Multi-exchange quote ingestion
- USD normalization for `USD`, `EUR`, `JPY`, `KRW`, and `USDT`
- Fee and slippage adjustment
- Best buy venue / best sell venue selection
- Spread reporting in both USD and basis points
- Synthetic low-latency benchmark
- Correctness tests for normalization and opportunity ranking

## Example Workflow

1. Receive top-of-book snapshots from several exchanges
2. Convert each venue quote into USD
3. Compute effective buy price and effective sell price
4. Compare all venues for the same instrument
5. Surface the highest positive net spread after costs

## Project Layout

- `include/arb/crypto_arb.hpp`
  Public detector API and data model
- `include/lob/latency_histogram.hpp`
  Latency histogram helper used by the benchmark
- `src/crypto_arb.cpp`
  Quote normalization and opportunity engine
- `src/crypto_arb_main.cpp`
  Demo and benchmark executable
- `tests/crypto_arb_tests.cpp`
  Detector tests

## Architecture

```text
Exchange Quotes
(US, Europe, Japan, Korea, Global)
             |
             v
   +------------------------+
   | Quote Ingestion Layer  |
   +------------------------+
             |
             v
   +------------------------+
   | FX Normalization       |
   | convert all to USD     |
   +------------------------+
             |
             v
   +------------------------+
   | Cost Adjustment        |
   | fees + slippage +      |
   | fixed execution cost   |
   +------------------------+
             |
             v
   +------------------------+
   | Opportunity Detector   |
   | best buy / best sell   |
   +------------------------+
             |
      +------+------+
      |             |
      v             v
+-------------+ +----------------+
| Bench Output | | Arbitrage      |
| Latency      | | Opportunity    |
| p50/p99/p999 | | USD + bps      |
+-------------+ +----------------+
```

## Build

```bash
cmake -S . -B build
cmake --build build --config Release
```

Windows helper:

```bat
build_windows.bat
```

## Run

```bash
./build/crypto_arb_bench
./build/crypto_arb_bench 1000000
./build/crypto_arb_tests
```

## Example Output

```text
USD-normalized arbitrage opportunities
  BTC/USD: buy on Upbit (South Korea) at $..., sell on bitFlyer (Japan) at $..., spread $...
```

## Interview Summary

Short version:

> Built a low-latency C++ crypto arbitrage detector that normalizes global exchange quotes into USD, adjusts for fees/slippage, and ranks the best cross-exchange opportunities.

## Next Improvements

- Add live WebSocket adapters for Binance, Coinbase, Kraken, and Bybit
- Add stale-quote filtering and latency-risk scoring
- Add paper-trading execution simulation
- Add persistent market-data replay
- Add transfer and settlement constraints to reduce false positives
