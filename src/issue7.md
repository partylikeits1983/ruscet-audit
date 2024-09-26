# Unused variables in function input `get_funding_fee()`

Severity: Informational

Type: Optimization

The function `get_funding_fee()` has unused function input parameters. The following parameters, `account`, `index_asset`, and `is_long` are not used.


`get_funding_rate()`: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-utils/src/main.sw#L262

```rust
#[storage(read)]
fn get_funding_fee(
    account: Account,
    collateral_asset: AssetId,
    index_asset: AssetId,
    is_long: bool,
    size: u256,
    entry_funding_rate: u256,
) -> u256 {
    _get_funding_fee(
        collateral_asset,
        size,
        entry_funding_rate
    )
}
```

### Recommendation

Remove unused input parameters and refactor other functions that call the `get_fundint_rate()` function.

