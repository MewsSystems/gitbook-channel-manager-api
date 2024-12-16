# Certification

Once you have finished development against the API, Mews needs to confirm that the integration has been developed according to specification before it can be moved to the Production environment. This is the purpose of the certification process. During the process, a series of standard tests will be performed to check that everything is in order.

## Preparing for certification

Follow these steps to prepare for certification:

1. Email partnersuccess@mews.com to request a `Client Token` and for a test property to be created in the Mews [Test environment](../guidelines/environments.md).
2. Create the following HTTPS endpoint URLs on the Channel Manager side [Test environment](../guidelines/environments.md):
    * [Update availability](../channel-manager-operations/inventory.md#update-availability) 
    * [Update prices](../channel-manager-operations/inventory.md#update-prices)
    * [Update restrictions](../channel-manager-operations/inventory.md#update-restrictions)
    * [Confirm booking](../channel-manager-operations/reservations.md#confirm-booking) 
    * [Change notification](../channel-manager-operations/notifications.md#change-notification) \(optional\)
3. Email the endpoint details to partnersuccess@mews.com.
4. Use your test property username and [Get properties](../mews-operations/configuration.md#get-properties) API operation to fetch the `connectionToken` for the [Test property](../guidelines/environments.md).
5. Pull property information, space type mapping and rate mapping information using the [Get configuration](../mews-operations/configuration.md#get-configuration) API operation.
6. Map all the rate plan and space category combinations in the user interface of the channel manager system using the data received from the [Get configuration](../mews-operations/configuration.md#get-configuration) request.

> **Multi-property enterprises**: Multi-property enterprises such as vacation rentals, apartments, villas, etc. are often set up in Mews as spaces belonging to a single Mews property, i.e. the 'property' is the Mews enterprise customer and the 'spaces' are the individual rental properties.
> These spaces (i.e. properties) can be sent to the channel manager as multiple connections, with one `Connection Token` per space (i.e. property), or as a single connection for the entire enterprise.

## Certification tests

These tests will be conducted during a 90-minute call between the Mews Marketplace team and a representative of the channel manager.  

* See [Certification tests](certification-tests.md)

### Evaluation

If the required tests cannot be completed successfully, Mews will not allow the channel manager to advance to the Production environment. Once any issues are resolved, certification will start over.

If there are no critical issues discovered during certification, the channel manager will be advanced to the Production environment.
  * Mews will require unique HTTPS production endpoint URLs for the Channel Manager side
  * Mews will issue a new `Client Token` for the channel manager to use in the live environment
  * The channel manager will issue documentation of their set-up process for approval

## Re-certification

There are two scenarios in which re-certification is required:

1. The pilot stage has not been completed in a reasonable time (six months).
2. The functionality supported by the integration has changed.

### Incomplete pilot

If, for any reason, it was not possible to go to pilot stage with a pilot customer, or the pilot stage could not be completed, then contact Mews for re-certification.

### Change in functionality

If you add new functionality to an existing integration, again we will need to re-certify the integration.
