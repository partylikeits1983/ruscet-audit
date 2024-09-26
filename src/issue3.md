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