# Unused Imports

Severity: Informational

Type: Optimization

There are several instances of unused imports.

1) Position is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/internals.sw#L16

2) Account is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-storage/src/events.sw#L5

3) VaultPricefeed is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-storage/src/main.sw#L40

4) tai64_timestamp is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-utils/src/main.sw#L17

5) Position is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-utils/src/main.sw#L34

6) mint_to is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/assets/time-distributor/src/main.sw#L16

7) VaultUtils is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/main.sw#L37

8) VaultStorage is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/main.sw#L39

9) PositionKey is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/main.sw#L41

10) transfer_assets is unused: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault/src/utils.sw#L24


### Recommendation
 
Consider removing the unused imports.

