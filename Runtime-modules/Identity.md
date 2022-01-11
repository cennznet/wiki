# Identity Module

The identity module provides a proof for an authentic identity. It requires the following information to determine token holder identity:

* Link to a Discord account
* Link to a Twitter account
* Proof of minimum stake

The identity module is useful for tasks that are associated with a real person, for example, voting. It prevents individuals from registering as multiple participants. This process will mean that the verified identity won’t be completely anonymous, but it doesn't require private information.

## Implementation

The identity module is based on the [Substrate Identity pallet](https://docs.substrate.io/rustdocs/latest/pallet_identity/index.html), but with a few CENNZnet specific changes:

* We've simplified the authorisation requirements
* We've added an additional CENNZnet interface to this module, which also gives us information about how many identities an account has registered

