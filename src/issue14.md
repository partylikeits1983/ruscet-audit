# RLP, RUSD, & Yield Asset use non standard SRC20 implementation

Severity: Low

Type: Non standard practice 

The RLP, RUSD, & Yield Asset contracts are non standard SRC20 implementations since they keep track of user balances within the contract itself. The contracts are non standard because they have a transfer function which conflicts with fuel's concept of native assets and the underlying UTXO model.

This is a non-standard practice which can lead to unintended side effects ([see issue 3](https://github.com/burralabs/ruscet-contracts/issues/3)).

SRC20 example implementation: https://docs.fuel.network/docs/sway-standards/src-20-native-asset/#example-implementation

The `_transfer()` function within the RLP, RUSD, & Yield Asset contracts updates the balance of sender and recipient within storage. This will lead to unintended discrepancies between a user's true asset balance and the balance of the user written in storage of the SRC20 contract and can potentially make the `_transfer() ` uncallable for some users.

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rlp/src/main.sw#L220-#L246
```rust
#[payable]
#[storage(read, write)]
fn _transfer(
    sender: Account,
    recipient: Account,
    amount: u64
) {
    require(sender != ZERO_ACCOUNT, Error::RLPTransferFromZeroAccount);
    require(recipient != ZERO_ACCOUNT, Error::RLPTransferToZeroAccount);

    require(
        amount == msg_amount(),
        Error::RLPInsufficientTransferAmountForwarded
    );

    let sender_balance = storage.balances.get(sender).try_read().unwrap_or(0);
    require(sender_balance >= amount, Error::RLPInsufficientBalance);

    storage.balances.get(sender).write(sender_balance - amount);
    storage.balances.get(recipient).write(
        storage.balances.get(recipient).try_read().unwrap_or(0) + amount
    );

    transfer_assets(
        msg_asset_id(),
        recipient,
        amount
    );
}
```

### Suggestion
It is recommended to remove (or significantly modify) the transfer function from the RLP, RUSD, & Yield Asset contracts as asset transfer can be performed via the native fuel UTXO coin transfer. 

In order to maintain the staking rewards functionality, it may be possible to expose the `_update_rewards()` function as a public method in the RUSD contract, so that any user can call the function and receive staking rewards. 

It may also be possible to add a method that allows a user to "sync" their balance stored in the contract with the actual asset balance of the native asset of their EoA.

Fuel Docs:
https://docs.fuel.network/docs/sway/blockchain-development/native_assets/#erc-20-vs-native-asset
