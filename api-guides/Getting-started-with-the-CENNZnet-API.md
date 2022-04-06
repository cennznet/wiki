# Getting started with the CENNZnet API

This guide will get you set up with the CENNZnet API and walk through a couple code examples.
To understand how to use the API, please read the [CENNZnet API Overview](api-guides/CENNZnet-API-Overview).

## Installation and Setup

First, [set up your  dev environment](getting-started/Dev-environment-setup).

## Running a development node

A [development node](Network-participating/Node-operating/Types-of-nodes?id=development-chainnodes) gives you a sandbox environment for developing new features.

Run the following command to start a dev node.
The `--dev` flag will start the node as a single validator on a development network, also enabling full access to the node's JSON-RPC API.

```bash
docker run -p 9944:9944 cennznet/cennznet:latest --dev --ws-external
```

For help, run:
```bash
docker run cennznet/cennznet:latest --help
```
For more information on the arguments, see [run a Full Node](Network-participating/Node-operating/Running-a-Full-Node). 

## Using the API
The CENNZnet API allows you to interact with a node in 2 ways:
* [Javascript Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [RxJS observables](https://rxjs-dev.firebaseapp.com/guide/overview)

You can choose the approach based on the requirements for your DApp. If you don't know which one to choose, the Promise version covers most use cases, and it doesn't require another library as it's a part of ES2015 specification.

There is also a in-depth [Getting Started guide in the API repo](https://github.com/cennznet/api.js/blob/develop/docs/GET_STARTED.md).

## Code examples
The [CENNZnet API repo](https://github.com/cennznet/api.js) contains many [code examples](https://github.com/cennznet/api.js/tree/develop/docs/examples) for both the Promise and the RxJS approaches. The examples cover common usages of the CENNZnet API. We will highlight a couple examples here for demonstration.

Clone the API repo, so you can run the examples.
```bash
git clone https://github.com/cennznet/api.js.git
```


### Basic connection
This example, 01_simple_connect, shows how to connect to a CENNZnet node using the CENNZnet API.

Once API (@cennznet/api) is installed, it is available either in Promise version or RxJS version  
* [Via Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/01_simple_connect/index.js) version of API
* [Via RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/01_simple_connect/index.js) version of API

Run the examples by running
```bash
yarn && yarn start
```

Should see output similar to:
```bash
You are connected to chain Development using CENNZnet Node v1.4.0-...
```

The `Api` object is dynamically populated with query and transaction methods once connected to a node.  
Here `types` specify CENNZnet specific types which are additional to the `@polkadot/api` library.  
In your application, you may develop new types for your runtime modules which will need to be included here also.  
```js
// Create the API and wait until ready
const api = await Api.create({
  provider: address,
  types: {...}
});
```

The API is used to fetch a bunch of data using promises/rxjs. In the case of `Promise.all` the values are not returned until all contained promises are resolved:
```js
// Retrieve the chain & node information information via rpc calls
const [chain, nodeName, nodeVersion] = await Promise.all([
  api.rpc.system.chain(),
  api.rpc.system.name(),
  api.rpc.system.version(),
]);
```

### Sending a transaction to the blockchain
Example 06_make_transfer shows how to make a transfer using the API.

* [Via Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/06_make_transfer/index.js)
* [Via RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/06_make_transfer/index.js)

In this example, we are using the `keyring.alice` and `keyring.bob` test accounts, which are available on a develop blockchain. 

Next, we want to confirm the transfer actually happens, so we can subscribe to any balance changes.

### Reading data from the blockchain
Example 03_listen_to_balance_change shows how to listen to balance change using the API.

* [Via Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/03_listen_to_balance_change/index.js).
* [Via RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/03_listen_to_balance_change/index.js).


## Next steps

Dive into the References area to learn more about the CENNZnet API and runtime modules! 

The [CENNZnet API Overview](api-guides/CENNZnet-API-Overview) is a good place to start!