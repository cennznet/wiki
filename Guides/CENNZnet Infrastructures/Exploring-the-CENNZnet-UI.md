CENNZnet provides a browser-based UI tool for interacting with the CENNZnet blockchain: [cennznet.io](https://cennznet.io/).

The UI can be used with both the production Azalea blockchain, or for testnets.

## Exploring cennznet.io with a Test Node

Run your develop node:
```
docker run cennznet/cennznet:1.0.0 --chain=dev --name=alice --base-path=/tmp/alice --validator --alice --unsafe-ws-external --ws-port=9944 --rpc-cors=all
```

Open up [cennznet.io](https://cennznet.io/):

[[images/ui/landing-page.png]]

We are currently connected to the production Azalea blockchain - to connect to the local test node:
* navigate to `Advanced -> Settings`
* Select "Local Node" from the drop-down menu
* Click "Save & Reload"

[[images/ui/select-local.png]]

We are now connected to a local node with Alice and Friends as our test accounts.

## Sending funds from Alice to Charlie

To send funds:
* Navigate to "Send assets"
* Select "Alice" and "Charlie" as the sender and receiver respectively
* Click "Make Transfer"
* Click "Sign and Submit" to confirm the transfer

[[images/ui/transfer-asset-0.png]]

For more complex extrinsics, go to `Advanced -> Extrinsics` this provides all available Runtime Module extrinsics on CENNZnet. The transfer we just made can be achieved using a `GenericAssets.transfer` extrinsic call.

## Check Balances

To check Chalie's CENNZ balance:
* Navigate to `Advanced -> Chain state`.
* Select `genericAsset` and `freeBalance`
* For `AssetId` use 16000 (CENNZ)
* Choose Charlie for the `AccountId`
* Press the plus sign, Charlie's balance will show up

[[images/ui/chain-state.png]]