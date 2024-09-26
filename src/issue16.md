# Low code coverage 

Severity: Informational

Type: Code Coverage

Not all functions and execution paths are tested in the `/tests` directory. Consider adding more tests, and basic fuzzing tests for functions that are responsible for the core business logic. 

In addition to add more tests that target different contract states, it is recommended to add minimal integration tests that chain together multiple different calls to the contract. 

It is highly recommended to add more tests for all public functions in the `vault/src/main.sw` contract, particularly the `swap()` function.