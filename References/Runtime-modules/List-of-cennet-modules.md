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
The following Substrate modules, also known as [pallets](https://substrate.dev/docs/en/knowledgebase/runtime/pallets), from Substrate are also used in CENNZnet. The CENNZnet API exposes some features from these modules.

### Authorship
The [Authorship pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_authorship/index.html) tracks the authors of blocks and their recent uncles.

### Babe
The [BABE pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_babe/index.html) is use for the [consensus mechanism](https://cennz.net/publications/understanding-consensus-mechanisms/). It collects on-chain randomness from VRF outputs and manages epoch transitions.

### Grandpa
The [GRANDPA pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_grandpa/index.html) is used in the finality part of the consensus mechanism.

### Identity
The [Identity pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_identity/index.html)

### imOnline
The [I'm Online pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_im_online/index.html) allows validator nodes to signal that the node is online.

### Multisig
The [Multisig module](https://substrate.dev/rustdocs/v3.0.0/pallet_multisig/index.html) provides functionality for multi-signature dispatch.

### RandomnessCollectiveFlip
The [Randomness Collective Flip pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_randomness_collective_flip/index.html) provides a random function that can be used in tests.

### Scheduler
The [Scheduler module](https://substrate.dev/rustdocs/v3.0.0/pallet_scheduler/index.html) allows you to schedule tasks to occur at a specified block number or at a specified period.

### Session
The [Session pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_session/index.html) allows validators to manage their session keys, session lengths, and handle session rotations.

### Sudo
The [Sudo pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_sudo/index.html) provides "privileged functions" (i.e. functions that require Root access).

### System

### Timestamp
The [Timestamp pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_timestamp/index.html) provides functionality to get and set the on-chain time.

### TransactionPayment
The [Transaction Payment pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_transaction_payment/index.html) provides the basic logic to compute pre-dispatch transaction fees.

### Treasury
The [Treasury pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_treasury/index.html) allows stakeholders in the system to manage funds and provides and a structure for making spending proposals.

### Utility
The [Utility pallet](https://substrate.dev/rustdocs/v3.0.0/pallet_utility/index.html) provides helpers for dispatch management.