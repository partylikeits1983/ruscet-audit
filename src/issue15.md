# RLP, RUSD, & Yield Asset are non standard SRC20 assets

Severity: Low

Type: Non standard practice 

The RLP, RUSD, & Yield Asset contracts are non standard SRC20 assets since they keep track of user balances within the contract itself.

This is a non-standard practice which can lead to unintended side effects (see issue 16).

[add link to SRC20 implementation]

`_transfer()` function within the RLP contract updates the balance of sender and recipient within storage. 
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
It is recommended to remove the transfer function from the RLP, RUSD, & Yield Asset contracts as asset transfer can be performed via the native fuel UTXO coin transfer.

