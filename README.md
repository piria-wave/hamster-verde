# hamster-verde

This repository is a public, append-only log of AI trading model outputs.

Signals are model outputs only and are **not trading advice**.

The purpose of this repository is transparency.
It records what the model produced and when it produced it.

## Overview

The repository contains several independent logs, each representing a different layer of the system:

1. Real-time model outputs (every 15 minutes)
2. Executed trades (closed positions)
3. Periodic summary reports (weekly, monthly)

Each log has its own update frequency and level of aggregation.
Primary logs are never rewritten or backfilled.

---

## 1. Model outputs (real-time)

The model produces an output every 15 minutes.
It is trained to produce two tokens (signals): `long` and `short`.

All logs in this repository currently refer to the **1000SHIBUSDT** trading pair.

Each signal has an associated probability.
Only signals with probabilities above a predefined threshold are executed.
If both signals are below the threshold, the model takes no action.

By design and training process, the model can open multiple positions simultaneously.
New signals do not affect already opened positions.

A position can be closed in one of three ways:
- take profit
- stop loss
- time elapsed

Model output logs follow these rules:

- Each output is appended as a single line
- Lines are written in chronological order
- One file is created per day
- A new file starts at the beginning of each day

These logs represent raw model signals.
They are published automatically, without manual intervention.

If needed, model performance can be independently reconstructed and verified from this raw log.

---

## 2. Trade logs (closed positions)

Trade logs record completed trades for a given day.

- One file per trading day
- Written once, after all positions from that day are closed
- Typically published a few hours after the start of the next day
- Append-only by design and not updated after publication
- Derived from model outputs and price data

These logs represent finalized daily results.
They provide a readable summary of executed trades,
while remaining independently verifiable from the raw model output logs.

---

## 3. Reports (aggregated summaries)

Reports provide aggregated views over fixed periods (week, month).

- Derived from lower-level logs
- May be recalculated
- Not considered a source of truth
- Human-readable

All summary data can be independently reconstructed from the underlying logs.

---

## Structure
```
logs/
  signals/
    YYYY/MM/
      YYYY-MM-DD.log
  trades/
    YYYY/MM/
      YYYY-MM-DD.log   # written once, finalized
reports/
  weekly/
  monthly/
```
