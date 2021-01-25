# Changelog

## 25th January 2021 12:00 UTC

* Added more restriction [use cases] (https://mews-systems.gitbook.io/channel-manager-api/channel-manager-api#restriction-examples).

## 8th December 2020 12:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `845`.

## 27th November 2020 12:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `844`.

## 10th November 2020 12:00 UTC

* [Extra pricing type](mews-api.md#extra-pricing-types): pricing type `per person per night` added.

## 20th October 2020 12:00 UTC

* [Error codes](general-remarks.md#error) list extended. New error codes are from 9 to 14.
* [Customer](mews-api.md#customer) extended with loyalty code.

## 20th August 2020 12:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `843`.

## 1st July 2020 12:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `834`.

## 27th May 2020 12:00 UTC

* [Get Configuration](mews-api.md#get-configuration) extended with pricing mode, which shows whether a property is built in net or gross pricing environment.
* [Extra](mews-api.md#extra) extended in [Process Group](mews-api#process-group). Consumption date can be posted for each extra sent in a reservation. 

## 28th April 2020 12:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `833`.

## 26th March 2020 12:00 UTC

* [Amount](mews-api.md#amount) extended in [Process Group](mews-api.md#process-group) and [Price](channel-manager-api.md#price) extended in [Update Prices](channel-manager-api.md#update-prices). Net pricing is now supported for inventory upload and reservation delivery.

## 19th March 2019 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `819`.

## 15th January 2019 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `740`. Some channels were merged, because they were duplicate. All merged channels are ~~crossed~~ and the proper code is mentioned.

## 28th November 2018 22:00 UTC

* [Restriction](channel-manager-api.md#restriction) list supports now ClosedTo* restrictions.

## 26th September 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `739`.

## 8th August 2018 22:00 UTC

* New endpoint [Get Channels](mews-api.md#get-channels) added to Mews API side to obtain all [Channel](channels.md#channels)s.

## 11th July 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `567`. Some channels were merged, because they were duplicate. All merged channels are ~~crossed~~ and the proper code is mentioned.

## 18th Jun 2018 15:00 UTC

* Mimimal required TLS version updated to `TLS 1.2`.

## 31th May 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `510`.

## 9th May 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `502`.

## 18th April 2018 21:00 UTC

* [Change Notification](channel-manager-api.md#change-notification) extened with a `type` property of [`Change Notification Type`](channel-manager-api.md#change-notification-type) type, that determines a type of change that happened in Mews.

## 28th March 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `500`.

## 8th March 2018 22:00 UTC

* [Process Group](mews-api.md#process-group) API call: following changes are done in a way that the previous fields still exist in both API and are still supported and the new fields are added next to it and used the same way as their previous versions:

* `cvv` field added to [`Payment card`](mews-api.md#payment-card) object.
* ~~code~~ was removed and replaced with `type` in [`Payment card`](mews-api.md#payment-card) object.

**The support is temporarily and will end approximately by end of April 2018. Make sure you migrate to use the new fields, as the previous fields will be removed.**

## 1th March 2018 22:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `500`.

## 7th February 2018 23:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `443`.

## 18th January 2018 00:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `441`.

## 10th January 2018 22:00 UTC

* [Get Configuration](mews-api.md#get-configuration) API call: `applicability` field added to [`Cancellation Policy`](mews-api.md#cancellation-policy) object.
Following changes are done in a way that the previous fields still exist in both API and are still supported and the new fields are added next to it and used the same way as their previous versions:

* ~~accessToken~~ renamed to `clientToken` in all API calls in **both** API sides.
* ~~connectionCode~~ renamed to `connectionToken`in all API calls in **both** API sides.
* [Get Properties](mews-api.md#get-properties) API call
  * ~~code~~ renamed to `token` in [`Connection Info`](mews-api.md#connection-info) object.
* [Process Group](mews-api.md#process-group) API call
  * ~~distributor~~ renamed to `channel` in [`Reservation`](mews-api.md#reservation) object.

**The support is temporarily and will end approximately by end of March 2018. Make sure you migrate to use the new fields, as the previous fields will be removed.**

## 13th December 2017 00:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `425`.

## 30th November 2017 00:00 UTC

* [Channel](channels.md#channels) list extended. The latest Channel added has code `423`.

## 15th October 2017 22:00 UTC

* Initial version released.
