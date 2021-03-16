## Using Docker

Make sure Docker daemon is installed (see [here](Installing-Dependencies#Docker)) and running.

Then run a CENNZnet full node:

```bash
$ docker run cennznet/cennznet:1.2.0 \
    # genesis is included in the image
    --chain=/cennznet/genesis/azalea.raw.json \
    --name=<NODE_NAME> \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'
```

Check its status on the telemetry site:
http://cennznet-telemetry.centrality.me/#/CENNZnet%20Azalea

## Building from Source

Make sure Rust is installed on your machine (see [here](Installing-Dependencies#Rust)).

Clone the repository and build the binary:

```console
$ git clone https://github.com/cennznet/cennznet --branch release/1.2.0 && cd cennznet
$ cargo build --release
```

Then run the binary directly:
```console
$ ./target/release/cennznet \
    --chain=genesis/azalea.raw.json \
    --name=<NODE_NAME> \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'
```

## Chain Config Files

Config files for specific CENNZnet networks are available in the main repository [here](https://github.com/cennznet/cennznet/tree/develop/genesis).

The `<name>.raw.json` versions should be used as they're compatible with multiple client versions, while the `<name>.json` merely serves as a human readable reference of the config, but does not maintain compatibility between client versions.

## To run a test blockchain locally
During development, you may want to interact with a local test net to test your program before connecting to the main blockchain. Interacting with the real blockchain results in in-reversible transactions, and almost all interactions will require some amount of payment, either as a form of transaction fees, or gas payment.

1. Build your cennznet locally, or setup the docker image of cennznet.
2. Launch the local chain using the following command:

```
./target/release/cennznet --chain=dev --base-path=/tmp/cennznet --name=test --validator --alice --unsafe-ws-external --ws-port=9944 --rpc-cors=all
```

`--chain=dev`: Local developer chain will be launched

`--validator` : Launch the chain with a local node as validator. The blockchain cannot produce new blocks if there are no validator connected.

`--alice` : use alice's account as the "origin" of the local node.

`--unsafe-ws-external --ws-port=9944 --rpc-cors=all` : Enables full WebSocket connectivity (unsafe for validators)

## Connecting to your Local test blockchain via UI
Once the local test chain is up and running, you can interact with it using the cennznet webpage UI. 
1. Go to the website: https://cennznet.io/#/settings
2. For the first option: `remote node/endpoint to connect to` select "Local Node" option instead of "CENNZnet"
3. Select "Save and refresh".
You should be able to connect to the local chain now and send transactions and queries via the website UI.

## Enabling Ws/RPC Ports
The following combination of flags will allow the node's RPC and/or websocket functionality to be exposed.
```bash
# Allow ws connections on non-local interface
--ws-external \
# configure ws port
--ws-port 9944
        
# Allow rpc connections on non-local interface
--rpc-external \
# configure rpc port
--rpc-port 9933

# Allow connections from selected origins (default local only)
--rpc-cors
```

## Other Flags

To get detailed descriptions on the flags, run either of the following:
```console
$ docker run cennznet/cennznet:1.1.1 --help  # using docker
$ ./target/release/cennznet -- --help        # using binary
```

## Running nodes with snapshots

Fresh nodes may take days to complete synchronisation. To make the process faster, the CENNZnet team takes snapshots of the nodes.

The [Validator node archives](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/validator/index.html) contains the full history, therefore requires a larger amount of storage space. 
The [Full node archives](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/fullnode/index.html) contain the history of the last 256 blocks. This allows you to interact with the chain, but doesn't have the history of earlier blocks.

To run a node with an archive,
* Download the latest archive
* Unzip the archive
* Add the argument  '-v </path/to/dir/containing/the/unzipped/archive>:/mnt/cennznet' to 'docker run'. This mounts the volume to the docker container, so that the container has access to the archive.
* Add the argument '--base-path /mnt/cennznet' to 'cennznet/cennznet'. This puts the archive in use.

For example,

```bash
docker run -p 9944:9944 -it -v </path/to/dir/containing/the/unzipped/archive>:/mnt/cennznet \
 cennznet/cennznet:1.3.1
 --base-path /mnt/cennznet \
 --chain=/cennznet/genesis/azalea.raw.json \
 --unsafe-ws-external \
 --unsafe-rpc-external --rpc-cors all \
 --telemetry-url 'ws://cennznet-telemetry.centrality.me:8000/submit 0' \
 --name <MY_NODE>
```
