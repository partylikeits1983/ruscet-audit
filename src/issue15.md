# Possibility to front run transactions that call the `_transfer_in()` function in the vault contract

Severity: Low

Type: Front Running

It is possible to front run transactions that call the `_transfer_in()` function if the transaction call pattern seen in the `/tests` directory is used in production. 

In the tests, when calling payable functions, there are two transactions that occur: 

1) Asset transfer to the contract
2) Function call in the contract

This transaction call pattern if used in production is possible to front run as a result of the logic in the `_transfer_in()` function.

`_transfer_in()` function: https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault/src/internals.sw#L136

```rust
pub fn _transfer_in(asset_id: AssetId, vault_storage_: ContractId) -> u64 {
    let vault_storage = abi(VaultStorage, vault_storage_.into());
    
    let prev_balance = vault_storage.get_asset_balance(asset_id);
    let next_balance = balance_of(ContractId::this(), asset_id);
    vault_storage.write_asset_balance(asset_id, next_balance);

    require(
        next_balance >= prev_balance,
        Error::VaultZeroAmountOfAssetForwarded
    );

    next_balance - prev_balance
}
```

The `_tranfer_in()` function reads the previous asset balance value from storage and the current asset balance using the sway-lib method `balance_of()`. The function returns the difference of the current balance and the previous balance. The difference is considered to be the amout sent by the user to the contract. 

The issue arises in how the tests in the repository are set up. All of the tests in the repostory that call functions that then call the `_transfer_in()` function, do not use the fuels-ts method to transfer assets during the function call. In production, it is possible to front run all transactions that follow this call pattern:

```typescript
await transfer(BNB.connect(user0), contrToAccount(vault), 100); // transfer 

await call(vault.functions.buy_rusd(toAsset(BNB), addrToAccount(user1)).addContracts(attachedContracts)); // call
```

Front running discussion on fuel: https://forum.fuel.network/t/front-running-within-the-fuelvm/6633

### Suggestion

Edit all of the tests that call payable functions to use the fuels-ts method to transfer assets during the function call to ensure the smart contracts are tested in a way that matches how they will be used in production. 

Another consideration is to use the the sway-libs `msg_amount()` method to get the exact amount of the asset transfered into the contract instead of computing the difference of the previous stored balance and the current balance.

Example: 
```rust
use std::{
    context::msg_amount,
};

#[payable]
#[storage(read, write)]
fn transfer_in() -> u64 {
    let amount = msg_amount();
    require(amount > 0, ValueError::InvalidAmount);

    let asset = msg_asset_id();
    require(asset == AUTHORIZED_ASSET, AssetError::InvalidAsset);

    // update storage & balances

    return amount;
}
```