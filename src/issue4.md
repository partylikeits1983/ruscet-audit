# Vault buy_rusd() function is not payable, yet assumes asset transfer

## Vault buy_rusd() function is not payable, yet assumes asset transfer

Severity: Medium

Type: Logic flaw

The buy_rusd() function is not payable, yet the function assumes the user will transfer an asset when calling the function. In the tests, a separate transaction transfers the asset to the contract, and then the buy_rusd() function is called. However, if the contract is used in this way in a production environment, there is a high risk of front running, which could result in a loss of user funds.

Non payable buy_rusd() function: