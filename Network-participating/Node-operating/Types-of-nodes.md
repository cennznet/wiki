# Types of nodes

There are a few types of nodes in CENNZnet.

## Archive Nodes

An Archive Node contains the full history from the start of CENNZNet. This requires a larger amount of storage space. We recommend allocating at least 250G of disk space for Archive Nodes.

Validators need to use Archive Nodes.

## Full Nodes

A Full Node contains the history of the last 256 blocks. This allows you to interact with the chain, but doesn’t have the history of earlier blocks.

DApps can connect to a Full Node or an Archive Node to communicate with CENNZnet.

## Development Chain/Nodes

The development chains are special chains that consists of one local node. They are completely disconnected from the MainNet or the TestNet, so that you have a local sandbox environment for testing.

A development chains comes with test accounts that own billions of CENNZ and CPAY. This allows you to test your DApps without paying real money.


## Light Nodes

A Light Node contain only the runtime and the current state. It doesn't store past extrinsics and so cannot restore the full chain from genesis. 

Light nodes are useful for resource restricted devices. A use-case of light nodes is a Chrome extension, which is a node in its own right, running the runtime in WASM format.