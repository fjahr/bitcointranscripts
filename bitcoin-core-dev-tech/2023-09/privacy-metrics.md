---
title: Privacy Metrics for Coin Selection
categories: ['core-dev-tech']
tags: ['bitcoin core', 'privacy', 'coin selection']
date: 2023-09-20
speakers: [Mark Erhardt]
---


- Goal: Get privacy consciousness into coin selection
- Configurability
  - Privacy vs cost (waste)
  - Privacy: weighted on a 0-5 scale
  - Cost: weighted on a 0-5 scale
- Convert privacy preference (0-5) into satoshis to make it compatible with the waste score
  - Combined score = PrivacyScoreWeight x PrivacyScore + CostWeight x WasteMetric
  - 20-30 sats per privacy point as a gut feeling
- Privacy score example: sending to different script type than inputs of transaction
  - We already match the change type to the recipient type, but that can still mean that we have differently typed inputs than outputs
  - If we have two input sets, where one has the same type and the other has a different type, the one with the same type inputs should be preferred by the privacy metric
- Privacy score example: Preferring spending UTXOs of similar age
  - E.g. spending a UTXO received two years ago and one received the same week may leak more information than two UTXOs received two years ago
  - The timeframe in which someone spends received coins could be a privacy leak
  - Right now, given current 12 input sets and variability of coin selection, might not be a privacy concern
- Need to implement privacy heuristics
  - Which requires more information about UTXOs
- 24 potential ideas for metrics so far
  - Start with just 3 for first implementation
  - Inspired by prior work by Bitcoin Privacy Wiki, Wasabi, LaurentMT
  - There are considerations for wallet implementation
  - Tx entropy metric (Samourai)
    - Number of interpretations the transaction could have
  - At some point get a group together to go through the criteria to get feedback on degree of privacy impact for each
- Could use wallet help in terms of pulling out data here and there
- Interplay between APS (avoid partial spends) and privacy scoring
  - Perhaps turn off APS with a privacy score of 0
    - Potential confusion from users ("I have privacy at 0, but APS still on by default")
  - Configuration options that have overlapping areas of concern
- Future
  - Wallet Health Metric (also 0-5)
