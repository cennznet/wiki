# Validator Operations 🛠

This guide provides an overview of the essential steps to start a validator node.  
You should have familiarity with a unix shell and docker to get the most out of this guide.  

⚠️ The process of connecting a node with a CENNZnet account is described in the [staking guide](Network-participating/Staking/Validator-Staking-Guide)  
and must be performed before the node will participate in consensus and earn rewards  

## Let's go
In this example we provisioned a Digital Ocean droplet with the following spec & [docker installed](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-10)  
```
450 GB disk
16GB Mem
2 vCPU
Ubuntu OS
```

### 1) Download the Latest Snapshot  
This is a snapshot of the CENNZnet blockchain maintained by Centrality.  
It will give the node a large head start on syncing the block data.  
**NOTE!** check [MainNet DB snapshots](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/2.0.0/validator/index.html)
for the most recent snapshot file and substitute it in the command below  
```bash
SNAPSHOT_DATE=2021-03-16-1035
aria2c -x 8 "https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/2.0.0/validator/NZDT-$SNAPSHOT_DATE-chains.tar.gz"

# extract the snapshot and move the 'chains' folder into './data/chains'
tar xvf NZDT-2021-03-16-1035-chains.tar.gz && \
mkdir data && \
mv chains/ data/
```

### 2) Generate P2P Network Identity 🔑
This will create a 32 byte secret seed we can use for our node's p2p secret key.  
Setting this value will give the node a consistent p2p network identity improving its peer connections and discovery
on the network.  
Save this value somewhere secure so it can be reused by your node later.  
```
NODE_P2P_KEY=$(openssl rand -hex 32)
```

### 3) Start the Node

The command below will start a dockerized CENNZnet node.  
The `data` folder will be mounted into the container at `/data`, this contains the chain snapshot downloaded earlier.  

The important configurations are:  
`--name` is any name you want your node to appear as when reported to the telemetry server  
`--node-key` the 32-byte seed network identity we generated earlier  
`--base-path` the path where the chain snapshot is mounted in the container i.e `/data`  
 `--eth-http` an http/s URL to an Ethereum full node**\*** (see [here](https://cennz.net/knowledge-hub/core-modules/emery-cennznet-ethereum-token-bridge/) for info)  

```bash
# !!!Replace with your pre-generated key!!!
NODE_P2P_KEY=$(openssl rand -hex 32)
NODE_NAME="YOUR NODE NAME HERE"

docker run -d \
    --name=<container_name> \
    --restart=always \
    -p 9933:9933 \
    -v $(pwd)/data:/data \
    cennznet/cennznet:2.0.0 \
    --validator \
    --node-key=$NODE_P2P_KEY \
    --name=$NODE_NAME \
      # Ethereum full node http endpoint (Eth mainnet)
    --eth-http='https://eth-mainnet.alchemyapi.io/v2/<API_KEY_REDACTED>' \
    --unsafe-rpc-external \
    --rpc-methods=Unsafe \
    --chain=/cennznet/genesis/azalea.raw.json \
    --base-path=/data \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:8000/submit 0'
```

### 4) Generate session keys
This command will ask your node to generate some keys which will be used for signing message to participate in consensus protocols
such as making blocks and voting on finality.  
```bash
curl \
    -H "Content-Type: application/json" \
    -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params": []}' \
    localhost:9933
```
*after this step you maybe safely disable the `--rpc-methods=Unsafe` option*

Save this value for use in "claiming your session keys" step in the staking guide.  

### Done
That's it. Check your node's sync progress on the [telemetry site](http://cennznet-telemetry.centrality.me/) and follow the staking guide to connect your node to a CENNZnet account.


## *Ethereum Connection

As of CENNZnet 2.0.0 version, validators require a connection to Ethereum mainnet in order to witness transactions for the Ethereum bridging protocol.
The simplest way is to use an Ethereum RPC provider service, a process which takes only a few minutes to setup.
It is also fine to host your own Ethereum node for this purpose.

These are some services which provide free ethereum full nodes:

- https://zmok.io
- https://infura.io
- https://www.alchemy.com/
- https://moralis.io/

Once an Ethereum full node connection is active, restart your CENNZnet validator providing the full node endpoint with the `--eth-http` flag.  
This will allow the validator to connect with Ethereum and participate in the Emery bridging protocol and earn rewards from the bridging protocol.  
