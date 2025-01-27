---
sip:  32
title: PoC+ polishing
description: Small PoC+ adjustments 
author:   jjos, frank_the_tank, ohager
status: Final
type: Standard
category: Core
requiers : SIP-27
created: 2021-09-18
---
## Introduction
Signum introduced the world to the [Proof of Commitment (PoC+)](sip-27.md) decentralized consensus as the evolution of the Proof of Capacity (PoC) consensus. Unlike the well-known Proof of Work (PoW) consensus used by Bitcoin and many other coins, which requires special power-hungry equipment, PoC+ uses available disk space in a hybrid method with Proof of Stake (PoS).

It offers a new way for miners to increase their effective storage capacity - committing a Signa balance (stake) in their accounts. This helps to secure the network and improves decentralization, allowing small (home) miners to compete with large mining farms.

## Motivation
The introduction of PoC+ in Signum has already shown empirically that this new consensus can produce much more decentralized networks than PoW, PoS, and PoC. However, its current parameters can be tuned to produce a more stable average commitment and also to increase security.

## Specification

In this SIP, the following changes are suggested:
 - The average commitment is estimated using a moving average of 96 blocks instead of 24.
 - There will be 3 different block windows when estimating a miner’s effective capacity (instead of only 2):
    - Past 360 blocks (1 day) if a miner has forged 2 or more blocks in this period.
    - Past 1440 blocks (4 days) if a miner has forged 2 or more blocks in this period (this is the additional one).
    - Past 32400 blocks (3 months) otherwise.
 - The cumulative difficulty will use the average commitment of a block 1440 blocks in the past (previously only this one was used) and also the latest block average commitment.
 - Newly added commitment becomes active for mining after 1440 blocks (4 days) instead of 60 blocks.

With these changes, the network average commitment oscillation is much reduced. In the experiment on our testnet, the upgrade took place at block 338090 and it is clear that the average commitment became much more stable after it:

![image](./assets/sip-32/image_1.png)


Having the average commitment and the cumulative difficulty more stable should also reduce the number of short-lived forks. Since too much oscillation in the average commitment can produce blocks with longer deadlines but with higher cumulative difficulties.

The increase in the number of blocks for the commitment to become active, as well as the increased minimum capacity, aims to make the platform even harder to attack. By increasing the number of blocks for the commitment to become active to 1440, we make it literally impossible for an attacker to revert the commitment transaction (since it will be only available for mining when it is already impossible to revert the transaction). By increasing the number of blocks for committed coins to become active, we reduce any temptation to get an edge by splitting capacity into different accounts, replotting after forging blocks, or generating plots on-the-fly/in-memory with GPUs.

## Backwards Compatibility
This is a hard forking change, thus breaks compatibility with old fully-validating node. It should not be deployed without widespread consensus.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
