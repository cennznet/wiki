# Getting started with CENNZnet DApp Development

## What do I need to know about blockchain?
CENNZnet does most of the hardwork, so you don't have to have a detailed understanding of how blockchain works. However it'd still help you find the best use cases and design the best DApp to have a basic understanding of how blockchain works. Head to the [Blockchain Primer](getting-started/blockchain-primer) guide if you need :)

## What technologies do I need know to build DApps on CENNZnet?

You will need **JavaScript** to interact with CENNZnet through the [CENNZnet API](https://github.com/cennznet/api.js).

In some cases, for example when building a crypto wallet app, the DApp only needs the functionalities provided by CENNZnet's runtime modules without any customisation. Then JavaScript would be the only prerequisite.

If you need features that aren't provided by the CENNZnet API, you can modify the runtime modules or create your own. The modules are written in the programming language Rust, which gives us the optimal performance and security.

## Coming to CENNZnet from Ethereum 

If you have an understanding of DApp development on Ethereum, transitioning to CENNZnet is going to be a breeze. You can either use CENNZnet's runtime modules by interacting with the JavaScript API, or [deploy Etherem smart contracts to CENNZnet ](dapp-development-guides/Using-Smart-Contracts-on-CENNZnet).

### Migrating a DApp from Ethereum to CENNZnet 

Migrating an existing DApp from Ethereum to CENNZnet is very easy thanks to the [EVM module](runtime-modules/EVM). Check out the demo here:

[Demo video](https://youtu.be/f4wblOufvs4)

## How does CENNZnet work?

The [Technical Overview](getting-started/CENNZnet-technical-overview) explains how CENNZnet works internally and the main interfaces.

## Development tools

[CENNZnet.io](https://cennznet.io/#/) is the most important development tool. If you are new to CENNZnet DApp development, be sure to get comfortable with cennznet.io!

[CENNZnet Portal Tutorial Series on YouTube](https://youtu.be/Ikm2lqgeK-A)

Textual guides:
* [Intro Guide to CENNZnet.io](https://medium.com/centrality/using-cennznet-io-ac5a90f9a2cb)
* [Advanced Guide to CENNZnet.io](https://medium.com/centrality/advanced-guide-to-cennznet-io-33be90f26ff3)

During development, it's useful to use a local sandbox environment, so that you can start from a clean state as required. The Development chain provides such an environment. To set one up, see this guide,
[Running a Development Chain](Network-participating/Node-operating/Running-a-Dev-Chain).

## The pre-production environment

Once your feature is ready to be shared with a wider audience, you can test it on the TestNet (Nikau 🌴), the CENNZnet pre-production environment, before you deploy it to the MainNet (Azalea).

The TestNet provides a similar environment to the MainNet, and it's a safe place to fail. 

On the TestNet, you won't need to spend actual MainNet currencies. You can issue test currencies for yourself using the [CENNZnet Faucet](dev-tools/CENNZnet-faucet).

## Next steps

Head over to [Dev Environment Setup](getting-started/Dev-environment-setup) to get yourself ready for DApp development!