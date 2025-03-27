# Changelog

## 26th March 2025

* Improvements to [Your integration journey](../your-journey/README.md) and [Certification](../your-journey/certification.md) pages. Documentation-only, no changes to API.

## 17th March 2025

* [Mews: Process group](../mews-operations/reservations.md#process-group):
  * Extended request object with new `booker` property.

## 12th March 2024

* [Mews: Process availability block](../mews-operations/availabilityBlock.md#process-availability-block):
  * Extended request object with `paymentCard` property.
  * Extended [Rate](../mews-operations/availabilityBlock.md#rate) object with `prices` property to specify occupancy-based prices.
  * **Deprecated** `grossAmount` property in the [Rate](../mews-operations/availabilityBlock.md#rate) object. Use `prices` instead.
  * **Breaking!** Endpoint URL changed to be sent via PCI proxy. **You must update the URL used for this operation**.

## 13th December 2024

* Refresh of entire site. Documentation-only, no changes to API.

## 7th November 2024

* [Mews: Process group](../mews-operations/reservations.md#process-group) – added recommended limit of 100 reservations per group. Documentation-only.

## 4th November 2024

* Deprecated `loyaltyCode` in [Mews: Process group](../mews-operations/reservations.md#process-group). This is replaced by `loyaltyInfo`.

## 11th October 2024

* [Mews: Process availability block](../mews-operations/availabilityBlock.md#process-availability-block) – Added `Rolling` to release strategy type; added `offset` property to release strategy object, and updated contract and description for `fixedDate` property (NOT a breaking change)
* [CHM: Process availability block](../channel-manager-operations/availabilityBlock.md) – Added `releasedStrategy` property to availability block object and deprecated `releasedDate` property – see [Deprecations](../deprecations/README.md).

## 10th October 2024

* [Availability block](../channel-manager-operations/availabilityBlock.md#availability-block) extended with `pickupType` field.
* [Availability block](../mews-operations/availabilityBlock.md#availability-block) extended with `pickupType` field.

## 8th October 2024

* New operation [Mews: Process availability block](../mews-operations/availabilityBlock.md#process-availability-block).

## 4th October 2024

* [CHM: Update prices](/channel-manager-operations/inventory.md) extended with `dependent rates`.

## 2nd Oct 2024

* Updated [Usage guidelines](../guidelines/README.md#channels) to advise how to add new sales channels or sources through the API. Documentation-only.

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

* Added loyalty info to Request in [CHM: Process Availability block](../channel-manager-operations/availabilityBlock.md#availability-block).
  
## 26th March 2024

* Minor changes to highlight the need for re-certification when replacing deprecated features, see [Deprecations](../deprecations/README.md) (documentation only)

## 9th January 2024

* Added [Age Category Code](../mews-operations/reservations.md#age-category-code) in [Mews: Process Group](../mews-operations/reservations.md#process-group) (documentation only).
* Added explanations to [Mews: Process Group](../mews-operations/reservations.md#process-group) for the consequences of using negative extra product amounts (documentation only).

| Changelog by year |
| :-- |
| [Changelog 2023](changelog2023.md) |
| [Changelog 2022](changelog2022.md) |
| [Changelog 2021](changelog2021.md) |
| [Changelog 2020](changelog2020.md) |
| [Changelog 2019](changelog2019.md) |
| [Changelog 2018](changelog2018.md) |
| [Changelog 2017](changelog2017.md) |
