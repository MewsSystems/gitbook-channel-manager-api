# Changelog

## 21th June 2024

* [Availability block](/channel-manager-operations/availabilityBlock.md#availability-block) extended with `notes` field.
* Customer [Loyalty info](/mews-operations/reservations.md#loyalty-info) extended with `tierCode` field.
* [CHM: Process group](/channel-manager-operations/reservations.md#process-group) operation was extended with [`timeState`](/mews-operations/reservations.md#reservation-time-states) field and [`paymentCardData`](/mews-operations/reservations.md#payment-card-data) object in [`Reservation`](/mews-operations/reservations.md#reservation) object.

## 16th May 2024

* Clarified [Synchronous error response](../guidelines/responses.md#synchronous-error-response)
* Deprecated `error` (singular) in [Synchronous error response](../guidelines/responses.md#synchronous-error-response), use `errors` (plural, array) instead. See [Deprecations](../deprecations/README.md)
* Deprecated `paymentType` in [`RatePlan`](../mews-operations/configuration.md#rate-plan), see [Deprecations](../deprecations/README.md).

## 19th April 2024

* Clarified dates in ARI pushes, Availability blocks and Reservation Push and delivery.

## 11th April 2024

* Added loyalty info to Request in [Channel manager API Operations: Process Availability block](../channel-manager-operations/availabilityBlock.md#availability-block).
  
## 26th March 2024

* Minor changes to highlight the need for re-certification when replacing deprecated features, see [Deprecations](../deprecations/README.md) (documentation only)

## 9th January 2024

* Added [Age Category Code](../mews-operations/reservations.md#age-category-code) in [Mews API Operations: Process Group](../mews-operations/reservations.md#process-group) (documentation only).
* Added explanations to [Mews API Operations: Process Group](../mews-operations/reservations.md#process-group) for the consequences of using negative extra product amounts (documentation only).

| Changelog by year |
| :-- |
| [Changelog 2023](changelog2023.md) |
| [Changelog 2022](changelog2022.md) |
| [Changelog 2021](changelog2021.md) |
| [Changelog 2020](changelog2020.md) |
| [Changelog 2019](changelog2019.md) |
| [Changelog 2018](changelog2018.md) |
| [Changelog 2017](changelog2017.md) |
