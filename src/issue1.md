# Unauthorized access in vault-pricefeed `update_price()`

Severity: High

Type: Unauthorized access

The vault-pricefeed contract allows any arbitrary caller to set the price of an asset. In the comments, it says this function will be removed in the future, however, this function allows an attacker to arbitrarily set the oracle price that the vault reads from. 

Source: https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/core/vault-pricefeed/src/main.sw#L258

[Github URL]
```rust
// this is just a helper method to update the price of an asset directly from VaultPricefeed
// this will be removed in the future when Pyth prices are supported on-chain
#[storage(read)]
fn update_price(
    asset: AssetId,
    new_price: u256
) {
    let pricefeed_addr = storage.pricefeeds.get(asset).try_read().unwrap_or(ZERO_CONTRACT);
    require(
        pricefeed_addr.non_zero(),
        Error::VaultPriceFeedInvalidPriceFeedToUpdate
    );

    let pricefeed = abi(Pricefeed, pricefeed_addr.into());
    pricefeed.set_latest_answer(new_price);
}
```

Attack scenario:

1) A malicious user calls the `update_price()` function with an `asset` and a `new_price` value. The call updates the price value in the pricefeed contract. 
2) In subsequent calls or in a multicall, the malicious user can exploit the ability to set the price in the pricefeed contract in other parts of the code base.

### Recommendation 

Add the `_only_gov()` function modifier to the `update_price()` function. Or complete the integration of this function with the Pyth pricefeed before deploying.

Additionally, consider adding a test of this function to the `/test` directory. 

