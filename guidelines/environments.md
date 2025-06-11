# Environments

## Mews side environments

### Demo environment

This environment is used during development, testing and certification of client applications. Properties within the environment are based in the Netherlands and accept `EUR` currency.

* **Platform address** - `https://api.mews-demo.com`
* **Reservation push endpoint** - see [Mews: Process group](../mews-operations/reservations.md#process-group)
* **Client Token** - will be provided by Mews upon request
* **Connection Token** - will be provided by Mews upon request
* **Test property** - user credentials will be provided by Mews upon request

Test properties are configured to accept the following test credit cards:

#### Test credit cards

* Expiration date: _08/2021_ or _10/2022_
* Card holder name: any value is accepted
* CVV: any value except for ```000``` is accepted
* Card types and numbers:
  * Visa: 4111111111111111
  * MasterCard: 5555444433331111
  * Amex: 370000000000002
  * Diners: 36006666333344
  * Discover: 6445644564456445

### Production environment

* **Platform address** - `https://api.mews.com`
* **Reservation push endpoint** - see [Mews: Process group](../mews-operations/reservations.md#process-group)
* **Client Token** - will be provided to you by Mews following certification in the demo environment \(e.g. `C66EF7B239D24632943D115EDE9CB810-JJ549OU4JF94692C940F6B5A8F9453D`\)
* **Connection Token** - will be provided to you by Mews or by the property on request, or via [Mews: Get properties](../mews-operations/configuration.md#get-properties) \(e.g. `NF9R27B239D24632943D115EDE9CFH3-EA00F8FD8294692C940F6B5A8F9453D`\)

## Channel Manager side environments

Similar to the Mews side, there should be two environments with different `clientTokens`. The test Demo environment will be used by Mews to verify the connection before transitioning to the live Production environment.

## Allowlisting

Allowlisting (formerly called 'whitelisting') is a common security measure which can be applied to a system to allow only specified external systems to talk to it. This has traditionally been achieved using IP address-based firewall rules. However, this approach does not work with modern cloud based architectures, which use dynamic and shared IP addresses, proxy servers and elastic resources. For this reason, we do not support the use of IP address allowlists for our APIs and we cannot supply a list of IP addresses for our APIs.
