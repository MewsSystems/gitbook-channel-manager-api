# Certification

Before moving your integration to the Production environment, Mews must verify that it meets all technical specifications. This is the purpose of the certification process. During certification, Mews will conduct a series of standard tests to ensure your integration functions correctly.

## Preparing for certification

Follow these steps to prepare for certification:

1. **Request a test environment setup**: Email partnersuccess@mews.com to request a `Client Token` and for a test property to be created in the Mews [Test environment](../guidelines/environments.md).

2. **Set up Channel Manager endpoints**: Create the following HTTPS endpoints on the Channel Manager side [Test environment](../guidelines/environments.md):
    * [Update availability](../channel-manager-operations/inventory.md#update-availability) 
    * [Update prices](../channel-manager-operations/inventory.md#update-prices)
    * [Update restrictions](../channel-manager-operations/inventory.md#update-restrictions)
    * [Confirm booking](../channel-manager-operations/reservations.md#confirm-booking) 
    * [Change notification](../channel-manager-operations/notifications.md#change-notification) \(optional\)
    
3. **Share endpoint details**: Send the endpoint details to partnersuccess@mews.com.

4. **Retrieve the Connection Token**: Use your test property credentials with the [Get properties](../mews-operations/configuration.md#get-properties) API operation to retrieve the `Connection Token` for the [Test property](../guidelines/environments.md).

5. **Fetch configuration data**: Pull property, space type mapping and rate mapping information using the [Get configuration](../mews-operations/configuration.md#get-configuration) API operation.

6. **Map rates and spaces**: Use the data from the [Get configuration](../mews-operations/configuration.md#get-configuration) response to map all rate plan and space category combinations in your channel manager system.

7. **Perform ARI and reservation tests**: Conduct all ARI (Availability, Rates & Inventory) and reservation tests, according to the scenarios outlined in [Certification tests](certification-tests.md).

8. **Complete the certification file**: Inform your Mews Technical Partner Success Manager once you’ve completed the tests. They will provide you with a certification file. Complete the file following the provided instructions, and return the completed file via email.

9. **Schedule the certification call**: After submitting the certification file, you’ll receive a link to book a 90-minute certification call with Mews. Reserve a time slot that suits you.

> **Multi-property enterprises**: Multi-property enterprises such as vacation rentals, apartments, villas, etc. are often set up in Mews as a single property with multiple individual spaces representing the individual rental properties.
> You can send these spaces to the Channel Manager as multiple connections, with one Connection Token per space (property), or as a single connection for the entire enterprise.

## Certification tests

Certification involves a 90-minute call between the Mews Marketplace team and a representative from the Channel Manager. The tests performed during this call will be divided into three main components:

* **Inventory Push Tests**: These are the primary tests performed during the call. In some cases, the call may be extended until all tests are successfully completed.
* **Reservation Tests**: Following the Inventory Push tests, Reservation Tests will be conducted via email.
* **Error Tests**: After the Reservation Tests, Error Tests will be conducted to check the system’s response to failed requests and invalid data.

For detailed descriptions of each test, see [Certification tests](certification-tests.md).

## Test evaluation

**If tests are not passed**:
If the required tests cannot be completed successfully, Mews will not allow the channel manager to advance to the Production environment. Once any issues are resolved, certification will start over.

**If certification is successful**:
If there are no critical issues discovered during certification, the channel manager will be advanced to the Production environment.
  * Mews will require unique HTTPS production endpoint URLs for the Channel Manager side.
  * Mews will issue a new `Client Token` for the channel manager to use in the Production environment.
  * Your team will need to provide documentation of the setup process for final approval.

## Re-certification

Re-certification is required in the following scenarios:

* **Incomplete pilot**: If, for any reason, it was not possible to go to pilot stage with a pilot customer, or the pilot stage could not be completed within six months, then contact Mews for re-certification.
* **Change in functionality**: If you add new features or make significant changes to an existing integration, re-certification is required to validate the updated functionality.
