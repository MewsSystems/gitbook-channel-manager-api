# Changelog

## 15th December 2023

* Added explanations to [Mews API Operations: Process Group](../mews-operations/reservations.md#process-group) for the consequences of using negative extra product amounts (documentation only)

## 29th November 2023

* New operation [Process availability blocks](../channel-manager-operations/availabilityBlock.md)
* [Update availability](../channel-manager-operations/inventory.md#update-availability), [Update restrictions](../channel-manager-operations/inventory.md#update-restrictions), [Update prices](inventory.md#update-prices) can now be configured as synchronous as well as asynchronous by response parameter `asyncConfirmation`.
* [Process group](../channel-manager-operations/reservations.md#request) is bi-directional.
* New operations for asynchronous confirmations
    - [Confirm availability update](../mews-operations/inventory.md#confirm-availability-update)
    - [Confirm price update](../mews-operations/inventory.md#confirm-price-update)
    - [Confirm restriction update](../mews-operations/inventory.md##confirm-reistriction-update)
    - [Confirm availability block synchronization](../mews-operations/availabilityBlock.md##confirm-availability-block-synchronization)
    - [Confirm reservations group synchronization](../mews-operations/reservations.md#confirm-group-synchronization)

## 16th March 2023

* Added [Deprecations](../deprecations/README.md) page
* Removed "ACTION REQUIRED!" section from the top of the Changelog page

| Changelog by year |
| :-- |
| [Changelog 2022](changelog2022.md) |
| [Changelog 2021](changelog2021.md) |
| [Changelog 2020](changelog2020.md) |
| [Changelog 2019](changelog2019.md) |
| [Changelog 2018](changelog2018.md) |
| [Changelog 2017](changelog2017.md) |
