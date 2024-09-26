# Consider Using a More Appropriate Library than Caer for Type Generation

### Consider Using a More Appropriate Library than Caer for Type Generation

Severity: Informational
Type: Optimization / Best Practice

Issue Description:
The project is currently using the Caer library, which is primarily a Python computer vision library, for file handling and path operations related to ABI formatting. While Caer provides the needed functionality, it is somewhat out of place in a smart contract library.

Although this issue is not strictly in scope, it's worth mentioning that using a more standard library for file handling, such as Python's built-in os or pathlib, would align better with the goals of the project and improve clarity.

Recommendation:
Consider using Python’s standard libraries (e.g., os, pathlib) for file handling and path operations. This change would make the codebase more intuitive for contributors and align the implementation with best practices, ensuring the use of libraries that are appropriate for the domain of the project.