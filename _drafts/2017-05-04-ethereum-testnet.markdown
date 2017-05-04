---
layout: post
title:  "Deploy an Ethereum test network"
date:   2017-05-04 12:30:00
categories: ethereum
---

### Prerequisites

* golang >= 1.7
* ubuntu >= 14.04
* docker

### Docker quickstart

One of the quickest ways to get Ethereum up and running on your machine is by using Docker:

    docker run -d --name ethereum-node -v /Users/alice/ethereum:/root -p 8545:8545 -p 30303:30303 ethereum/client-go:alpine --fast --cache=512


### Create a genesis block

    {
    "config": {
          "chainId": 0,
          "homesteadBlock": 0,
          "eip155Block": 0,
          "eip158Block": 0
      },
    "alloc": {
        "0x0000000000000000000000000000000000000001": {"balance": "111111111"},
        "0x0000000000000000000000000000000000000002": {"balance": "222222222"}
    },
    "coinbase"   : "0x0000000000000000000000000000000000000000",
    "difficulty" : "0x20000",
    "extraData"  : "",
    "gasLimit"   : "0x2fefd8",
    "nonce"      : "0x0000000000000042",
    "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp"  : "0x00"
    }
