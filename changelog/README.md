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

## 26th September 2022

* In [Process Group](../mews-operations/reservations.md#request), channel is deprecated and replaced by sources, which is a collection of new object [Source](../mews-operations/reservations.md#source) and provides additional information about reservation sources or channels.
* In [Process Group](../mews-operations/reservations.md#request), company is split into separate entities for [Company](../mews-operations/reservations.md#company) and [Travel Agency](../mews-operations/reservations.md#travel-agency). iata is removed from company (deprecated).

## 11th August 2022

* Error response codes 4 and 5 deprecated in favor of codes 9 and 10 - see [Responses](../guidelines/responses.md)
* [Channel](../channels/README.md) list extended. The latest Channel added has code `925`.

## 2nd August 2022

* Detail added to [Responses](../guidelines/responses.md) page to clarify error responses.

## 19th July 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `923`.

## 27th June 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `922`.

## 8th June 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `913`.

## 30th May 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `912`.
* [Channel](../channels/README.md) list extended. The latest Channel added has code `898`.

## 27th May 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `897`.

## 13th May 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `896`.

## 29th April 2022

* [Reservation](../mews-operations/reservations.md#reservation) extended in [Process Group](../mews-operations/reservations.md#process-group). The age based pricing via `guestCounts` is now supported. Both `adultCount` and `childCount` are now deprecated.

## 28th April 2022

* Extended API operation [`getConfiguration`](../mews-operations/configuration.md#get-configuration) with [age categories](../mews-operations/configuration.md#age-categories).
* [Inventory price updates](../channel-manager-operations/inventory.md#rate-price) extended. Age-based pricing via `agePrices` is now supported. 

## 21st April 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `881`.

## 11th March 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `877`.

## 1st March 2022

* [Customer](../mews-operations/reservations.md#customer) extended. `title` is now supported.

## 21st February 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `876`.

## 17th February 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `874`.

## 28th January 2022

* [Channel](../channels/README.md) list extended. The latest Channel added has code `873`.

| Changelog by year |
| :-- |
| [Changelog 2021](changelog2021.md) |
| [Changelog 2020](changelog2020.md) |
| [Changelog 2019](changelog2019.md) |
| [Changelog 2018](changelog2018.md) |
| [Changelog 2017](changelog2017.md) |
