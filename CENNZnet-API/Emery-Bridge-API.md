# Emery Bridge API

The Emery Bridge API allows you to securely transfer tokens between CENNZnet and Ethereum. It allows two-ways transfers of ERC-20 tokens, Native ETH, data and assets (like NFTs).

For an overview of what the Emery Bridge does and how it works, refer to our [Knowledge Hub article](https://cennz.net/knowledge-hub/core-modules/emery-cennznet-ethereum-token-bridge/).

## The components of the Emery bridge

The Emery bridge is a complex mechanism that ensures security and efficiency. It consists of **a bridge contract on Ethereum**, as well as **2 additional runtime modules on CENNZnet**. It uses the CENNZnet network validators to verify bridge transactions.

* **The Eth-bridge module** provides the infrastructures to transfer any data or event between Ethereum and CENNZnet.

* **The ERC20-Peg module** works on top of the Eth-bridge module. It handles events on Ethereum and CENNZnet, and then locks and unlocks tockens on the two chains. It matches specific events related to ERC20 tokens and Generic Assets.


## Transfering from Ethereum to CENNZnet

To transfer from Ethereum to CENNZnet, you will need to complete the following steps:
1. Send a deposit transaction to a bridge contract on Ethereum containing the following:
* The destination CENNZnet address
* Amount
* Token type (ERC20 token address or native ETH)

? Question, how do people find this ETH contract? with remix?

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
3. Wait for a couple of minutes for the notarization of the transaction.

The secure process works as follows. The CENNZnet bridge module will wait for confirmations on Ethereum and create a notarization claim against the deposit if valid. The CENNZnet validators will query the transaction against Ethereum full nodes. The CENNZnet bridge module then waits for at least 2/3rds of validators to notarise a deposit successfully, before minting the correct amount of tokens to the nominated CENNZnet address.



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
      await api.tx.erc20Peg.withdraw(testTokenId2, amount, ethBeneficiary,).signAndSend(alice, {nonce}, async ({status, events}) => {
        if (status.isInBlock) {
          for (const {event: {method, section, data}} of events) {
            console.log('\t', `: ${section}.${method}`, data.toString());
          }
        }
      });
```
2. Wait (how long?) for the validators to sign a message and issue a withdrawal proof.
3. Submit the withdrawal proof to the bridge contract on Ethereum. This will release the tokens to the intended recipient address.


## Complete code examples

The [Bridge API end to end test](https://github.com/cennznet/api.js/blob/eeb672ef39f6922512a4aaf8c393b4d1131a1ffc/packages/api/test/e2e/ethBridge.e2e.ts) demon strates how to use the Emery Bridge to deposit and withdraw.