# Running a full node

This guide uses docker, ensure the Docker daemon is installed (see [here](Installing-Dependencies#Docker)) and running.
To build from source see [building from source](#Building-from-Source).  

## Chain Config Files

Config files for specific CENNZnet networks are available in the main repository [here](https://github.com/cennznet/cennznet/tree/develop/genesis).

The `<name>.raw.json` versions should be used as they're compatible with multiple client versions, while the `<name>.json` merely serves as a human readable reference of the config, but does not maintain compatibility between client versions.

### Connecting to CENNZnet MainNet
Then run a full node that connects to the MainNet:

```bash
$ docker run cennznet/cennznet:1.5.1 \
    # genesis is included in the image
    --chain=/cennznet/genesis/azalea.raw.json \
    --name=<NODE_NAME> \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'
```

Check its status on the telemetry site:
http://cennznet-telemetry.centrality.me/#/CENNZnet-Azalea

## Storage Config
To configuring the node's storage directory
```
--base-path=/path/to/storage/dir
```

To run the node in archive mode, storing *all* blocks and states use:
```
--pruning=archive
```
by default the full node will only retain the most recent latest 256 blocks and states on disk.
Set a custom limit:
```
--pruning=1000
```
At the time of writing a full node consumes ~10GB of storage.  
While in archive mode ~90GB storage.  

## Enabling Ws/RPC Ports
The following combination of flags will allow the node's RPC and/or websocket functionality to be exposed.
```bash
# Allow Ws & RPC connections from selected origins (default local only)
--rpc-cors=example.com,test.dev

# Allow ws connections on non-local interface
--ws-external
# configure ws port (9944 default)
--ws-port 9944

# Allow rpc connections on non-local interface
--rpc-external
# configure rpc port (9933 default)
--rpc-port 9933
```

## Running a Bootnode
A bootnode is any fullnode with a public p2p port.
It will allow other nodes to join the network and discover other peers.

In addition to ordinary full node flags, a public boot node must specify:
1) Open p2p port (default 30333)
2) Set network identity
3) Set peer connection limit
4) Advertise/listen multi-address

configure P2P port
```
--port 30333
```

Set p2p network identity.
Pass 32 byte key as flag or us secure file.


```
--node-key=$(openssl rand -hex 32)
```
Or generate network key using [subkey tool](https://substrate.dev/docs/en/knowledgebase/integrate/subkey)
```
./subkey generate-node-key > identity.key
```
start node with flag
```
--node-key-file=/path/to/identity.key
```

Specify maximum peer connections in and out
```
--in-peers=50
--out-peers=50
```

Set listen and advertised multi-addresses
```
# Public multi-address to advertise (supports connection behind proxy)
--public-addr= "/dns4/bootnode.exampel.com/tcp/30333/p2p/12D3KooNA6iSdKNJUpvNPTRzxiLfA2LzPgCTywWP2ZeRnabDaDEa"
# Public multi-address to listen on
--listen-addr="/dns4/bootnode.exampel.com/tcp/30333/p2p/12D3KooNA6iSdKNJUpvNPTRzxiLfA2LzPgCTywWP2ZeRnabDaDEa"
```
p2p address e.g. `12D3KooNA6iSdKNJUpvNPTRzxiLfA2LzPgCTywWP2ZeRnabDaDEa` can be found by checking node start logs or using `subkey inspect-node-key` command
on the node key file.

## Other Flags

To get detailed descriptions on the flags, run either of the following:
```bash
$ docker run cennznet/cennznet:1.5.1 --help  # using docker
$ ./target/release/cennznet -- --help        # using binary
```

## Synchronisation

When you start a node with a genesis file, it only holds the initial block. This means it only knows about the very beginning of CENNZnet but not any events that every other nodes acknowledges. Synchronisation is the process where the node learns the history (existing blocks) on the network, so it's up to date with the latest events on the MainNet.

The node finishes synchronising when the best block number is the same or close to the target block number. 
Example output from the node:

```
INFO ⚙️ Syncing 21.2 bps, target=#1140132 (1 peers), best: #532405 (0xfad8…07ea), finalized #531968 (0x1a24…d249), ⬇ 7.3kiB/s ⬆ 60 B/s
```

### Connecting to Rata for development
[Rata](Getting-started/CENNZnet-technical-overview?id=cennznet-networks-and-genesis-files) is a playground network for testing. The following command would run a node that connects to Rata and  with full WebSocket connectivity enabled (unsafe for validators).
```bash
docker run cennznet/cennznet:1.5.1 \
  --chain=/cennznet/genesis/rata.raw.json \
  --bootnodes=/dns4/bootnode-rata-0.centrality.me/tcp/30333/p2p/12D3KooWA6iSdKNJUpvNPTRzxiLfA2LzPgCTywWP2ZeRnabDaDEa \
  --unsafe-ws-external \
  --ws-port=9944 \
  --rpc-cors=all
```

## Building from Source

Make sure Rust is installed on your machine (see [here](Installing-Dependencies#Rust)).

Clone the repository and build the binary:

```bash
$ git clone https://github.com/cennznet/cennznet --branch release/1.5.1 && cd cennznet
$ cargo build --release
```

Then run the binary directly:
```bash
$ ./target/release/cennznet \
    --chain=genesis/azalea.raw.json \
    --name=<NODE_NAME> \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'
```
