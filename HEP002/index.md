---
label: HEP002
description: Brief description here...
date: 2025-08-17
tags:
  - draft
authors:
  - name: Steven Varga
    affiliation: Varga Consulting
  - name: Gerd Heber
    affiliation: The HDF Group
---

(hep002-title)=
# HEP002: Tick Data in HDF5

## Introduction

This HEP (HDF5 Enhancement Proposal) owes its existence to Steven Varga's release of his [IEX2H5 tool](https://github.com/vargaconsulting/iex2h5), and the HDF5 profile he adopted for this tool.

## HDF5 Profile

### Root-level datasets

- `/time.txt` - Regular time index for RTS data
- `/instruments.txt` - List of trading symbols/instruments
- `/trading_days.txt` - Trading day index

### IRTS (Irregular Time Series) datasets

- `/irts/YYYY-MM-DD` - Raw tick data for each trading day is stored as a compound datatype with the following fields:
  - `time` (`uint64` [nanoseconds])
  - `price` (`float32`)
  - `size` (`uint32`)
  - `contract_id` (`uint16` [instrument identifier])
  - `flags` (`uint16` [bid/trade/ask indicators])

### RTS (Regular Time Series) datasets

The RTS matrices are organized with instruments as rows and time slots as columns, using compression and chunking for efficient storage.
The IEX2H5 tool uses 1 minute as the default time interval. On a typical trading day, there are 6 hours and 30 minutes, or 390 minutes, i.e., 390 columns.    

- `/rts/ask/YYYY-MM-DD` - Ask prices at regular intervals
- `/rts/bid/YYYY-MM-DD` - Bid prices at regular intervals
- `/rts/trade/YYYY-MM-DD` - Trade prices at regular intervals
- `/rts/volume/YYYY-MM-DD` - Trade volumes at regular intervals

### Statistics datasets

- `/stats/YYYY-MM-DD/avg_trade_count` - Average trade count per interval
- `/stats/YYYY-MM-DD/first_trade` - First trade time per instrument
- `/stats/YYYY-MM-DD/last_trade` - Last trade time per instrument
- `/stats/YYYY-MM-DD/trade_count` - Total trade count per instrument
- `/stats/YYYY-MM-DD/trade_size` - Total trade size per instrument
- `/stats/YYYY-MM-DD/event_count` - Total event count per instrument

## Discussion

## References

1. IEX2H5: IEX TOPS Dataset to HDF5 Converter, https://github.com/vargaconsulting/iex2h5
2. Reuters Financial Glossary, Second Edition, Addison-Wesley Longman Ltd, 2003.
3. Romesh Vaitilingam, FT Guide to Using the Financial Pages, Sixth Edition, FT Publishing International, 2010.
4. Larry Harris, Trading and Exchanges: Market Microstructure for Practitioners, Oxford University Press, 2002.
5. Gary Stevenson, The Trading Game: A Confession, Crown Currency, 2024.
