# List of CENNZnet Modules

CENNZnet is built on [Substrate](https://www.parity.io/substrate/), which provides building blocks for blockchains. This document provides high-level overview of modules and links to the detailed documentation for each module.


## CENNZnet specific modules

### Attestation
The [Attestation](References/Runtime-modules/Attestation) module allows you to issue and check claims on accounts. For example, an issuer can set claims to verify if a person is older than 18 years old.

### CENNZX
The [CENNZX](References/Runtime-modules/CENNZX) module allows you to exchange between different types of generic assets on CENNZnet. It allows users to participate in exchange as a liquidity provider or as an exchange user.

### Doughnut
The [Doughnut](References/Runtime-modules/Doughnut) module let you prove that one address delegates something to another address. Doughnuts are Proofs of Delegation between two or more cryptographic key pairs. 

### Generic Asset
The [Generic-Asset](References/Runtime-modules/Generic-Asset) module lets you mint, transfer, and burn different types of tokens. It also allows you to check balance on accounts.

### NFT
The [NFT](References/Runtime-modules/NFT) module allows users to easily create and sell custom NFTs(non-fungible token). It also provides functionalities to sell and auction NFTs. 

### Staking
The [Staking](References/Runtime-modules/Staking) module allows users to participate in staking to capitalise from their assets or hardware.

### Rewards
The Rewards module can be considered an ‘internal’ module that looks after staking reward economics and payments. It's unlikely to be useful for DApp development, except for when tracking staking rewards.

## Substrate modules
The following Substrate modules/[pallets](https://substrate.dev/docs/en/knowledgebase/runtime/pallets) from Substrate are also used in CENNZnet. The CENNZnet API exposes some features from these modules for customisation purposes.

### Authorship

### Babe

### Grandpa

### Identity

### imOnline

### Multisig

### RandomnessCollectiveFlip

### Scheduler

### Session

### Sudo

### System

### Timestamp

### Treasury

### TransactionPayment

### Utility