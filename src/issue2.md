# Dead code in vault-pricefeed contract 

## Dead code in vault-pricefeed contract

Severity: Low 

Type: Dead Code

The _get_amm_price function always returns zero leading to dead code in parts of the vault-pricefeed contract. As a result, any conditional checks that rely on the AMM price being greater than zero never execute, effectively making the AMM price logic redundant.

Dead code:

https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault-pricefeed/src/main.sw#L298-#L306

https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault-pricefeed/src/main.sw#L407-#L428

get_amm_price() function leading to dead code: https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault-pricefeed/src/main.sw#L532-#L534