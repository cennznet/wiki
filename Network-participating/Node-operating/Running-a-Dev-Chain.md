# Running a development chain

During development, you may want to interact with a local test net to test your program before connecting to the main blockchain. Interacting with the real blockchain results in in-reversible transactions, and almost all interactions will require some amount of payment, either as a form of transaction fees, or gas payment.

### Running from docker image
* Install [Docker](https://www.docker.com/)
* Run the command:
```bash
docker run -p 9944:9944 cennznet/cennznet:1.4.0 --dev --ws-external
```

### Running a local build of CENNZnet
[build CENNZnet from source](Network-participating/Node-operating/Running-a-Full-Node?id=building-from-source)

```bash
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
