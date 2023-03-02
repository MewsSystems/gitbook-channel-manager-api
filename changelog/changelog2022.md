# Changelog 2022

## 29th September 2022

* Channels list removed from the documentation. Please use [Get Channels](../mews-operations/configuration.md#get-channels) to get the list of supported channels.

## 26th September 2022

* In [Process Group](../mews-operations/reservations.md#request), channel is deprecated and replaced by sources, which is a collection of new object [Source](../mews-operations/reservations.md#source) and provides additional information about reservation sources or channels.
* In [Process Group](../mews-operations/reservations.md#request), company is split into separate entities for [Company](../mews-operations/reservations.md#company) and [Travel Agency](../mews-operations/reservations.md#travel-agency). iata is removed from company (deprecated).

## 11th August 2022

* Error response codes 4 and 5 deprecated in favor of codes 9 and 10 - see [Responses](../guidelines/responses.md)
* Channel list extended. The latest Channel added has code `925`.

## 2nd August 2022

* Detail added to [Responses](../guidelines/responses.md) page to clarify error responses.

## 19th July 2022

* Channel list extended. The latest Channel added has code `923`.

## 27th June 2022

* Channel list extended. The latest Channel added has code `922`.

## 8th June 2022

* Channel list extended. The latest Channel added has code `913`.

## 30th May 2022

* Channel list extended. The latest Channel added has code `912`.
* Channel list extended. The latest Channel added has code `898`.

## 27th May 2022

* Channel list extended. The latest Channel added has code `897`.

## 13th May 2022

* Channel list extended. The latest Channel added has code `896`.

## 29th April 2022

* [Reservation](../mews-operations/reservations.md#reservation) extended in [Process Group](../mews-operations/reservations.md#process-group). The age based pricing via `guestCounts` is now supported. Both `adultCount` and `childCount` are now deprecated.

## 28th April 2022

* Extended API operation [`getConfiguration`](../mews-operations/configuration.md#get-configuration) with [age categories](../mews-operations/configuration.md#age-categories).
* [Inventory price updates](../channel-manager-operations/inventory.md#rate-price) extended. Age-based pricing via `agePrices` is now supported. 

## 21st April 2022

* Channel list extended. The latest Channel added has code `881`.

## 11th March 2022

* Channel list extended. The latest Channel added has code `877`.

## 1st March 2022

* [Customer](../mews-operations/reservations.md#customer) extended. `title` is now supported.

## 21st February 2022

* Channel list extended. The latest Channel added has code `876`.

## 17th February 2022

* Channel list extended. The latest Channel added has code `874`.

## 28th January 2022

* Channel list extended. The latest Channel added has code `873`.

| Next |
| :-- |
| [Changelog 2021](changelog2021.md) |
