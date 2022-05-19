## Calling the EVM with specified fee preferences

The CENNZnet EVM integration allows for users to call smart contract methods using the CENNZnet runtime.
One downside is that all transaction fees are paid in CENNZnet's native fee currency (CPAY). 

We have allowed for users to call our fee preferences proxy and specify the asset they would like to use to pay the 
transaction fees

To get started, let's take a look at a standard Transfer Solidity transaction using CENNZnet's EVM. This is similar
to calling GenericAsset.transfer, however we are using the EVM as an entry point to the runtime.

```
    function transfer(address who, uint256 amount)
```

In this function, there are two input arguments, the receiver and the amount transferred.
This can be called on any ERC20 contract.

To specify the fee preferences for this function call, we need to encode the function with parameters into
an Ethereum abi.

This can be done using the javascript ethers library with the following:

```javascript
    import { utils } from 'ethers';

    // Defines the functions, used for the abi to encode the values of that function 
    const erc20Abi = [
        'function transfer(address who, uint256 amount)',
    ];
    let iface = new utils.Interface(erc20Abi);
    const transferInput = iface.encodeFunctionData("transfer", [receiverAddress, transferAmount]);
```

the value of transferInput will then be an encoded abi similar to the following:
```
    a9059cbb000000000000000000000000
    7a107fc1794f505cb351148f529accae12ffbcd8
    000000000000000000000000000000000000000000000000000000000000007b
```

This abi can then be used as the input value for when we call our custom fee Proxy and specify fee preferences

This can be done in a similar way as follows:
```javascript
    const feeProxyAbi = [
        'function callWithFeePreferences(uint32 asset, uint32 slippage, address target, bytes input)',
    ]

    const feeProxyAddress = '0x00000000000000000000000000000000000004bb';
    const feeProxy = new Contract(feeProxyAddress, feeProxyAbi, cennznetSigner);
    // The asset to use as the fee payment asset
    const asset_id = 17001;
    // The slippage value for exchanging between payment asset and CPAY (out of 1000)
    const slippage = 50; // 5%
    // Call the fee proxy contract with the above values and input from the previous example.
    feeProxy.callWithFeePreferences(asset, slippage, cennzTokenAddress, transferInput);
```

When the above call (callWithFeePreferences) is called, it will target the FeeProxy precompile and 
exchange the fee asset to CPAY to use for the transaction fee. 
Ultimately this means the user won't need to hold any CPAY, just the asset specified with asset_id. 

