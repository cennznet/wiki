# Transaction error codes

General
---
Any transaction may fail with one of the following common errors (inherited from [plug](https://github.com/plugblockchain/plug-blockchain/blob/d3a65efbd1e8251f87a063665bd8964f7f3afb25/primitives/runtime/src/transaction_validity.rs#L36))

| Code             | Description            |
|------------------|------------------------|
|	0 | Transaction method (call) is invalid |
|	1| General transaction payment failure |
|	2| Nonce is higher than expected (future) |
| 	3 | Nonce is lower than expected (stale) |
|	4 | Transaction signature/proof is bad |
|	5 | Transaction has expired |
|	6 | Transaction cannot fit into a block (too large or resource intensive) |

Generally, CENNZnet has module specific error codes which are automatically generated and will render nicely via the metadata / js client.
However, some modules require special transaction error codes (this is an unfortunate implementation detail).

Fee Payment
---
These error codes will be raised during any normal transaction, especially those which use multi-currency fee payment feature.

| Code             | Description            |
|------------------|------------------------|
| 200   | Sender has insufficient CPAY balance to fund the transaction |
| 201   | The requested asset ID to use for fee payment does not exist (it has no issuance on CENNZnet) |
| 202   | An unexpected error during fee payment |

CENNZ-X
---
These error codes come from the CENNZ-X spot exchange, they will be seen during trades or investor liquidity operations. They may also be seen by any transaction which uses multi-currency fee payment.

| Code             | Description            |
|------------------|------------------------|
| 195   | Trader has insufficient balance for the requested trade |
| 197   | Trade yield would be less than the specified minimum |
| 199   | Trade with a `0` value is invalid |
| 203   | Investor has less liquidity in the exchange than the requested amount to withdraw |
| 204   | Trading an asset for itself is invalid |
| 205   | Not enough liquidity in the exchange to fulfill the requested trade |
| 206   | Trade cost would exceed the specified maximum |
