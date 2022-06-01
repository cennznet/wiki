## Calling the EVM with specified fee preferences

The CENNZnet EVM includes multi-currency fee payment so users can pay transaction fees in any asset, not just CPAY.
To use multi-currency functionality calls should be proxied via a 'fee proxy' precompile, which allows users to specify
their desired fee currency.
One restriction is that there must be liquidity in CENNZX for the fee asset and CPAY. This is due to the fact
that behind the scenes, CENNZX is used to swap the desired fee asset to CPAY (ensuring network fee is always settled in CPAY terms).
Ultimately this means the user doesn't need to hold any CPAY, only their specified or application specific fee asset

To use multi-currency fee payment calls should be wrapped via the `callWithFeePreferences` method using the fee proxy precompile (`0x00000000000000000000000000000000000004bb`)
```solidity
/// @param feeTokenAddress derived address of the generic asset for fee payment
/// @param slippage max price slippage for the fee payment (expressed as numerator of x/1000)
/// @param targetContract the real contract to call
/// @param targetInput the ethereum abi encoded input for targetContract
function callWithFeePreferences(address feeTokenAddress, uint32 slippage, address targetContract, bytes targetInput);
```

To get started, let's take a look a standard ERC20 transfer using CENNZnet's EVM, where fees will be paid using CENNZ.

```solidity
    function transfer(address who, uint256 amount)
```

In this function, there are two input arguments, the receiver and the amount transferred.
This can be called on any ERC20 contract.

To specify the fee preferences when calling this function, we need to encode the function including its parameters using
the Ethereum abi.

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

the value of transferInput is encoded similar to the following:
```
    0xcCccccCc00003E80000000000000000000000000
    7a107fc1794f505cb351148f529accae12ffbcd8
    000000000000000000000000000000000000000000000000000000000000007b
```

`transferInput` can now be used as the input for `callWithFeePreferences`

Now dispatch the CENNZ transfer via the fee proxy precompile
```javascript
    const feeProxyAbi = [
        'function callWithFeePreferences(address asset, uint32 slippage, address target, bytes input)',
    ]

    const feeProxyAddress = '0x00000000000000000000000000000000000004bb';
    const feeProxy = new Contract(feeProxyAddress, feeProxyAbi, cennznetSigner);
    // The asset to use as the fee payment asset
    const cennzTokenAddress = 0xcCccccCc00003E80000000000000000000000000;
    // The slippage value for exchanging between payment asset and CPAY (out of 1000)
    const slippage = 50; // 5%
    // Call the fee proxy contract with the above values and input from the previous example.
    feeProxy.callWithFeePreferences(cennzTokenAddress, slippage, cennzTokenAddress, transferInput);
```
