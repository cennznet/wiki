## ERC20 peg precompile

The ERC20 peg precompile allows interacting with CENNZnet's ethereum token bridge.

The peg precompile is available at address:
```solidity
constant ERC20_PEG_PRECOMPILE = address(1939);
```

To withdraw tokens via the ERC20 peg to the bridged Ethereum network use:
```solidity
function withdraw(address asset, uint256 amount, address receiver) -> U256
```
The returned value is a unique proof Id for claiming the tokens on ethereum.
The withdraw proof is available from CENNZnet full node e.g. `using @cennznet/api.js` or the [bridge relayer api](https://bridge-contracts.nikau.centrality.me/proofs/1021) endpoint

```bash
> curl http://cennznet.example.com:9933 -H "Content-Type: application/json" -d '{
     "jsonrpc":"2.0",
      "id":1,
      "method":"ethy_getEventProof",
      "params": [<proofId>]
}'
```