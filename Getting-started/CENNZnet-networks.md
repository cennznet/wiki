
# The CENNZnet networks

Anyone can start a new CENNZnet network by starting a CENNZnet node. When you run a CENNZnet node, you can connect to an existing chain using a genesis file. The genesis file provides the initial state and configurations of the blockchain.

In the CENNZnet repository, you can find 3 genesis configurations. 
* **Azalea (the MainNet)**: is the production environment, where transactions and exchanges happen. The MainNet is used by all CENNZnet DApps. 

* **Nikau🌴 (the TestNet)**: is the pre-production environment for testing bleeding edge features before they are deployed to Azalea. Nikau runs on nodes hosted on AWS. The CENNZnet team maintains Nikau and keeps it close to the MainNet.

* **Rata (the playground)**: is the testing environment that's not kept close to the production environment. You can also use this genesis file to start a small sandbox chain for testing locally. It can start a chain of nodes that runs in docker containers, all on your local machine. This allows you to test complex scenarios that aren't possible to test with a development chain, which consists of 1 node only. To connect to Rata, you will need to use the custom endpoint, [wss://kong2.centrality.me/public/rata/ws](wss://kong2.centrality.me/public/rata/ws).

## Connecting to a network
The easiest way to connect to one of the CENNZnet networks is to use the web portal, [CENNZnet.io](http://cennznet.io/).
Head to the [CENNZnet.io user guides](References/CENNZnet-infrastructures/Exploring-the-CENNZnet-UI) to learn how to manage account and explore the blockchain.

## Joining the network

As a DApp developer, it is beneficial to join the network. Having nodes of your own allows your DApp to have better and faster access to the network. You can also monitise from the nodes by participating in staking as a validator.

To learn about how to set up a node, read our guides in the Network participating -> Node operating section, for example, [Running a Full Node](Network-participating/Node-operating/Running-a-Full-Node).