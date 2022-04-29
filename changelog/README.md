# ACTION REQUIRED!

We have changed the URLs for our API. The old URLs with `mews.li` are no longer operational, instead you should be using:
* `api.mews.com`
 
Affected endpoints: 
* [Get Properties](../mews-operations/configuration.md#get-properties)
* [Get Configuration](../mews-operations/configuration.md#get-configuration)
* [Get Channels](../mews-operations/configuration.md#get-channels)
* [Set Inventory](../mews-operations/inventory.md#set-inventory)
* [Request ARI Update](../mews-operations/inventory.md#request-ari-update)

For more details, see [Environments](../mews-operations/README.md#environments).

# Changelog

## 29 April 2022 12:00 UTC

* [Reservation](../mews-operations/reservations.md#reservation) extended in [Process Group](../mews-operations/reservations.md#process-group). The age based pricing via `guestCounts` is now supported. Both `adultCount` and `childCount` are now deprecated.

## 28 April 2022 12:00 UTC

* Extended API operation [`getConfiguration`](../mews-operations/configuration.md#get-configuration) with [age categories](../mews-operations/configuration.md#age-categories).
* [Inventory price updates](../channel-manager-operations/inventory.md#rate-price) extended. Age-based pricing via `agePrices` is now supported. 

## 21 April 2022 12:00 UTC

* [Channel](../channels/README.md) list extended. The latest Channel added has code `881`.

## 11 March 2022 12:00 UTC

* [Channel](../channels/README.md) list extended. The latest Channel added has code `877`.

## 1 March 2022 12:00 UTC

* [Customer](../mews-operations/reservations.md#customer) extended. `title` is now supported.

## 21 February 2022 12:00 UTC

* [Channel](../channels/README.md) list extended. The latest Channel added has code `876`.

## 17 February 2022 12:00 UTC

* [Channel](../channels/README.md) list extended. The latest Channel added has code `874`.

## 28 January 2022 12:00 UTC

* [Channel](../channels/README.md) list extended. The latest Channel added has code `873`.

| Changelog by year |
| :-- |
| [Changelog 2021](changelog2021.md) |
| [Changelog 2020](changelog2020.md) |
| [Changelog 2019](changelog2019.md) |
| [Changelog 2018](changelog2018.md) |
| [Changelog 2017](changelog2017.md) |
