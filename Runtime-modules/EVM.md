# EVM(ETHEREUM VIRTUAL MACHINE) module

The EVM module provides the environment to deploy and run Ethereum Smart Contracts natively on CENNZnet. 

This brings a few major benefits to CENNZnet and Ethereum users:

* Connect the Ethereum and CENNZnet ecosystems: CENNZnet DApp developers can reuse the code and infrastructure built by the Ethereum ecosystem in their CENNZNet DApps.
* Easy Dapp migration: Ethereum DApp developers can now migrate their DApps to CENNZnet. This provides faster transaction speeds and lower transaction fees.
* Write CENNZnet DApps with Solidity: Ethereum developers can keep using their favourite languages and tools when developing on CENNZnet.

See the guide [Smart Contracts](dapp-development-guides/EVM/Using-Smart-Contracts-on-CENNZnet) on how to deploy and execute Smart Contracts on CENNZnet.

## Combining CENNZnet modules with Smart Contracts
The EVM module on CENNZnet allows you to call CENNZnet modules from your Smart Contracts. This can add the power of CENNZnet modules to existing Ethereum DApps.


## Choosing between the EVM module and runtime modules

With the addition of the EVM module, you can now implement your DApp using smart contracts or runtime modules. What are the pros and cons of either approach?

The runtime modules offer the best efficiency and lower costs than smart contracts. This is because they are written in Rust and are highly optimized. Using the runtime modules also saves you development time, because the functionalities are easily accessible through a simple JavaScript API. When designing a new DApp, it’s best to search through the modules to see if the problem has already been solved.

The EVM module and smart contracts offer more flexibility. You can create your custom logic and deploy them any time. You can also use Solidity and other tools that you may be more familiar with.

## Fees
Fees are charged for deploying and executing smart contracts on CENNZnet. 

Executing a smart contract on CENNZnet costs a very small transaction fee, typically a few CPAYs.

Deploying a smart contract is slightly more expensive than executing. This is because each contract deployed to the EVM will be stored on the chain forever. This requires storage space.
