# Interacting with CENNZnet using the API

## Installation and Setup

Install Node.js (`>=10.16.3`) and Yarn (`>= 1.19.0`).

The library as a package is published [here](https://www.npmjs.com/package/@cennznet/api)
```bash
npm i --save @cennznet/api
```

First, [run a Full Node](Network-participating/Node-operating/Running-a-Full-Node).  
The `--dev` flag will start the node as a single validator on a development network, also enabling full access to the node's JSON-RPC API.  
This is not recommended for real validator nodes; but it is fine for testing.

```bash
docker run cennznet/cennznet:1.2.2 --dev --tmp
```

If you want to know more about these parameters use:
```bash
docker run cennznet/cennznet:1.2.2 --help
```

Once API (@cennznet/api) is installed, it is available either in Promise version or RxJS version  
* Connect to node via [Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/01_simple_connect/index.js) version of API
* Connect to node via [RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/01_simple_connect/index.js) version of API

Run the above examples  
```bash
node start
```

Should see output similar to:
```bash
You are connected to chain Development using CENNZnet Node v1.2.0-...
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

## Using the API

### 1. Sending a transaction to the blockchain

* [Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/06_make_transfer/index.js)
* [RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/06_make_transfer/index.js)

We are using the `keyring.alice` and `keyring.bob` test accounts which are immediately available on a develop blockchain. 

Next, we want to confirm the transfer actually happens, so we can subscribe to any balance changes.

### 2. Getting data from the blockchain

* Subscribe change in balance via [Promise](https://github.com/cennznet/api.js/blob/develop/docs/examples/promise/03_listen_to_balance_change/index.js) way.
* Subscribe change in balance via [RxJS](https://github.com/cennznet/api.js/blob/develop/docs/examples/rx/03_listen_to_balance_change/index.js) way.

For more information on using the API:
* [Polkadot API docs](https://polkadot.js.org/api/start/)

## Send Messages to Smart Contracts
To use the API with Smart Contracts, refer to [call the contract](https://substrate.dev/substrate-contracts-workshop/#/0/calling-your-contract) in Substrate 