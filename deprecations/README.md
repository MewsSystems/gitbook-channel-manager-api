# Deprecations

Deprecations are features of the API which you are discouraged from using, even though they may still be supported for a period of time for the sake of backwards compatibility.
Such features are normally deprecated because they are superseded by a better alternative. They can include object properties, entire objects or entire API operations.
The list of deprecations is as follows. Individual items are also highlighted in the [Changelog](../changelog/README.md) when they occur.
Historic deprecations that have already been discontinued may not be listed. For more information, see our [Deprecations Policy](https://mews-systems.gitbook.io/open-api/staying-up-to-date/deprecations-policy).

> **Important:** We strongly advise you to review this list and if you are using any of the deprecated items in your integration, to update your implementation accordingly. If the feature change requires re-certification, please [contact Mews](mailto://partnersuccess@mews.com) before making the change.

The table columns have the following meanings:

* __Feature__ - What entity or feature is being deprecated
* __Comments__ - Additional information, such as the reason for the deprecation and what the replacement is
* __Deprecated__ - The date at which the deprecation notice was given (see the [Changelog](../changelog/README.md))
* __Discontinued__ \- The date at which it is planned to discontinue support completely; a value of '-' indicates no date has been set

## Deprecated operations

| Feature | Comments | Deprecated | Discontinued |
| :-- | :-- | :-- | :-- |
| - | - | - | - |

## Deprecated properties

| Feature | Comments | Deprecated | Discontinued |
| :-- | :-- | :-- | :-- |
| `grossAmount` in [Rate](../mews-operations/availabilityBlock.md#rate) object in [Mews: Process availability block](../mews-operations/availabilityBlock.md#process-availability-block) | Replaced by `prices` collection of [`Price`](../channel-manager-operations/inventory.md#price) | 6 March 2025 | - |
| `loyaltyCode` in [Customer](../mews-operations/reservations.md#customer) object in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `loyaltyInfo` | 4 Nov 2024 | - |
| `releasedDate` in [Dates](../channel-manager-operations/availabilityBlock.md#dates) object in [CHM: Availability block](../channel-manager-operations/availabilityBlock.md#availability-block) | Replaced by `fixedDate` in [`Release strategy`](../mews-operations/availabilityBlock.md#release-strategy) of  [`Availability block`](../channel-manager-operations/availabilityBlock.md#availability-block) | 11 Oct 2024 | - |
| `error` in [Synchronous error response](../guidelines/responses.md#synchronous-error-response) | Replaced by `errors` (plural and an `array`) | 15 May 2024 | - |
| `paymentType` in [`RatePlan`](../mews-operations/configuration.md#rate-plan) returned by [Mews: Get configuration](../mews-operations/configuration.md#get-configuration) | May yield incorrect value. More info below (\*1). | 16 May 2024 | - |
| `channel` in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `sources` | 26 Sep 2022 | - |
| `iata` in [Company](../mews-operations/reservations.md#company) in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `iata` in the [Travel Agency](../mews-operations/reservations.md#travel-agency) object | 26 Sep 2022 | - |
| `adultCount` and `childCount` in [`Reservation`](../mews-operations/reservations.md#reservation) object in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `guestCounts`; **Requires re-certification** | 29 Apr 2022 | - |
| `code` in [`Payment card`](../mews-operations/reservations.md#payment-card) object in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `type` | 08 Mar 2018 | End Apr 2018 |
| `accessToken` in all API calls on both Mews side and CHM side | Replaced by `clientToken` | 10 Jan 2018 | End Mar 2018 |
| `connectionCode` in all API calls on both Mews side and CHM side | Replaced by `connectionToken` | 10 Jan 2018 | End Mar 2018 |
| `code` in [`Connection Info`](../mews-operations/configuration.md#connection-info) object in [Mews: Get properties](../mews-operations/configuration.md#get-properties) | Replaced by `token` | 10 Jan 2018 | End Mar 2018 |
| `distributor` in [`Reservation`](../mews-operations/reservations.md#reservation) object in [Mews: Process group](../mews-operations/reservations.md#process-group) | Replaced by `channel` | 10 Jan 2018 | End Mar 2018 |

> **(\*1) paymentType**: We are introducing enhancements that elevate settlement rules to the enterprise level, allowing them to be assigned either to a rate group or directly to a rate. Due to the complex nature of settlement rules, which can vary within a rate group, `paymentType` is now obsolete as the value may vary depending on reservation dates, which can't be reflected in this field.

## Deprecated functionality

| Feature | Comments | Deprecated | Discontinued |
| :-- | :-- | :-- | :-- |
| [Error response codes](../guidelines/responses.md#error-codes) 4 and 5 | Use codes 9 and 10 instead | 11 Aug 2022 | - |

