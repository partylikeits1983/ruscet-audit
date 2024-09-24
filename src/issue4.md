# Centralization risk in RLP contract

Severity: Informational

Type: Centralization

The contract initializer and added minters can mint / burn any amount of the RLP asset which is a potential source of centralization.

Source: 
1. https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rlp/src/main.sw#L144 

2. https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rlp/src/main.sw#L152

`mint()` and `burn()` functions: 
```rust 
#[storage(read, write)]
fn mint(account: Account, amount: u64) {
    _only_minter();
    _mint(account, amount)
}

#[payable]
#[storage(read, write)]
fn burn(account: Account, amount: u64) {
    _only_minter();
    _burn(account, amount)
}
```

### Recommendation:

Consider removing the ability to mint / burn any arbitrary amount of an asset, and add the call to the `mint()` function in the `initialize()` function of the RLP contract.