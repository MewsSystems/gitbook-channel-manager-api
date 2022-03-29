# Mews side

This section describes the Mews side of the Channel Manager API, i.e. API Operations hosted by Mews.
The Mews side receives requests from external Channel Managers, including requests to process reservations (new, updated and cancelled).

## Environments

### Test Environment

This environment is used during development, testing and certification of client applications.
Test properties within the environment are based in the Netherlands and accept `EUR` currency.

* **Platform Address** - `https://api.mews-demo.com/`
* **Reservation Push Endpoint** - unique to the environment and listed under [Process Group](reservations.md#process-group)
* **Client Token** - will be provided by Mews upon request
* **Connection Token** - will be provided by Mews upon request
* **Test Property** - user credentials will be provided by Mews upon request

The property is configured to accept the following test credit cards:

#### Test Credit Cards

* Expiration date: _08/2021_ or _10/2022_
* Card holder name: any value is accepted
* CVV: any value except for ```000``` is accepted
* Types and Numbers:
  * Visa: 4111111111111111
  * MasterCard: 5555444433331111
  * Amex: 370000000000002
  * Diners: 36006666333344
  * Discover: 6445644564456445

### Production Environment

* **Platform Address** - `https://api.mews.com`
* **Reservation Push Endpoint** - unique to the environment and listed under [Process Group](reservations.md#process-group)
* **Client Token** - will be provided to you by Mews following certification in the test environment \(e.g. `C66EF7B239D24632943D115EDE9CB810-JJ549OU4JF94692C940F6B5A8F9453D`\)
* **Connection Token** - will be provided to you by Mews or the property on request, or via [Get Properties](configuration.md#get-properties) API operation \(e.g. `NF9R27B239D24632943D115EDE9CFH3-EA00F8FD8294692C940F6B5A8F9453D`\)
