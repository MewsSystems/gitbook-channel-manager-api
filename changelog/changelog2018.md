# Changelog 2018

## 28th November 2018

* [Restriction](../channel-manager-operations/operations.md#restriction) list supports now ClosedTo* restrictions.

## 26th September 2018

* Channel list extended. The latest Channel added has code `739`.

## 8th August 2018

* New endpoint [Get Channels](../mews-operations/configuration.md#get-channels) added to Mews API side to obtain all Channels.

## 11th July 2018

* Channel list extended. The latest Channel added has code `567`. Some channels were merged, because they were duplicate. All merged channels are ~~crossed~~ and the proper code is mentioned.

## 18th June 2018

* Mimimal required TLS version updated to `TLS 1.2`.

## 31st May 2018

* Channel list extended. The latest Channel added has code `510`.

## 9th May 2018

* Channel list extended. The latest Channel added has code `502`.

## 18th April 2018

* [Change Notification](../channel-manager-operations/operations.md#change-notification) extened with a `type` property of [`Change Notification Type`](../channel-manager-operations/operations.md#change-notification-type) type, that determines a type of change that happened in Mews.

## 28th March 2018

* Channel list extended. The latest Channel added has code `500`.

## 8th March 2018

* [Process Group](../mews-operations/reservations.md#process-group) API call: following changes are done in a way that the previous fields still exist in both API and are still supported and the new fields are added next to it and used the same way as their previous versions:

* `cvv` field added to [`Payment card`](../mews-operations/reservations.md#payment-card) object.
* ~~code~~ was removed and replaced with `type` in [`Payment card`](../mews-operations/reservations.md#payment-card) object.

**The support is temporarily and will end approximately by end of April 2018. Make sure you migrate to use the new fields, as the previous fields will be removed.**

## 1st March 2018

* Channel list extended. The latest Channel added has code `500`.

## 7th February 2018

* Channel list extended. The latest Channel added has code `443`.

## 18th January 2018

* Channel list extended. The latest Channel added has code `441`.

## 10th January 2018

* [Get Configuration](../mews-operations/configuration.md#get-configuration) API call: `applicability` field added to [`Cancellation Policy`](../mews-operations/configuration.md#cancellation-policy) object.
Following changes are done in a way that the previous fields still exist in both API and are still supported and the new fields are added next to it and used the same way as their previous versions:

* ~~accessToken~~ renamed to `clientToken` in all API calls in **both** API sides.
* ~~connectionCode~~ renamed to `connectionToken`in all API calls in **both** API sides.
* [Get Properties](../mews-operations/configuration.md#get-properties) API call
  * ~~code~~ renamed to `token` in [`Connection Info`](../mews-operations/configuration.md#connection-info) object.
* [Process Group](../mews-operations/reservations.md#process-group) API call
  * ~~distributor~~ renamed to `channel` in [`Reservation`](../mews-operations/reservations.md#reservation) object.

**The support is temporarily and will end approximately by end of March 2018. Make sure you migrate to use the new fields, as the previous fields will be removed.**

| Next |
| :-- |
| [Changelog 2017](changelog2017.md) |
