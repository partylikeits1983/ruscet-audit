# Compiler Warnings

Severity: Informational

Type: Compiler Warnings

When compiling the Rucet contracts there are a significant number of compiler warnings. The majority of these warnings are as a result of the Sway compiler complaining that there is a state change after calling an external contract. 

This is a reasonable issue to produce a warning, however, all the analyzed warnings as a result of this warning are due to the architectural design of the Ruscet contracts, distributing the logic of the application across several contracts.

However, it is recommended that other compiler warnings should be addressed:

### 1) Ignored return type: 

Source: https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault/src/utils.sw#L140

Consider changing to:
```rust
let (_liquidation_state, _margin_fees) = vault_utils.validate_liquidation();
```

### 2) Ignored return type: 

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/utils.sw#L278

Consider changing to:
```rust
let (_liquidation_state, _margin_fees) = vault_utils.validate_liquidation();
```

### 3) Ignored return type

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/utils.sw#L544C9-L544C27

Consider changing to:
```rust
let _amount_after_fees = _decrease_position(
    account,
    collateral_asset,
    index_asset,
    0,
    position.size,
    is_long,
    account,
    false,
    vault_storage_,
    vault_utils_
);
```
