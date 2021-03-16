## CENNZnet v1.1.0-rc1
* Adds support for multi-currency payment of the fees including the gas fee
* Scales down CPAY and CENNZ balances plus fee payments for a better user experience
* Improves on the staking reward curve
* Fixes authorship reward payment
* Unravels imbalance calculation in the Transaction Payment module
* Implements CENNZnut Contract Validation
* Adds CENNZxSpot Liquidity Check RPCs
* Refactors CENNZxSpot for clarity and improves on doc comments
* Refactors the Sylo module for maintainability
* Integrates Plug v1.0.0-rc4:
   - Stabilizes the transaction pool
   - Splits the notification stream for the transaction pool and the off-chain worker. It makes the whole system faster under load.
   - Adds a Generic Asset RPC call for querying asset metadata
   - Fixes a few Generic Asset issues and refactors it for maintainability
   - Adds Dispatch Method Arguments
   - Adds deposit/withdraw events to EVM module
   - Makes EVM gasometer configurable
   - Fixes propagation in the network
   - Passes transaction source to validate_transaction
   - Splits PrimaryPreDigest and SecondaryPreDigest
   - Makes Treasury tip `KeepAlive`
   - Allows Grandpa noting same sets in gossips with different authorities
   - Fixes some network syncing issues
   - Bumps doughnut-rs version
* Bump stable rust version to 1.44.1

Docker image 🐳
`docker pull cennznet/cennznet:1.1.0-rc1`

## CENNZnet v1.0.0-rc3.1 (Stability) 🔨
* Adds support for multi-currency gas and fee payments
* Awards transaction fees equally to all validators
* Adds the CENNZxSpot price query RPC
* Uses standard types and checked methods for large integer calculations in CENNZxSpot
* Improves the staking module code around invulnerable validators.
* Amortises validator reward payouts
* Integrates Plug v1.0.0-rc3: 
   - Enables gas fee payment in Contracts to be implemented with different strategies
   - Improves doughnut support:
       + Adds a delegated runtime dispatch that supports doughnut
       + Adds an optional doughnut parameter to contract calls
       + Ensures delegated calls are verified by doughnut
       + Introduces plugDoughnutApi trait for PlugDoughnut
       + Fixes doughnut validation to input chain timestamp in seconds
       + Updates doughnut-rs version for codec and schnorrkel compatibility
   - Improves Contracts:
       + Keeps the origin of the calls who authors the original extrinsic. Important for entrusted calls.
       + Allows Contracts to distinguish out of gas from other traps
       + Adds ext_transfer call to Contracts
       + Adds the Rent projection RPC to Contracts
   - Improves Attestation module
   - Adds the MultiAccounting trait to Generic Asset
   - Introduces Prometheus metric endpoint and makes many different modules report their metrics
   - Adds the subscribe_all_heads RPC
   - Waits for RPC server to clean up on shutdown
   - Makes RPC calls to be executed with configured strategy
   - Makes Utility/Recovery passthrough always pay a fee
   - Adds fork-awareness and finalization notifications to transaction pool watchers
   - Introduces Async/await in transaction-graph
   - Revalidates some transactions on every block import
   - Improves network syncing
       + Allows syncing to peers with finalized common blocks 
       + Prioritizes new blocks over old forks when syncing
       + Fixes the notification sinks leak during the initial sync of the client
       + Validates block responses for Sync required data
   - Does not allow zero Existential Deposit when using Balances
   - Introduces `VestingCurrency` trait for Balances
   - Introduces `OnReapAccount`
   - Improves the staking module behaviour:
       + Increases the penalty for being offline
       + keep nominations after getting kicked with zero slash
       + Introduces safe multi-era slashing for NPoS
       + Doesn't slash Validators for being offline until 10% at once
   - Improves gossip producing and handling:
       + Introduces time-based gradual gossip
       + Integrates GossipEngine tasks into `Future` implementation affecting network-gossip and finality-grandpa
       + Improves handling of global gossip messages in Grandpa
   - Adds generic voting rule for backing off from the best block in Grandpa 
   - Adds support for hard forking any pending standard changes to Grandpa
   - Makes BABE pass epoch data via intermediates
   - Introduces BABE exponential back-off on missed blocks
   - Makes sure AURA slot numbers are strictly increasing
   - Adds a tipping protocol to the treasury module
   - Exposes runtime information about the extrinsic in the metadata
   - Forgets peer-set manager nodes after 5mn
   - Adds the renounce candidacy function to elections-phragmen
   - Allows referendums to begin out of order
   - Fixes some economic issues of the democracy module
   - Adds a comprehensive identity system

Docker image 🐳
`docker pull cennznet/cennznet:1.0.0-rc3.1`

## CENNZnet v1.0.0-rc1 (Stability) 🔨
* New contract RPC endpoint like `ethCall` for dummy executions + gas cost estimates
* Replaces AURA consensus with BABE consensus
* Replaces `fees` module with `transaction-payment`
  - Enables setting tx priority via `tip`
* Consolidation and cleanups to arithmetic types
* Integrates Plug v1.0.0-rc1:
  - Runtime doughnut permissioning
  - Block finalization fixes
  - Extensible transactions improve transaction life cycle hooks and provide great flexibility for extension going forward

Docker image 🐳
`docker pull cennznet/cennznet:1.0.0-rc1`