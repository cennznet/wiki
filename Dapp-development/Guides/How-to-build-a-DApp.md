# How to build a DApp

A DApp(Decentralised App) is an app that uses a blockchain as the backend data store. The blockchain gives your app the benefits of decentralized storage and peer-to-peer communication.

To build a DApp with CENNZnet, you will need:
* Build a frontend in your chosen framework as long as it
s compatible with JavaScript.
* Interact with CENNZnet's MainNet using the CENNZnet JavaScript API.
* You may need to write and deploy Smart Contracts if you need custom methods that's not provided by the core modules.

## The architecture of a DApp

As outlined in the [CENNZnet technical overview](Getting-started/CENNZnet-technical-overview), your DApp can communicate with CENNZnet in 2 ways:
1) Calling methods of the runtime modules
2) Calling smart contracts

![Architecture](../../assets/images/how-to-build-a-dapp/architecture.png)

## A DApp's communication with a node


## Storage considerations

### Storing large data
You can store any data on the blockchain, but it's costly to store large data on the blockchain. This is because history is kept forever. 

To reduce the costs, you can store larger data on distributed storage solutions, for example [IPFS](https://ipfs.io/), and store URLs to the data on the chain.

### Storing sensitive data

When storing sensitive data, the general approach is to do the following:
1) store a hash of the sensitive thing on CENNZnet so all can verify
2) share the actual information on secure channel off chain which can be compared to the thing on CENNZnet 