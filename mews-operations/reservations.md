# Mews API Operations: Reservations

## Process Group

\[`async`\] This operation allows the channel manager to push a reservation group \(i.e. _booking_\) to Mews. This option allows creations, modifications, and partial or complete cancellations. Mews will process and confirm the booking asynchronously.

### Request 

* **Demo Environment:** `https://sandbox.pci-proxy.com/v1/push/0426f19b66715a93`
* **Production Environment:** `https://api.pci-proxy.com/v1/push/b2721c9a0351d553`

The example shows a valid group definition with 2 space reservations + cancellation of the 3rd space reservation. First `reservation` definition shows all details, second `reservation` definition shows the minimal required details for creation / modification of a `reservation`. The 3rd `reservation` definition shows the partial cancellation - cancelling the 3rd space reservation.

> __There are certain rules that need to be followed in order for Mews to process the group correctly:__
>
> * It is required to send multiple related reservations in one group as part of one message.
>   * The whole group is uniquely identified by `channelManagerId` in Mews and in the channel manager extranet.
>   * Each reservation should have a unique code within the group. The same code for the reservation should be provided in any following modification message.
>   * Each reservation in the group can have different `start`, `end`, `spaceTypeCode`, `ratePlanCode`.
> * Group total cost `group.totalAmount` is the sum of all `reservation.totalAmount` objects in the group, which is the sum of all night amounts and total amounts of all `extras` of the `reservation`.
>   * If for any reason the sum of the `reservation.totalAmount` objects is different, Mews will automatically distribute the missing/additional amount to the nights, so the `group.totalAmount` is achieved.
>   * Reservation `group` amounts should be either `gross` or `net`. Either both `gross` and `net` amounts, or one of them should be sent.
> * When **modifying** some reservations from a multi-reservation group, the whole group definition with all other unchanged reservations needs to be sent \(i.e. Mews doesn't process diffs\).
> * When **cancelling** a reservation from a multi-reservation group, all remaining reservations need to be present in the group definition as well.
>   * There are 2 ways to cancel a reservation from a multi-reservation group.
>     * A. If the `reservation.state` is set to [Reservation States](#reservation-states).`Cancelled`.
>     * B. If the reservation is not included in the group definiton message.
> * When **cancelling** a whole group, the `reservations` collection can be empty of all reservations provided as cancelled \(as per case A above\).

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "channelId": "EXP-123456",
    "channelManagerId": "123456",
    "comments": [
        "Approximate arrival: 16:30.",
        "Guest request a room with ocean view."
    ],
    "currencyCode": "EUR",
    "customer": {
        "address": {
            "addressLine1": "Some street 123",
            "addressLine2": "Some other detail",
            "city": "Some city",
            "country": "US",
            "latitude": 30,
            "longitude": 20,
            "region": "Some region",
            "zip": "123 45"
        },
        "email": "john@smith.com",
        "firstName": "John",
        "lastName": "Smith",
        "nationalityCode": "US",
        "languageCode": "en-US",
        "telephone": "1-3526-88918"
    },
    "channel": {
        "code": 1,
        "name": "Expedia"
    },
    "company": {
        "id": "MEWS",
        "iata": "65893",
        "name": "Mews Systems, s.r.o.",
        "contact": {
            "email": "mews@mews.li",
            "phone": "+420 775 684 983",
            "address": {
                "zip": "110 00",
                "city": "Prague",
                "country": "Czech Republic",
                "addressLine1": "586 Ulice Test",
                "addressLine2": "Patro 2"
            }
        }
    },
    "paymentCard": {
        "cvv": "666",
        "expireDate": "1222",
        "holderName": "John Smith",
        "number": "4111111111111111",
        "type": 1
    },
    "paymentType": 1,
    "reservations": [
        {
            "adultCount": 1,
            "childCount": 0,
            "code": "01",
            "extras": [
                {
                    "code": "1",
                    "count": 1,                    
                    "amount": {
                        "net": 16.2,
                        "gross": 20
                    },
                    "from": "2021-05-06",
                    "to": "2021-05-07",
                    "pricing": 3
                }
            ],
            "from": "2020-05-05",
            "guests": [
                {
                    "address": {
                        "addressLine1": "Some other street 123",
                        "addressLine2": "Some other detail",
                        "city": "Some other city",
                        "country": "US",
                        "latitude": 30,
                        "longitude": 20,
                        "region": "Some other region",
                        "zip": "123 45"
                    },
                    "email": "jane@smith.com",
                    "firstName": "Jane",
                    "lastName": "Smith",
                    "nationalityCode": "US",
                    "languageCode": "en-US",
                    "telephone": "1-369-81891",
                    "loyaltyCode": "PG60972345"
                }
            ],
            "amounts": [
                {
                    "net": 81,
                    "gross": 100
                },
                {
                    "net": 97.2,
                    "gross": 120
                },
            ],
            "ratePlanCode": "FF",
            "spaceTypeCode": "SGL",
            "state": 1,
            "to": "2020-05-07",
            "totalAmount": {
                "gross": 260,
                "net": 208
            }
        },
        {
            "adultCount": 2,
            "code": "02",
            "from": "2020-05-06",
            "amounts": [
                {
                    "net": 81,
                    "gross": 100
                },
                {
                    "net": 97.2,
                    "gross": 120
                },
                {
                    "net": 97.2,
                    "gross": 120
                }
            ],
            "ratePlanCode": "NR",
            "spaceTypeCode": "DBL",
            "state": 2,
            "to": "2020-05-09",
            "totalAmount": {
                "net": 275.4,
                "gross": 340
            }
        },
        {
            "adultCount": 2,
            "code": "03",
            "from": "2020-05-06",
            "ratePlanCode": "FF",
            "spaceTypeCode": "DBL",
            "state": 3,
            "to": "2020-05-09"
        }
    ],
    "totalAmount": {
        "net": 486,
        "gross": 600
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required \(always\) | Client token of the channel manager. |
| `connectionToken` | `string` | required \(always\) | Token of a concrete connection. |
| `channelId` | `string` | required \(always\) | Unique identification of the booking in the channel \(i.e. OTA\). |
| `channelManagerId` | `string` | required \(always\) | Unique identification of the booking in the channel manager. |
| `currencyCode` | `string` | required \(exc. Cancellation\) | 3 letter code of currency of all prices within the booking. |
| `totalAmount` | [`Amount`](#amount) object | required \(exc. Cancellation\) | Total amount of the whole booking. |
| `paymentType` | `int` | required \(exc. Cancellation\) | [Payment Type](#payment-types) code - determines whether the booking is prepaid or not. |
| `customer` | [`Customer`](#customer) object | required \(exc. Cancellation\) | Represents the main booker. Does not necessarily mean that the person arrives to the property. |
| `paymentCard` | [`Payment Card`](#payment-card) object | optional | Represents the payment card of the [`Customer`](#customer) to cover for the booking. |
| `channel` | [`Channel`](#channel) object | optional | Represents the channel \(i.e. Travel Agency\). |
| `reservations` | [`Reservation`](#reservation) collection | optional | Each reservation within the booking. _Empty \(_`null`_ or _`[]`_\) means whole group will be cancelled._ |
| `comments` | `string` collection | optional | Represents any comments related to the booking. |

#### Customer

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `email` | `string` | _recommended_ | Email. |
| `lastName` | `string` | required | Last name. |
| `firstName` | `string` | optional | First name. |
| `telephone` | `string` | optional | Telephone. |
| `loyaltyCode` | `string` | optional | Loyalty code of the customer. |
| `nationalityCode` | `string` | optional | [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) - two letter country code or [ISO 3166-1 alpha-3 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) - three letter country code. |
| `languageCode` | `string` | optional | Language [code](https://msdn.microsoft.com/en-us/library/ee825488) of the communication language of the customer. This language will be used as the default for communication with the customer. |
| `address` | [`Address`](#address) object | optional | Represents address. |

#### Company

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `id` | `string` | optional | Identifier of company. |
| `iata` | `string` | optional | IATA number of travel agency. |
| `name` | `string` | optional | Name of company or travel agency. |
| `contact` | [`Contact`](#contact) object | optional | Company or travel agency contact information. |

#### Contact

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `email` | `string` | _recommended_ | Contact email address. |
| `telephone` | `string` | optional | Contaact telephone number. |
| `address` | [`Address`](#address) object | optional | Contact address. |

#### Payment card

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `type` | `int` | required | [Payment Card Type](#payment-card-types) code. |
| `number` | `string` | required | Payment card number. _Only numbers allowed._ |
| `expireDate` | `string` | required | Expiration date of card in `"MMyy"` format \(e.g `"0222"` for February 2022 expiration\). |
| `cvv` | `string` | optional | Card CVV code. The value cannot be ```000```.|
| `holderName` | `string` | optional | Card holder name. |

#### Payment Card Types

| Code | Description |
| :-- | :-- |
| `1` | Visa |
| `2` | Master Card |
| `3` | American Express |
| `4` | Bank Card |
| `5` | Carte Bleu |
| `6` | Carte Blanche |
| `7` | Diners Club |
| `8` | Discover Card |
| `9` | Eurocard |
| `10` | Japanese Credit Bureau |
| `11` | Universal Air Travel |
| `12` | China Unionpay |
| `13` | Maestro |

#### Channel

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required | [Channel](#channels) code. |
| `name` | `string` | required | Name of the Channel. |

#### Reservation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required \(always\) | Unique code of the reservation within the whole booking. Any value but `_` accepted. No characters limit. |
| `spaceTypeCode` | `string` | required \(exc. Cancellation\) | Space type code of the reservation. |
| `ratePlanCode` | `string` | required \(exc. Cancellation\) | Rate type code of the reservation. |
| `from` | `string` | required \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | required \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-31"` for New Year's Eve\). |
| `totalAmount` | [`Amount`](#amount) object | required \(exc. Cancellation\) | Total amount of the reservation. |
| `adultCount` | `int` | required \(exc. Cancellation\) | Number of adults in the reservation. |
| `childCount` | `int` | optional \(exc. Cancellation\) | Number of children in the reservation. |
| `state` | `int` | optional | [Reservation State](#reservation-states) code of reservation state. _If not provided, Mews will handle the reservation as `Created` or `Modified`._ |
| `amounts` | [`Amount`](#amount) collection | required \(exc. Cancellation\) | Collection of amounts for each night of the reservation. _The count of amounts in this collection has to correspond with number of nights in the reservation._ |
| `extras` | [`Extra`](#extra) collection | optional | Collection of extra ordered products for the reservation \(e.g. Breakfast\). _Their total amount is included in the _`totalAmount`_ of the reservation._ |
| `guests` | [`Customer`](#customer) collection | optional | Collection of guests that will arrive to the property. |

_¹ It is required that the code remains the same within each booking modification message and partial modification message. If it can't be achieved because Channel doesn't provide it, simple generation of "01", "02", ... codes will suffice as long as those codes are generated in same way for each message regarding that one booking._

#### Reservation States

| Code | Description |
| :-- | :-- |
| `1` | Created |
| `2` | Modified |
| `3` | Cancelled |

#### Extra

* Total cost of the extra product should be sent in `net` or `gross` amounts.
* Either both `gross` and `net` amounts, or one of them should be sent.
* `from` and `to` dates cannot be the same, but it can be any other interval within the reservation dates. (e.g., reservation is from 2025-10-12 to 2025-10-15, extra `from` and `to` cannot be 2025-10-12 to 2025-10-12).

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of the extra product. |
| `amount` | [`Amount`](#amount) object | required | Total amount of the extra product. |
| `count` | `int` | required | Count of extra products ordered. |
| `from` | `string` | optional \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | optional \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"` for New Year's Eve\). |
| `pricing` | `int` | required | [`Extra pricing Type`](#extra-pricing-types) code of the extra product pricing. |


#### Amount

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `net` | `decimal` | optional | Price with taxes excluded. |
| `gross` | `decimal` | optional | Price with taxes included. |

#### Extra Pricing Types

| Code | Description |
| :-- | :-- |
| `1` | Once per reservation. |
| `2` | Per person - for each guest of reservation. |
| `3` | Per night - for each night of reservation. |
| `4` | Per person per night - for each guest for each night of reservaion.|

### Response

[Simple response](../general/README.md#simple-response) will determine whether the booking was accepted for processing or not.
