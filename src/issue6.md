# Centralized Control in RLP Asset Minting/Burning

Centralized Control in RLP Asset Minting/Burning
Severity: Informational

Type: Centralization

The contract initializer and any added minters have the ability to mint or burn unlimited amounts of the RLP asset. This introduces a centralization risk where these privileged accounts can arbitrarily affect the total supply of the asset, potentially undermining trust in the system and creating governance risks.

Source:
1) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rlp/src/main.sw#L144

2) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rlp/src/main.sw#L151

mint() and burn() functions:

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
Consider implementing the following changes to mitigate centralization risk:

Remove the ability for any arbitrary amount to be minted or burned by a privileged account.
In the initialize() function, allow an initial minting of the asset to set the initial supply, and then lock minting/burning behind decentralized mechanisms such as multisig or governance votes.
Implement access control mechanisms, like a time delay or community voting, to limit the power of minters and reduce centralization risks.
