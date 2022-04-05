![CENNZnet](./assets/images/banner.gif)

## Community

Join the official CENNZnet Discord server 🤗

* Get CENNZnet technical support 🛠
* Meet startups and DApp developers 👯‍♂️
* Learn more about CENNZnet and blockchain 🙌
* Get updates on CENNZnet bounties and grants 💰
* Hear about the latest hackathons, meetups and more 👩‍💻

Join the Discord server by clicking on the badge below!

[![Support Server](https://img.shields.io/discord/801219591636254770.svg?label=Discord&logo=Discord&colorB=7289da&style=for-the-badge?logoWidth=24)](https://discord.gg/AnB3tRtkJ4)

## Ecosystem Apps
[Faucet🚰](https://app-faucet.centrality.me)   
Test token faucet

[App Hub ✨](https://app.cennz.net)  
Swap, liquidity, bridge

[Litho Marketplace](https://lithoverse.xyz)   
Create, buy, and sell NFTs 

[Block Explorer](https://uncoverexplorer.com/)  
View network blocks and transactions

[CENNZnet UI](https://cennznet.io/)  
Staking, web wallet & dev tools

[Telemetry](http://cennznet-telemetry.centrality.me/#/0x0d0971c150a9741b8719b3c6c9c2e96ec5b2e3fb83641af868e6650f3e263ef0)  
View network node status and metrics

## Quick Start
start CENNZnet dev node
```bash
docker run -p 9933:9933 cennznet/cennznet:2.1.0 --dev --tmp
```

setup hardhat config 👷‍♀️
```json
// hardhat.config.js
  networks: {
    cennznet: {
      // CENNZnet default RPC port
      url: "http://localhost:9933",
      // CENNZnet dev chainId
      chainId: 2999,
      // CENNZnet dev seed
      accounts: ["0xcb6df9de1efca7a3998a8ead4e02159d5fa99c3e0d4fd6432667390bb4726854"],
    },
  },
```

That's it! you can run scripts and deploy contracts just like a regular Ethereum node. Keep [reading here](EVM/Guide.md) to learn more about building on the CENNZnet EVM  

## Operator Quick Start

Start a full node named `my-node` and connect it to the CENNZnet MainNet:
```bash
# via docker
$ docker run cennznet/cennznet:2.1.0 \
    --chain=/cennznet/genesis/azalea.raw.json \
    --name=my-node \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'

# via source
git clone https://github.com/cennznet/cennznet && git checkout release/2.1.0
cd cennznet && cargo build --release
./target/release/cennznet \
    --chain=./genesis/azalea.raw.json \
    --name=my-node \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:8000/submit 0'
```

Check `my-node` is running [here](http://cennznet-telemetry.centrality.me/#/CENNZnet-Azalea).

*For more details on running a node, see [Running a Full Node](Network-participating/Node-operating/Running-a-Full-Node).*

### Network Snapshots
[Mainnet - Validator / Archive 💾](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/2.0.0/validator/index.html)  
[Mainnet - Full 💾](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/2.0.0/fullnode/index.html)  

### Websocket Endpoints
CENNZnet core team hosted fullnode endpoints for use with `@cennznet/api.js`  
Mainnet:              `wss://cennznet.unfrastructure.io/public/ws`  
TestNet (Nikau 🌴):   `wss://nikau.centrality.me/public/ws`  
TestNet (Rata):       `wss://rata.centrality.me/public/ws` 
