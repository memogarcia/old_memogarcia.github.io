---
layout: post
title:  "Understanding Blockchains"
date:   2017-06-06 12:30:00
categories: blockchain
---

## What is a blockchain?

A blockchain is a decentralized ledger that stores transactions.

### Transactions
Transactions are atomic events approved by the ledger, which means that an invalid transaction will never get stored in the blockchain as blocks.

### Blocks

Blocks are data structures stored in sequence (in a chain) that stores a certain amount of transactions.

```bash
  block 1
    ref block 0
    transaction 1
    transaction 2

  block 2
    ref block 1
    transaction 3
    transaction 4
```

### Fork

A new block holds a reference to its previous block and it is possible for more than one block to claim to be the new successor in which case a **fork** occurs.

When a fork occurs the blockchain agrees upon the right block the be added to the chain through a consensus.

### Consensus

Consensus can be public or by a consortium.
in a public blockchain everyone can participate and agree upon the next block, in a consortium consensus only a trusted set of nodes are allowed to agree on the next block.
