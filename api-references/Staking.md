# Staking API

Staking allows you to make passive income by using your nodes or tokens to take part in our Proof of Stake consensus mechanism.

To learn more about how Staking and the Staking module works, refer to the [Staking Module Documentation](runtime-modules/Staking).

## Common staking queries

Get the active staking era info
```js
(await api.derive.stakingCennznet.overview()).activeEra.toNumber()
```

Get elected validator (stash) addresses only  
```js
(await api.derive.stakingCennznet.validators()).validators
```

Get validator (stash) addresses that are candidates for future elections.  
They could also be elected in the current session or not.  
```js
(await api.query.staking.validators.keys()).map(candidateStashAddress => { .. });
```

Check a validator's commission rate
```js
// the commission rate applied this era
await api.query.staking.erasValidatorPrefs(era, validatorStashAddress)
// the commission rate that will apply next era
await api.query.staking.validators(validatorStashAddress)
```

Check a validator's total, own, and nominated stake
```js
(await api.derive.stakingCennznet.erasStakers(era, validatorStashAddress))
```

## API References

[Staking APIs](https://raw.githubusercontent.com/cennznet/api.js/develop/docs/cennznet/staking.md ':include :type=tsdoc')
