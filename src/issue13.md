# Uninitialized on deploy

Severity: Low

Type: Contract Initialization

All `initialize()` functions present in the codebase allow any caller to initialize the contract.

### Recommendation:

To prevent unwanted initialization, or the possibility of the non-initialization of a contract, consider setting the address of the initializor in configurables.