# Redundant storage read in a loop 

Severity: Informational

Type: Optimization

In multiple parts of the codebase there are redundant reads to a variable within a loop leading to increased gas usage.

Source: 

1) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-pricefeed/src/main.sw#L464 

2) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rusd/src/main.sw#L184 

3) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/rusd/src/main.sw#L195 

4) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/yield-asset/src/main.sw#L164 

5) https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/yield-asset/src/main.sw#L175

### Recommendation

Consider setting the storage read as a variable, and using the variable in the while loop to reduce gas across the codebase.

```rust
let price_sample_space = storage.price_sample_space.read();
while i < price_sample_space {
    // loop
}
```

as opposed to:

```rust
while i < storage.price_sample_space.read() {
    // loop
}
```