# Changelog

## 4th October 2024

* Added [Mews API Operations: Process availability block](../mews-operations/availabilityBlockProcessor.md).


## 2nd Oct 2024

* Updated [Guidelines](../guidelines/README.md#channels) to advise how to add new sales channels or sources through the API. Documentation-only.

## 11th September 2024

* [Restrictions](../concepts/restrictions.md) explanations re-written and moved to new section [Concepts](../concepts/README.md).

## 25th July 2024

* Moved integration process documentation into new section [Your integration journey](../your-journey/README.md). Documentation-only.
* Added [Re-certification](../your-journey/certification.md#re-certification) to the [Certification](../your-journey/certification.md) page. Documentation-only.

## 23rd July 2024

* Re-worded [error code](../guidelines/responses.md#error-codes) descriptions for improved clarity, and updated [Certification Error Tests](../your-journey/certification-tests.md#error-tests). Documentation-only.

## 24th June 2024

* [Availability block](../channel-manager-operations/availabilityBlock.md#availability-block) extended with `notes` field.
* [Loyalty info](../mews-operations/reservations.md#loyalty-info) extended with `tierCode` field.
* [Reservation](../mews-operations/reservations.md#reservation) extended with `timeState` field and [`paymentCardData`](../mews-operations/reservations.md#payment-card-data) object. Note these properties are only used by [CHM: Process group](../channel-manager-operations/reservations.md#process-group), _not_ by [Mews: Process group](../mews-operations/reservations.md#process-group).
* Correction: Minor corrections to examples in [CHM: Process group](../channel-manager-operations/reservations.md#process-group) and [Mews: Process group](../mews-operations/reservations.md#process-group).
* Correction: `paymentType` removed from Request in [CHM: Process group](../channel-manager-operations/reservations.md#process-group). This is only used on the Mews side.

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
