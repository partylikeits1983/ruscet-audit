# `write_buffer_amount()` function not used & duplicates the logic of the `set_buffer_amount()` function

Severity: Low 

Type: Unused function & duplicated logic

The `write_buffer_amount()` is unused and duplicates the logic of the `set_buffer_amount()` function.

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-storage/src/main.sw#L590

### Recommendation

Consider removing the `write_buffer_amount()`

