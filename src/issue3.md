# RLP, RUSD, & Yield Asset contracts will have asset balance drift between UTXO amounts & contract storage

## RLP, RUSD, & Yield Asset contracts will have asset balance drift between UTXO & contract. The balance of a user in the contract will not necessarily be the balance of the user via the balanceOf method

Severity: High

Type: Business Logic Flaw

In the RLP, RUSD, and Yield Asset contracts, there is a significant business logic flaw in how user balances are tracked. This bug can result in a situation where a user is unable to call the transfer() function for RLP, RUSD, or Yield Asset contracts.

The issue stems from the _transfer() function. The transfer() function updates the caller's and recipient's balances in storage and also calls the transfer_assets() function, which creates a UTXO with the amount transferred to the recipient. The problem arises because the user's asset balance in storage does not necessarily match the actual balance reflected via the balance_of method.

The following test demonstrates how the balance of a user, when calling the contract's get_balance() function, may not reflect the user's actual balance.

```typescript
it("shows contract/utxo balance drift", async () => {
    const RLP = getAssetId(rlp);

    // Step 1: Mint 100 units of the RLP asset to user0
    await rlp.functions.mint(addrToAccount(user0), 100).call();
    
    // Wait for the transaction to be processed
    await delay(1000);

    // Step 2: Retrieve the RLP UTXO balance and contract balance for user0
    const rlpUtxoBalanceUser0 = await user0.getBalance(RLP);
    const rlpContractBalanceUser0 = await call(rlp.functions.balance_of(addrToAccount(user0)));

    // Log the initial balances of user0
    console.log("RLP UTXO balance for user0:", rlpUtxoBalanceUser0.toString()); // 100
    console.log("RLP contract balance for user0:", rlpContractBalanceUser0.value.toString()); // 100

    // Step 3: Transfer the entire 100 RLP from user0 to user1
    await user0.transfer(user1.address, 100, RLP);

    // Wait for the transaction to be processed
    await delay(1000);

    // Step 4: Retrieve and log the UTXO balances for both users after the transfer
    const rlpUtxoBalanceUser0AfterTransfer = await user0.getBalance(RLP);
    const rlpUtxoBalanceUser1AfterTransfer = await user1.getBalance(RLP);

    console.log("RLP UTXO balance for user0 after transfer:", rlpUtxoBalanceUser0AfterTransfer.toString()); // 0
    console.log("RLP UTXO balance for user1 after transfer:", rlpUtxoBalanceUser1AfterTransfer.toString()); // 100

    await delay(1000);

    // Step 5: Check and log the RLP contract balance for user0 after the transfer
    const rlpContractBalanceUser0AfterTransfer = await call(rlp.functions.balance_of(addrToAccount(user0)));
    console.log("RLP contract balance for user0 after transfer:", rlpContractBalanceUser0AfterTransfer.value.toString()); // 100 when it is actually 0

    // Throw an error if the RLP contract balance for user0 does not change
    if (rlpContractBalanceUser0.value.toString() === rlpContractBalanceUser0AfterTransfer.value.toString()) {
        throw new Error("RLP contract balance for user0 did not change after the transfer");
    }
});
```

### Bug scenario
This bug can potentially lead to a situation where a user cannot call the transfer() function the RLP, RUSD, & Yield Asset.

Alice sends 100 tokens to Bob via the transfer() method in the contract.
Bob sends 100 tokens back to Alice via the Fuel UTXO asset transfer method.
Alice calls the transfer() method trying to send assets to Charlie. This transaction will fail, because the sender_balance - amount computation in the contract will lead to underflow.
Suggestion
The suggestion is to remove the storage writes in the transfer() function as well as the get_balance() function from the RLP, RUSD, and yield-tracker contracts, however, this may require additional refactoring to track how other functionality is handled in the contracts.

Affected contracts:

1) https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/assets/rlp/src/main.sw#L218-#L246

2) https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/assets/rusd/src/main.sw#L368#L416

3) https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/assets/yield-asset/src/main.sw#L289-#L337