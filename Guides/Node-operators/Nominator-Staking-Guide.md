Staking user tokens on CENNZnet AKA _nominating_, is the process of placing tokens at stake behind validator accounts.  
It allows ordinary token holders to participate in securing the network by supporting validator nodes with their tokens.  
In return, for locking their tokens the user will share a pro-rata Cpay reward with their chosen validators (assuming those validators are elected). Equally, the nominator's tokens are at risk of slash if the validator commits a serious offense.  
This ensures nominators can share in the responsibility of securing the network by electing good validators.  

Nominating is a simple two step process:
1) User sends a transaction to bond their CENNZ into the staking system
2) User sends transaction to choose their preferred validators

## Nominating
Before nominating, the user should generate two keypairs, a stash and a controller.  
The stash holds funds to stake, while the controller is used to manage the stash's preferences.  
This separation allows the stash keypair to be kept secure and used less frequently.  
Note: The controller could also be delegated to a 3rd party DApp account which manages the stash on behalf of the user.  

Once tokens are staked they are locked and will be unusable for other network tasks e.g. transfers, fee payment, etc.

These are the transactions required to successfully stake user tokens.  
```ts
// 1) Bond: put 10,000 CENNZ tokens at stake, signed by stashKeypair
let stakeAmount = 100000000;
await api.tx.staking.bond(controllerAddress, stakeAmount, rewardAddress).signAndSend(stashKeypair);

// 2) Nominate: Choose preferred validators to nominate, signed by controllerKeypair
await api.tx.staking.nominate([validator1Address, validator2Addresss]).signAndSend(controllerKeypair);
```

The user may change their nominations at anytime by issuing another `nominate` transaction. The effect will be applied after the next validator election.  


## Retrieving Stake
This is the process to exit staking and retrieve tokens.  
There is a delay period ("unlocking") before tokens maybe retrieved. This acts as a safety mechanism to ensure any slashes are correctly applied before stake is removed from the system.  

The user must first signal they want to withdraw some funds from the staking system.  
This will schedule them to unlock, and will be unusable for staking during this time. 
After the unlocking period completes their funds can be withdraw by issuing a final transaction to reclaim them.  
```ts
// 1) unbond some funds, scheduling them to be unlocked
await api.tx.staking.unbond(amountToUnbond).signAndSend(controllerKeypair);
// 2) Release funds for normal use, call after unlocking period ends
await api.tx.staking.withdrawUnbonded().signAndSend(controllerKeypair);
```

Lastly, it is possible to cancel the unlocking operation of some funds and instead place them back at stake by issuing a `rebond` transaction e.g:
```ts
await api.tx.staking.rebond(rebondAmount).signAndSend(controllerKeypair);
```



 