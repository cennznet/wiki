# Network Fees

This guide gives an overview of network fees on CENNZnet and the fee token CPAY.  

## What is CPAY?

CPAY is a token issued on the CENNZnet blockchain which is used to pay network transaction fees.  
Anytime an operation occurs on CENNZnet to store or modify data, this is known as a _transaction_.  
This is not a transaction in the financial sense but in the [database operational sense](https://en.wikipedia.org/wiki/Database_transaction).  
The transaction's result could be financial in nature or non-financial, for example creating an NFT.  

## Why is CPAY necessary?

Operating computer hardware incurs costs, normally a platform business e.g. cloud provider would pass these costs on to their customers. Typically this is done with a billing model where users subscribe for a fixed number of operations, pay per use, etc.  

However, CENNZnet is a decetralized, public blockchain, without a central business. Data is stored and maintained by a diverse network of independent operators aka _validators_.  
When users write or modify data on CENNZnet they must pay the corresponding CPAY fee to cover the validators costs to execute it.  
Validators receive the CPAY fee payment for each transaction that they process as fair compensation by the CENNZnet protocol.  

Lastly, CPAY fees act as a security mechanism for the network. As CENNZnet is a public blockchain,
malicious users are still required to pay network fees, making spam or other activities which could congest the network costly to execute.  

## How are fees calculated?

Each transaction on CENNZnet carries a 'weight' value which is used to derive an associated CPAY fee.

The weight is calculated from a statistical average of the computational resources used to perform a particular transaction.  
This is measured using a benchmarking process with a standard validator hardware configuration. 

## How are fees paid?

CPAY fees are automatically deducted from an account when it sends a transaction on CENNZnet.  
If the account does not have CPAY tokens or not enough to cover the cost of the transaction it will be rejected.  

## How do I get Cpay?

CPAY tokens may be obtained from participating exchanges or earned by staking CENNZ governance tokens to
secure the network.  
