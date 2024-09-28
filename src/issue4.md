# Vault buy_rusd() function is not payable, yet assumes asset transfer

## Vault buy_rusd() function is not payable, yet assumes asset transfer

Severity: Medium

Type: Logic flaw

The buy_rusd() function is not payable, yet the function assumes the user will transfer an asset when calling the function. In the tests, a separate transaction transfers the asset to the contract, and then the buy_rusd() function is called. However, if the contract is used in this way in a production environment, there is a high risk of front running, which could result in a loss of user funds.

Non payable buy_rusd() function:

https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault/src/main.sw#L178

In the tests, there is first a transaction that transfers assets to the contract, and then the buy_rusd() function is called:

https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/test/core/vault/buyRUSD.test.ts#L153

Attack scenario:

Alice transfers an asset to the contract using a simple transfer, since the buy_rusd() function is not payable.
In a separate transaction, Alice calls buy_rusd() with the assetId of the token she sent to the contract in the first transaction.
Bob, who is monitoring the network, sees that Alice sent an asset to the contract with the intention of calling the buy_rusd() function. If Bob’s transaction succeeds, the _transfer_in() function will update Bob’s balance, not Alice’s.
_transfer_in() function: https://github.com/burralabs/ruscet-contracts/blob/36668ffd579d0f666dcd8ab2530cc096d3bbb2f8/contracts/core/vault/src/internals.sw#L136-#L149

### Suggestion
The buy_rusd() function should be made payable and assets should be transferred to the contract in the function call using the add_transfer method: https://docs.fuel.network/docs/fuels-ts/contracts/transferring-assets/

Front running discussion on the fuel forum discussing this exact pattern: https://forum.fuel.network/t/front-running-within-the-fuelvm/6633