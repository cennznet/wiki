# EVM Guide
---

CENNZnet is EVM compatible allowing developers to bring existing contracts and dapps to get started quickly.

Familiar tools like web3.js, ethers, metamask, and Remix IDE work out of the box by simply changing your endpoint to a CENNZnet network.


Try the [faucet🚰](https://app-faucet.centrality.me) to add networks to your wallet and claim tokens quickly

There are some small differences, compared to Ethereum, to be aware of when building on CENNZnet explained below (marked with 👀)

## Networks 🌐

**Nikau** - Stable testnet, tracks the mainnet version

**Rata** - Chaos testnet, has the latest features early in the release cycle

| name    | chain id | rpc                              |
|---------|----------|-------------------------------------------|
| Rata    | 3000     | https://rata.centrality.me/public         |
| Nikau   | 3001     | https://nikau.centrality.me/public        |
| Mainnet | 21337    | https://cennznet.unfrastructure.io/public |


The CENNZnet core team provides the above full node endpoints, rate limited at 20 requests/second.

## CENNZ & CPAY

CENNZnet uses a dual token system. A governance/value token *CENNZ* and a fee token *CPAY*
The native currency used by Ethereum RPCs on CENNZnet for: balances, paying gas fees, and value transfers is ***CPAY***  

👀 CPAY is a 4dp token, while ethereum tooling like metamask and solidity expect native token amounts in wei or 18dp units. 
To keep compatibility simple CENNZnet takes care of scaling inputs down by `1e-14` and scaling up outputs by `1e14`.
Any fractional amounts < `1e14` / `0.0001 CPAY` are always rounded up only by adding `1e14`

**CENNZ** is available as an ERC20 token at:
- **Mainnet** address: `0xcCcCCccC00000001000000000000000000000000`
- **Testnet** address: `0xcCccccCc00003E80000000000000000000000000`

CENNZ is also a 4dp token but does not require any special handling as per the ERC20 standard.


### CPAY Transfer Examples
The following line of solidity code transfers `1 CPAY` to the `receiver`
```solidity
receiver.call{value: 1 ether}("")
```

Transfers `1.0001 CPAY` to `receiver` (i.e rounds up by `0.0001 CPAY`)
```solidity
receiver.call{value: 1 ether + 1 wei}("")
```


The following ethers transaction transfers `0.0001` CPAY to the receiver
```typescript
await wallet.sendTransaction({
  to: receiver,
  value: 1,
});
```

transfers `1.2` CPAY to the receiver
```typescript
await wallet.sendTransaction({
  to: receiver,
  value: ethers.utils.parseEther('1.2'),
});
```

### CPAY Recap
- Native token amounts and transfers are handled by CENNZnet transparently, so no special handling or conversion is required by developers
- input and output amounts are expressed in wei (18dp) as normal
- amounts (or fractions of amounts) transferred < `1e14 `are always rounded up by adding `1e14` / `0.0001 CPAY`

## Transaction Fees ⛽️  
CENNZnet supports EIP-1559 and legacy transaction fee models.
Ethereum tooling is able to query fee estimation info accurately out of the box without any special handling required.
The `eth_feeHistory` RPC is provided for this purpose see its docs for more details https://docs.alchemy.com/alchemy/apis/ethereum/eth_feehistory & https://gist.github.com/zsfelfoldi/473e29106d38525de6b4413e2ebcddf1

👀 Priority fees may appear to be high when expressed in gwei i.e.`5000 gwei`+ recall this number is converted into a nominal CPAY value ~ `<0.0001 CPAY`

## Addresses
EVM contract developers on CENNZnet can use existing ethereum wallets and addresses without any changes.

CENNZnet has functionality outside the EVM, otherwise known as the runtime.
The runtime uses a different addressing scheme to the EVM.
To convert a 20 byte ethereum address to its CENNZnet equivalent

Use this conversion function:
```javascript
const utils = require('@polkadot/util');
const utilCrypto = require('@polkadot/util-crypto');

function cvmToAddress(cvmAddress) {
  var message = utils.stringToU8a('cvm:');
  message = utils.u8aConcat(message, new Array(7).fill(0), utils.hexToU8a(cvmAddress));
  let checkSum = message.reduce((a, b) => a ^ b, 0);
  message = utils.u8aConcat(message, new Array(1).fill(checkSum));

  return utilCrypto.encodeAddress(message, 42);
}
```
The address may then be used via services like the [uncover explorer](https://uncoverexplorer.com)


## Using an Ethereum keypair with CENNZnet

Existing CENNZnet dapps (non-evm) can now support ethereum wallets such as Metamask.
Using the address conversion function above a CENNZnet address is derivable from any existing ethereum address.
To enable signing CENNZnet runtime transactions from an Ethereum wallet the `EthWallet` module is provided

The ethereum wallet can act as the derived CENNZnet address (attainable via  `cvmToAddress(ethAddress)`). The derived address must have sufficient CPAY for tx fees.

```typescript
// Send a message to CENNZnet
import { Api } from '@cennznet/api';

const cennznet = await Api.create({network: 'nikau'});
// Build the CENNZnet transaction payload
let nonce = await cennznet.rpc.system.accountNextIndex(ethAddress);
let runtimeCall = cennznet.tx.system.remark(utils.stringToHex(`Hello from Metamask!: ${ethAddress}`));
let payload = cennznet.createType('ethWalletCall', { call: runtimeCall, nonce }).toU8a();

// Request signature from metamask
let signature = await ethereum.request({ method: 'personal_sign', params: [payload, ethAddress] });

// Broadcast the tx to CENNZnet
let txHash = await cennznet.tx.ethWallet.call(call, ethAddress, signature).send();
```

## Interacting with the Runtime from EVM (Precompiles)

CENNZnet uses a precompile system to allow communication between the CENNZnet runtime and EVM contracts.
This allows native CENNZnet currencies and NFTs to appear as regular ERC20s and ERC721s to ethereum tooling and contracts.
Future protocol enhancements will add further functionality to supercharge contract development suchas as scheduling, cennzx liquidity, ethereum bridging and more.

## Working with Generic Assets
Generic assets (GAs) implement the ERC20 abi. This allows them to be fully programmable and used anywhere a normal ERC20 token can.
To interact with a given generic asset first derive its corresponding contract address:  
```typescript 
import web3 from 'web3';
import { numberToHex } from '@polkadot/util';
let assetId = '<YOUR_ASSET_ID>'; // e.g. 16001
let tokenContractAddress = web3.utils.toChecksumAddress("cccccccc" + numberToHex(assetId));
```


The following code snippet interacts with the testnet CENNZ token address using its ERC20 abi.

?> **Important** use solidity `^0.8.13`

```solidity
pragma solidity ^0.8.13;
import "@openzeppelin/contracts/interfaces/IERC20";

// CENNZ testnet address with assetId 16000
address cennz = 0xcCccccCc00003E80000000000000000000000000

function balanceOfProxy(address who) public view returns (uint256) {
    return IERC20(cennz).balanceOf(who);
}

function transferFromProxy(uint256 amount) external {
    IERC20(cennz).transferFrom(msg.sender, address(this), amount);
}
```
