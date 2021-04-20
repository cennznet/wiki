# CENNZnet DApp development

## Dev environment
A machine with the following dependencies installed:
* [Docker](https://www.docker.com/)

* [Node.JS](https://nodejs.org/en/)

## What technologies do I need know to build DApps on CENNZnet?

You will need **JavaScript** to interact with CENNZnet through the [CENNZnet API](https://github.com/cennznet/api.js).

In some cases, for example when building a crypto wallet app, the DApp only needs the functionalities provided by CENNZnet's runtime modules without any customisation. Then JavaScript would be the only prerequisite.

Most DApps require customised storage on the blockchain. This is done through Smart contracts. On CENNZnet, smart contracts are small pieces of code written in [**Rust**](https://www.rust-lang.org/). This gives us the optimal performance and security.

## Development tools

[CENNZnet.io](https://cennznet.io/#/) is the most important development tool. If you a new to CENNZnet DApp development, be sure to read the following user guides to CENNZnet.io:
* [Intro Guide to CENNZnet.io](https://medium.com/centrality/using-cennznet-io-ac5a90f9a2cb)
* [Advanced Guide to CENNZnet.io](https://medium.com/centrality/advanced-guide-to-cennznet-io-33be90f26ff3)

During development, it's useful to use a sandbox local environment, so that you can start from a clean state as required. The Development Chain provides such an environment. To set one up, see this guide,
[Running a Development Chain](Network-participating/Node-operating/Running-a-Dev-Chain).

## The pre-production environment

Once your feature is ready to be shared with a wider audience, you can test it on the TestNet (Nikau), the pre-production environment, before you deploy it to the MainNet (Azalea).

The TestNet provides a similar environment to the MainNet, and it's a safe place to fail. 

On the TestNet, you won't need to spend actual MainNet currencies. You can issue test currencies for yourself using the [CENNZnet Faucet](References/CENNZnet-infrastructures/CENNZnet-faucet).

## Next steps

This guide [How to build a DApp](Dapp-development/Guides/How-to-build-a-DApp) covers the architecture of a DApp and where to go next :)
