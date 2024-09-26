# Duplicated write to storage in _decrease_position() function

Severity: Informational

Type: Optimization

There is a duplicate call to vault_storage.write_position() in the _decrease_position() function.

Source:

https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/utils.sw#L319

```rust
vault_storage.write_position(position_key, position);
vault_utils.validate_liquidation(
    account,
    collateral_asset,
    index_asset,
    is_long,
    true
);

if is_long {
    vault_utils.increase_guaranteed_usd(collateral_asset, collateral - position.collateral);
    vault_utils.decrease_guaranteed_usd(collateral_asset, size_delta);
}

let price = if is_long {
    vault_utils.get_min_price(index_asset)
} else {
    vault_utils.get_max_price(index_asset)
};

log(DecreasePosition {
    key: position_key,
    account,
    collateral_asset,
    index_asset,
    collateral_delta,
    size_delta,
    is_long,
    price,
    fee: usd_out - usd_out_after_fee,
});
log(UpdatePosition {
    key: position_key,
    size: position.size,
    collateral: position.collateral,
    average_price: position.average_price,
    entry_funding_rate: position.entry_funding_rate,
    reserve_amount: position.reserve_amount,
    realized_pnl: position.realized_pnl,
    mark_price: price,
});

vault_storage.write_position(position_key, position); // duplicate call here
```

### Recommendation:
Consider removing duplicate call to vault_storage on line 319 to reduce the gas cost when calling the _decrease_position() function.