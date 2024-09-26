# Unused variable in function input `_update_cumulative_funding_rate()`

Severity: Informational

Type: Optimization

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-utils/src/main.sw#L1200

### Recommendation
Consider removing input `_index_asset` as an input parameter since it isn't used in the function `_update_cumulative_funding_rate()`. 

Update any functions that call the `update_cumulative_funding_rate()` function and remove `_index_asset` as an input parameter.

