# Missing access control in set_latest_answer() function

## Add access control modifier to set_latest_answer() function

Severity: High

Type: Unauthorized Access

The bug in the set_latest_answer function occurs because there is no access control enforcing that only authorized users can set the price. As a result, anyone can call this function and change the return value of the latest_answer() function.

The commented-out line of code:

https://github.com/burralabs/ruscet-contracts/blob/93c58b12d72429d1a680429bcbf72c4246a70502/contracts/pricefeed/src/main.sw#L79
```
// require(get_sender() == storage.gov.read(), Error::PricefeedForbidden);
```

is meant to prevent unauthorized users from calling the function by checking if the caller (get_sender()) matches the stored governance address storage.gov.read().

This may have been commented out for testing purposes, however, it is best to fix this issue and update any tests that may be affected by this change.

Since this line is commented out, anyone can call the function and set the price, which exposes the system to attacks like price manipulation or false data being injected.