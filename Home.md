![CENNZnet](./images/banner.png)

## Intro
CENNZnet is a public blockchain based on Proof of Stake (PoS) consensus built on top of the [Plug](https://github.com/plugblockchain/plug-blockchain/) framework.

The PoS algorithm facilitates fairness compared to the widely used Proof of Work algorithm through a process of staking.

It also allows CENNZ holders to participate in the process of staking and get rewarded for it.
CENNZnet implements a decentralised currency exchange (CENNZX), fully managed by users, providing spot price trades and integrated multi-currency fee payment.

Decentralized applications (DApps) built on CENNZnet can seamlessly interact with the users of other DApps.

## Community

Join our official CENNZnet Discord server 🤗

* Get CENNZnet technical support 🛠
* Meet startups and DApp developers 👯‍♂️
* Learn more about CENNZnet and blockchain 🙌
* Get updates on CENNZnet bounties and grants 💰
* Hear about the latest hackathons, meetups and more 👩‍💻

Join the Discord server by clicking on the badge below!

[![Support Server](https://img.shields.io/discord/801219591636254770.svg?label=Discord&logo=Discord&colorB=7289da&style=for-the-badge)](https://discord.gg/AnB3tRtkJ4)


## Quick Start

Start a full node named `my-node` and connect it to the CENNZnet MainNet:
```bash
# via docker
$ docker run cennznet/cennznet:1.3.0 \
    --chain=/cennznet/genesis/azalea.raw.json \
    --name=my-node \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:1024 0'

# via source
git clone https://github.com/cennznet/cennznet && git checkout release/1.3.0
cd cennznet && cargo build --release
./target/release/cennznet \
    --chain=./genesis/azalea.raw.json \
    --name=my-node \
    --telemetry-url 'ws://cennznet-telemetry.centrality.me:8000/submit 0'
```

Check `my-node` is running [here](http://cennznet-telemetry.centrality.me/#/CENNZnet-Azalea).

*For more details on running a node, look at [Running a Full Node](Guides/Node-operators/Running-a-Full-Node).*

## Unfrastructure Links
Centrality hosted CENNZnet full nodes and ecosystem services

Block explorer API: https://service.eks.centralityapp.com/cennznet-explorer-api/api/  
Nikau full nodes:   wss://nikau.centrality.me/public/ws  
Mainnet full nodes: wss://cennznet.unfrastructure.io/public/ws  
CENNZnet UI:        https://cennznet.io/  
Block Explorer:     https://uncoverexplorer.com/  

Mainnet DB snapshots:
- https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/validator/index.html  
- https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/fullnode/index.html  

