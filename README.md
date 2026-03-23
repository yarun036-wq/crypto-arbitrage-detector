# Crypto Arbitrage Detector

This is a low-latency C++ project that compares crypto prices across exchanges in different countries, converts all quotes into USD, adjusts for fees and slippage, and finds the best buy and sell venues.

## What It Does

- ingests top-of-book quotes from multiple crypto exchanges
- normalizes local exchange prices into USD
- applies taker-fee, slippage, and fixed-cost adjustments
- detects cross-exchange arbitrage opportunities
- reports the best spread in USD and basis points
- includes a low-latency benchmark and a correctness test binary

## Layout

- `include/arb/crypto_arb.hpp`: arbitrage detector API
- `include/lob/latency_histogram.hpp`: latency histogram helper
- `src/crypto_arb.cpp`: normalization and opportunity engine
- `src/crypto_arb_main.cpp`: demo and benchmark executable
- `tests/crypto_arb_tests.cpp`: detector correctness tests

## Build

```bash
cmake -S . -B build
cmake --build build --config Release
```

On Windows:

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
BTC/USD: buy on one venue in USD-normalized terms, sell on another venue, spread after fees = ...
```

## Next Steps

- add real WebSocket adapters for Binance, Coinbase, Kraken, and Bybit
- add stale-quote and latency-risk filters
- add paper-trading execution simulation
- add persistent market data replay
