# Identity Module

The identity module allows users to make claims of ownerships of email and social accounts on CENNZnet. It provides the following types of verifications:
* Email
* Discord account
* Twitter account

Associating an email or social account to your CENNZnet account is called a Registration. A CENNZnet account can have multiple Registrations. 

The identity module is useful for tasks that are associated with a real person, for example, voting. The identity module is crucial for [Governance](Runtime-modules/Governance).

## The process of associating an email/social account with a CENNZnet account
* User sends a call `identity.set_identity` from the CENNZnet portal or CENNZnet API
* User sends a verificaton message from your email/social account
* A bot (acting as a registrar) receives the verification request, and provides a proof or judgement. 

## Retrieving the identity proof
Use the method `identity.provideJudgement` to retrieve the identity claims associated with a CENNZnet account.

## Fees
To use the identity module, the users would only need to pay a small amount of CPAY for transaction fee for calling the method `identity.set_identity`. Unlike Substrate, there's no fees involved for the registrars when using the Identity module.

## The programming interface
The identity module is based on the Substrate Identity pallet, which means this module follows the interface defined in the [Substrate docs](https://docs.substrate.io/rustdocs/latest/pallet_identity/index.html).

## Implementation

The identity module is based on the [Substrate Identity pallet](https://docs.substrate.io/rustdocs/latest/pallet_identity/index.html), but with a few CENNZnet specific changes:

* We've simplified the authorisation requirements
* We've added an additional CENNZnet interface to this module, which also gives us information about how many identities an account has registered

The source code can be found in the [CENNZnet's fork of Substrate](https://github.com/cennznet/substrate).
