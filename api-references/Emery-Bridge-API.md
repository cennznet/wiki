# Emery Bridge API

The Emery Bridge API allows you to securely transfer tokens between CENNZnet and Ethereum. It allows two-ways transfers of ERC-20 tokens, Native ETH, data and assets (like NFTs).

For an intro to the Emery Bridge refer to our [Knowledge Hub article](https://cennz.net/knowledge-hub/core-modules/emery-cennznet-ethereum-token-bridge/).

## The components of the Emery bridge

The Emery bridge is a complex mechanism that ensures security and efficiency. It consists of **a bridge contract on Ethereum**, as well as **2 additional runtime modules on CENNZnet**. It uses the CENNZnet network validators to verify bridge transactions.

* **The Eth-bridge module** provides the infrastructures to transfer any data or event between Ethereum and CENNZnet.

* **The ERC20-Peg module** works on top of the Eth-bridge module. It handles events on Ethereum and CENNZnet, and then locks and unlocks tockens on the two chains. It matches specific events related to ERC20 tokens and Generic Assets.

As a user of the Emery bridge, you will only need to interact with the ERC20-Peg module and the bridge contact on Ethereum.

## How the transferring works

The CENNZnet bridge is a decentralized relay maintained by existing validators. 

When an Ethereum event is to be proved on CENNZnet, validators independently witness transactions on Ethereum and sign attestations. Consensus will be reached for or against the event.

Conversely, when a CENNZnet event is to be proved to Ethereum, an ethereum contract can provide proof to the CENNZnet bridge contract which will verify it’s authenticity. The brige contract maintains the public keys of the current and historic CENNZnet validators.

The bridge contact never mints tokens on Ethereum, but only unlocks them. Tokens must first be locked on Ethereum before they can be unlocked on CENNZnet and transferred back again. This means the liquidity depends on the current deposits held by the bridge contract. 

## Addresses and repos
The Emery Bridge is now available for testing on the test networks of Ethereum and CENNZnet.

Ropsten peg contract address: `0xdCbBBDf43fdeE6540B6467BA24f9AE6042FA266C`

Brige contracts repository: https://github.com/cennznet/bridge-contracts/

Rata websocket endpoint: `wss://kong2.centrality.me/public/rata/ws`

CENNZnet api version: `@cennzent/api@1.5.0-alpha.2`

Native ETH deposit: https://ropsten.etherscan.io/tx/0x4c1cd625a35fee124d254904dffc9514edc4bbdc46b4c3c97baf0da840da1559


## Transfering from Ethereum to CENNZnet

To transfer from Ethereum to CENNZnet, you will need to complete the following steps:
1. Send a deposit transaction to a bridge contract on Ethereum containing the following:
* The destination CENNZnet address
* Amount
* Token type (ERC20 token address or native ETH)

2. Send a `depositClaim` transaction to the CENNZnet `erc20Peg` module

```js
let depositTxHash = "0xcdb32cc1892d23068e24233f41d08b5cdf5d317c622c920aa70c1390a6cf3478";
let claim = {
    tokenAddress: "0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9",
    amount: "5644",
    beneficiary: "0xd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"
};
let nonce = await api.rpc.system.accountNextIndex(alice.address);
await api.tx.erc20Peg.depositClaim(depositTxHash, claim).signAndSend(alice, {nonce}, async ({status, events}) => {
    if (status.isInBlock) {
    for (const {event: {method, section, data}} of events) {
        console.log('\t', `: ${section}.${method}`, data.toString());
    }
    }
});
```
3. Wait a couple of minutes for the notarization of the transaction.

The secure process works as follows. The CENNZnet bridge module will wait for confirmations on Ethereum and create a notarization claim against the deposit if valid. The CENNZnet validators will query the transaction against Ethereum full nodes. The CENNZnet bridge module then waits for at least 2/3rds of validators to notarise a deposit successfully, before minting the correct amount of tokens to the nominated CENNZnet address.

### Examples
[Code example](https://github.com/cennznet/bridge-contracts/blob/main/scripts/deposit.js)
[Deposit example (ropsten)](https://ropsten.etherscan.io/tx/0x6b3643e007d0a130c59471b523bf2cf4fc81c56928f999827a09006ace3b0bf7)
[Deposit claim example (rata)](https://cennznet.io/#/explorer/query/0xd51f63a0adf5741c8b4e80905b02b7557fe3a4df514bb149ef1192b26903b177)



## Transfering from CENNZnet to Ethereum

To transfer from CENNZnet to Ethereum, you will need to complete the following steps:
1. Send a `withdraw` transaction to the CENNZnet `erc20Peg` module containing the following:
* Recipient Ethereum address
* Asset Id
* Amount

```js
let nonce = await api.rpc.system.accountNextIndex(alice.address);
      let amount = 5644;
      const ethBeneficiary = '0x70997970c51812dc3a010c7d01b50e0d17dc79c8';
       await api.tx.erc20Peg.withdraw(testTokenId2, amount, ethBeneficiary,).signAndSend(bob, {nonce}, async ({status, events}) => {
        if (status.isInBlock) {
          for (const {event: {method, section, data}} of events) {
            console.log('\t', `: ${section}.${method}`, data.toString());
            if (section === 'erc20Peg' && method == 'Erc20Withdraw') {
              let count = 0;
              const unsubHeads = await api.rpc.ethy.subscribeEventProofs((result: any) => {
                console.log('data::', result.toHuman());
                if (count++ == 1) {
                  unsubHeads();
                  done();
                }
              });
            }
          }
        }
      });
```
2. Wait for the validators to sign a message and issue a withdrawal proof. Note: you can use the method `api.rpc.ethy.subscribeEventProofs` to get the withdrawal proof.
3. Submit the withdrawal proof to the bridge contract on Ethereum. This will release the tokens to the intended recipient address.

Note: Any address may submit a valid withdrawal proof. Only the intended receipient will be paid.

## Complete code examples

The [Bridge API end to end test](https://github.com/cennznet/api.js/blob/eeb672ef39f6922512a4aaf8c393b4d1131a1ffc/packages/api/test/e2e/ethBridge.e2e.ts) demon strates how to use the Emery Bridge to deposit and withdraw.