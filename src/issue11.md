# Compiler warnings

Severity: Low

Type: Compiler Warnings

There are a significant number of compiler warnings when compiling the codebase. Most of the compiler warnings are as a result of performing a write after an external call, however, all of the external calls are part of the smart contract architecture. Although it is a valid warning, this specific compiler warning is present on many other Sway smart contract projects.

### Recommendation:

Consider fixing all comipler warnings not associated with the write after external call warning.