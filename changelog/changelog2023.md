# Changelog 2023

## 30th November 2023

* New operation [Process Availability Blocks](../channel-manager-operations/availabilityBlock.md)
* [Update Availability](../channel-manager-operations/inventory.md#update-availability), [Update Restrictions](../channel-manager-operations/inventory.md#update-restrictions), [Update Prices](inventory.md#update-prices) can now be configured as synchronous as well as asynchronous by response parameter `asyncConfirmation`.
* [Process Group](../channel-manager-operations/reservations.md#request) is bi-directional.
* New operations for asynchronous confirmations
    - [Confirm availability update](../mews-operations/inventory.md#confirm-availability-update)
    - [Confirm price update](../mews-operations/inventory.md#confirm-price-update)
    - [Confirm restriction update](../mews-operations/inventory.md##confirm-reistriction-update)
    - [Confirm availability block synchronization](../mews-operations/inventory.md##confirm-availability-block-synchronization)
    - [Confirm reservations group synchronization](../mews-operations/inventory.md#confirm-group-synchronization)