# Example JS App

We have created an example JS app with a basic CLI that provides you with examples of using CENNZnet's API.
You can find the [code repository here](https://github.com/cennznet/api-example-app).

Before you start here, please look through the [Cennznet JS API getting started page](https://github.com/cennznet/cennznet/wiki/Interact-with-CENNZnet--using-the-API)

You will need to install Node.js (>=10.16.3) and Yarn (>= 1.19.0).

Items covered in this guide include:
- How to launch the JS example app.
- How to use the example app and find more information.
- Example use-case of exploring the CENNZnet's API using the example app.

We will cover 3 example use-cases:
- Sending and Receiving Assets using the [Generic Assets](API-examples-Generic-Assets) module
- Trading in the [CENNZX Spot Exchange](#cennzx-spot-exchange)
- Setting and checking for claims using [Attestation](https://github.com/cennznet/cennznet/wiki/API-examples-Attestation)

## How to get started with the JS app
1. Clone the example app [code repository](https://github.com/cennznet/api-example-app)

```
git clone git@github.com:cennznet/api-example-app.git
```

2. Navigate to your project folder 

```
cd api-example-app
yarn
cd src
```

Now you are all set up. For the app to work, you need to run a local test chain to interact with.
Follow the instruction here [to setup a local dev chain](https://github.com/cennznet/cennznet/wiki/Running-a-Full-Node#to-run-a-test-blockchain-locally).

You can run the example app now.

```
node index.js --help
```

You should see a list of supported commands and how to use them.

## How to use the JS app
You can call the functions provided in this app to interact with your blockchain. To call a function, append the name of the function and parameters as arguments.

```
node index.js balances
```
You should see some results returned from querying the chain on current balances for A(lice) and B(ob).

For each of the supported commands, some parameters may be options. You can get more info on it by:

```
node index.js transfer -h
```
You should now see more info about how to use this command.

```
usage: index.js transfer [-h] to amount [asset_id] [sub]

Positional arguments:
  to          Who's account to tranfer TO
  amount      The Amount to transfer
  asset_id    The Asset ID to transfer
  sub         Substribe to events regarding this transfer.
```

Arguments with [] mean they are optional. If they are not entered, a default value will be used.
i.e.[asset_id] will be defaulted to 16000.

If you want to find out more about how the JS API is being called, you can find the source code inside their dedicated modules. For example, the "transfer" command can be found inside the "generic_asset.js" file.


## Example use-cases
The example app provides you with some example code on how to use some of the JS API to interact with CENNZnet. 
In this section, we will use some example use-cases to demonstrate how this app can be used.

### Sending and receiving Assets
Use case: Alice want to send 2,000,000 units of asset ID:16000 to Bob. For this, you can use the "transfer" call.
1. Check the starting balances:

```
node index.js balances

Response:
Asset:16000: A: 1000000000000000000000000000, B:1000000000000000000000000000
```

2. Make the transfer transaction:

```
node index.js transfer bob 987654321 16000 true


Response:
Current status is Ready
Current status is InBlock
Current status is Finalized
Transaction included at blockHash 0xadccc68ac731d5e1aea2c76a94103e48a5aac8464cdc8700804751d45190a826
	' {"ApplyExtrinsic":2}: genericAsset.Transferred:: [16000,"5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY","5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty",987654321]
	' {"ApplyExtrinsic":2}: system.ExtrinsicSuccess:: [{"weight":10000,"class":"Normal","paysFee":true}]
Transfer with sub to events. Amount:987654321, Asset:16000
Transaction is Successful
```

3. Check the new balances

```
node index.js balances

Response:                     
Asset:16000: A: 999999999999999999012345679, B:1000000000000000000987654321
```
As you can see, Alice has transferred some money into Bob's account.

### Trading in the CENNZX spot exchange
(Note: The concept of "Asset" is similar to the idea of different Currencies, e.g. NZD, USD, JPY)
Use case: Alice wants to exchange some Asset with ID 16000 into Asset with ID 16001

1. Ensure there are liquidity in the exchange pool

[TODO]: Replace this part with an API app command

The easiest way to add liquidity is via the Cennznet GUI. 
Go to https://cennznet.io/#/extrinsics
Add liquidity to the exchange pool of 16000 and 16001.

![add_liquidity](../../assets/images/ui/add_liquidity.png)

In this example, we will add 1,000,000,000 units of 16000, and 200,000,000 of 16001.
The exchange rate between 16000 and 16001 will be expected to be around 5:1

2. Make the exchange
In our demo app call:

```
node index.js buy_asset alice 16000 16001 100

Response:
Current status is Ready
Buying asset:16001, amount:100
Current status is InBlock
Current status is Finalized
Transaction included at blockHash 0x6f35cb9bcf79285695c6e77c14a09721893a9d2d6a0747d5cb05241c00e3a1c6
	' {"ApplyExtrinsic":2}: cennzxSpot.AssetPurchase:: [16000,16001,"5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY",502,100]
	' {"ApplyExtrinsic":2}: system.ExtrinsicSuccess:: [{"weight":10000,"class":"Normal","paysFee":true}]
```
Alice has bought 100 unit of 16000 with 502 units of 16001, and the exchange rate was very close to the expected 1:5

### Setting and checking for claims using the Attestation module
Use case: Alice wants to make some "Claim" about her opinion about Bob.
1. Create the claim
Alice claims that bob's "coolness" has a value of "average"
```
node index.js set_claim bob coolness average
```


2. Query the claim 

```
node index.js query_claim bob coolness

Response:
Claim about:bob on topic:coolness
Has value:average
```

3. Update the claim

```
node index.js set_claim bob coolness super-duper
```

4. Query the new value

```
node index.js query_claim bob coolness

Response:
Claim about:bob on topic:coolness
Has value:super-duper
```
The value has been updated.


5. Removing the claim

```
node index.js remove_claim bob coolness
```

6. Check if the claim is removed

```
node index.js query_claim bob coolness 

Response:
Claim about:bob on topic:coolness
Has value:
```
The value has been removed.
