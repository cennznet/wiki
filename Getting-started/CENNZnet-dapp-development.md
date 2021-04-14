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

To avoid paying gas fees during development, it's useful to set up a Development Chain. See this guide,
[Running a Development Chain](Network-participating/Node-operating/Running-a-Dev-Chain), to set up a development chain.

## Next steps

This guide [How to build a DApp](Dapp-development/Guides/How-to-build-a-DApp) covers the architecture of a DApp and where to go next :)
