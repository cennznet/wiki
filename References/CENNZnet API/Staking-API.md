Common queries about stake

Get elected validator (stash) addresses only  
```ts
await api.query.staking.validators.currentElected()
```

Get validator (stash) addresses that are candidates for future elections.  
They could also be elected in the current session or not.  
```ts
(await api.query.staking.validators.keys()).map(candidateStashAddress => { //.. });
```

Check a validator's commission rate
```ts
await api.query.staking.validators(stashAddress)
```

Check a validators total, self, and nominated stake
```ts
(await api.query.staking.exposure(stashAddress)).toJSON()
```