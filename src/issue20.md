# Centralization risk in RUSD contract
 
## Centralization risk in RUSD contract

Severity: Informational

Type: Centralization risk

The `add_vault()` and `remove_vault()` functions allow the contract intializer to change the vault contract address.  The vault contract has the priviledge of calling the `mint()` and `burn()` functions in the RUSD contract. 

`add_vault()` and `remove_vault()` functions in RUSD contract:

https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rusd/src/main.sw#L126-#L136 

```rust
#[storage(read, write)]
fn add_vault(vault: ContractId) {
    _only_gov();
    storage.vaults.insert(vault, true);
}

#[storage(read, write)]
fn remove_vault(vault: ContractId) {
    _only_gov();
    storage.vaults.remove(vault);
}
```

`mint()` and `burn()` functions in RUSD contract:

https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rusd/src/main.sw#L202-#L213 

```rust
#[storage(read, write)]
fn mint(account: Account, amount: u64) {
    _only_vault();
    _mint(account, amount);
}

#[payable]
#[storage(read, write)]
fn burn(account: Account, amount: u64) {
    _only_vault();
    _burn(account, amount);
}
```

### Recommendation

Consider limiting the ability to update the vault contract to a team multisig or voting contract.

