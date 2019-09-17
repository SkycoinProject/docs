+++
title = "Coin Hours"
weight = 5
+++

<!-- MarkdownTOC autolink="true" bracket="round" levels="1,2,3,4,5,6" -->

- [Introduction](#introduction)
- [Coin Hours Overview](#coin-hours-overview)
- [Coin Hours Fee](#coin-hours-fee)
	- [Coin Hours Fee Examples](#coin-hours-fee-examples)
- [Coin Hours Use Cases](#coin-hours-use-cases)
	- [Skywire](#skywire)
	- [CX](#cx)
	- [CoinJoin](#coinjoin)

<!-- /MarkdownTOC -->

### Introduction

Unlike Bitcoin, Skycoin users don't need to give away part of their balance as
incentive from miners to process their transaction.
Instead, they use Coin Hours as fees.
A transaction requires a minimum percent of input Coin Hours to be destroyed.
This Coin Hour burn fee prevents transaction spam by ratelimiting the number of
transactions a single entity can make. Coin Hours are automatically generated,
at a rate of one Coin Hour per hour per coin. Coin Hours are similar to “Gas”
in Ethereum and NEO and act as a separate parallel currency in Skycoin.
There is no cap on the number of Coin Hours in the network.
Coin Hours provide a way to allocate scarce resources (like bandwidth or storage)
without the need to rely on the coin supply.

In future versions of the Skycoin protocol,
Coin Hours will have an exchange rate against SKY,
allowing for an optimal displacement of resources according to demand.

See [Coin Hours](https://github.com/SkycoinProject/skycoin/wiki/Coin-Hours) in the
Skycoin Github Wiki for more information.

### Coin Hours Overview

Coin Hours are earned by holding Skycoin in a wallet (holding Skycoin on
exchanges won't result in Coin Hours earnings).
Coin Hours are attached to unspent outputs (UXTOs),
and the amount is calculated as following:

- Holding `n` SKY generates `n` Coin Seconds per second
- 3600 Coin Seconds equals 1 Coin Hour
- Coin Seconds are converted to Coin Hours and rounded down (only whole Coin Hours are considered a valid unit)

Coin Hours are only recalculated when a new block is executed,
because the block timestamps are used as the network clock.
The amount of time passed is calculated as
the difference between the head block time and the time of the block in which the UXTX was created.
The local computer's time cannot be used to calculate coin hours because
system clocks are not synchronized across a decentralized network.

### Coin Hours Fee

Coin Hours' primary function is to prevent transaction spam in the network.
A transaction must destroy a minimum percent of its Coin Hours in order to
be processed by the network. This percent is configurable and currently set at 50%.
The percent is always rounded up.
A transaction must destroy at least 1 Coin Hour.

A transaction spends one or more UXTOs as inputs and creates one of more UXTOs as outputs.
The sum of Coin Hours for all inputs must be at least 1.
The sum of Coin Hours for all outputs must be less than or equal to the 50% of sum of input Coin Hours,
rounded down.

In the case of transaction congestion in the network,
transactions are prioritized by the number of coin hours burned per byte.
A transaction may optionally burn more coin hours than the minimum required
in order to increase its priority.

#### Coin Hours Fee Examples

An invalid transaction, with 0 input coin hours:

```json
{
    "inputs": [
        {
            "coins": "1.000000",
            "hours": 0,
        }
    ],
    "outputs": [
        {
            "coins": "0.500000",
            "hours": 0,
        },
        {
            "coins": "0.500000",
            "hours": 0,
        }
    ]
}
```

An invalid transaction, because it does not burn enough coin hours,
with 3 input coin hours and 2 output coin hours:

```json
{
    "inputs": [
        {
            "coins": "1.000000",
            "hours": 3,
        }
    ],
    "outputs": [
        {
            "coins": "0.500000",
            "hours": 1,
        },
        {
            "coins": "0.500000",
            "hours": 1,
        }
    ]
}
```

A valid transaction, with 5 input coin hours and 2 output coin hours:

```json
{
    "inputs": [
        {
            "coins": "1.000000",
            "hours": 5,
        },
        {
            "coins": "1.000000",
            "hours": 0,
        }
    ],
    "outputs": [
        {
            "coins": "1.500000",
            "hours": 1,
        },
        {
            "coins": "0.500000",
            "hours": 1,
        }
    ]
}
```

### Coin Hours Use Cases

Coin Hours can be the basis of several Skycoin-powered features,
such as payment for VPN services, or bandwidth on Skywire.

#### Skywire

Coin Hours are used as the payment currency for bandwidth in the Skywire network.

#### CX

CX program blockchains will also run on the Skywire network, making Coin Hours
a currency for CX application users.

CX program blockchains can also have their own internal Coin Hours (not the same as Skycoin Coin Hours),
and use these similarly to how Ethereum's Gas restricts program execution.

#### CoinJoin

In CoinJoin servers,
Coin Hours can be used as collateral during the merging and mixing process,
discouraging users from backing out or slowing down an ongoing CoinJoin operation.
