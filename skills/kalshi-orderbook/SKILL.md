---
name: kalshi-orderbook
description: Kalshi orderbook semantics (bids-only) and correct sweep/taker logic. Use when implementing or debugging orderbook-based trading tools (e.g., market_orderbook_sweep), interpreting orderbook.yes/no arrays, converting bids ↔ implied asks (price inversion), or when fills are unexpectedly zero due to taking the wrong side of the book.
---

# Kalshi orderbook semantics (critical)

## Core fact
Kalshi `GET /markets/{ticker}/orderbook` returns **BIDS ONLY**:
- `orderbook.yes` = YES bids
- `orderbook.no` = NO bids
- There are **no asks** in the payload.

Docs: Kalshi explains that in a binary market, a bid on one side implies an ask on the other side.

## Price inversion rule
A bid at `X` cents implies an ask at `100 - X` cents on the opposite side (same size).

So for **taker/sweep** logic (buying immediately):

- To **BUY YES**, you must consume implied YES asks derived from **NO bids**:
  - `yesAskPrice = 100 - noBidPrice`

- To **BUY NO**, you must consume implied NO asks derived from **YES bids**:
  - `noAskPrice = 100 - yesBidPrice`

## Common failure mode
If you incorrectly treat `orderbook.yes/no` as asks and sweep them directly, you’ll submit IOC buys at prices where there is no sell liquidity, causing:
- many orders with `status: canceled`
- `fill_count: 0`

Example:
- Seeing `orderbook.no` level at `1¢` means **someone is bidding 1¢ to buy NO**.
- That corresponds to a **YES ask at 99¢** (expensive), not “NO available for 1¢”.

## Implementation checklist
When building sweep/IOC logic:
1) Convert bids → implied asks using price inversion.
2) Sort implied asks by **lowest price first** (best ask).
3) Sweep asks up to `maxPrice`.
4) Cap batch size to Kalshi limits (often 20 orders).

## Where this lives in code
- kalshi-toolkit: `tools/market/orderbook/sweep/plan.ts` should provide a helper like `deriveTakerAsksFromBids()`.
- Sweeper tools should use derived asks (not raw orderbook arrays) before planning orders.
