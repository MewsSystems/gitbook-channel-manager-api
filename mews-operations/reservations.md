# Mews API Operations: Reservations

## Process group

\[`async`\] Use this operation to push a group of reservations or bookings to Mews. The operation is called Process Group because it supports multiple options for processing on multiple groups of reservations. As well as creating new reservations in Mews, you can modify existing reservations and make cancellations, all using the same endpoint. Mews will process the requests and confirm back the reservations asynchronously.

> ### Rules to follow
>
> There are certain rules that need to be followed in order for Mews to process the group correctly:
>
> * It is required to send multiple related reservations in one group as part of one message.
>   * The whole group is uniquely identified by `channelManagerId` in Mews and in the channel manager extranet.
>   * Each reservation should have a unique code within the group. The same code for the reservation should be provided in any following modification message.
>   * Each reservation in the group can have different `start`, `end`, `spaceTypeCode`, `ratePlanCode`.
> * Group total cost `group.totalAmount` is the sum of all `reservation.totalAmount` objects in the group, which is the sum of all night amounts and total amounts of all `extras` of the `reservation`.
>   * If for any reason the sum of the `reservation.totalAmount` objects is different, Mews will automatically distribute the missing/additional amount to the nights, so the `group.totalAmount` is achieved.
>   * Reservation `group` amounts should be either `gross` or `net`. Either both `gross` and `net` amounts, or one of them should be sent.
> * When **modifying** some reservations from a multi-reservation group, the whole group definition with all other unchanged reservations needs to be sent, i.e. Mews doesn't process diffs.
> * When **cancelling** a reservation from a multi-reservation group, all remaining reservations need to be present in the group definition as well.
>   * There are two ways to cancel a reservation from a multi-reservation group:
>     1. If the `reservation.state` is set to [Reservation States](#reservation-states).`Cancelled`.
>     2. If the reservation is not included in the group definition message.
> * When **cancelling** a whole group, the `reservations` collection can be empty of all reservations provided as cancelled \(as per case 1 above\).

### Request 

* **Demo Environment:** `https://sandbox.pci-proxy.com/v1/push/0426f19b66715a93`
* **Production Environment:** `https://api.pci-proxy.com/v1/push/b2721c9a0351d553`

The example shows a valid group definition with two space reservations plus cancellation of a third space reservation.
The first `reservation` definition shows all details, the second `reservation` definition shows the minimal required details for creation / modification of a `reservation`.
The third `reservation` definition shows the partial cancellation - cancelling the third space reservation.

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "MyWeddingMessage789456123",
    "channelId": "EXP-123456",
    "channelManagerId": "123456",
    "availabilityBlockCode", "Wedding123",
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
        "title": "Mister",
        "nationalityCode": "US",
        "languageCode": "en-US",
        "telephone": "1-3526-88918"
    },
    "sources": [
        {
            "code": 1,
            "name": "Expedia",
            "type" : 0,
            "isPrimary" true
        },
        {
            "code": 2,
            "name": "ChoiceCRS",
            "type" : 0,
            "isPrimary" false
        },
    ],
    "company": {
        "id": "MEWS",
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
    "travelAgency": {
        "iata": "65553",
        "name": "Best Travel Agency, s.r.o",
        "contact": {
            "email": "best@company.com",
            "phone": "+420 775 775 775",
            "address": {
                "zip": "132 45",
                "addressLine1": "Some other street 123",
                "addressLine2": "Some other detail",
                "city": "Some other city",
                "country": "US"
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
            "guestCounts": [
                {
                    "code": "Adult",
                    "count": 1
                }
            ],
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
                    "title": "Misses",
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
            "guestCounts": [
                {
                    "code": "Adult",
                    "count": 2
                }
            ],
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
            "guestCounts": [
                {
                    "code": "Adult",
                    "count": 2
                }
            ],
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
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations |
| `channelId` | `string` | required | Unique identification of the booking in the channel \(i.e. OTA\). |
| `channelManagerId` | `string` | required | Unique identification of the booking in the channel manager. |
| `availabilityBlockCode` | `string` | optional | Unique identification of the availability block in the channel manager. |
| `currencyCode` | `string` | required \(exc. Cancellation\) | 3 letter code of currency of all prices within the booking. |
| `totalAmount` | [`Amount`](#amount) object | required \(exc. Cancellation\) | Total amount of the whole booking. |
| `paymentType` | `int` | required \(exc. Cancellation\) | [Payment Type](configuration.md#payment-types) code - determines whether the booking is prepaid or not. |
| `customer` | [`Customer`](#customer) object | required \(exc. Cancellation\) | Represents the main booker. Does not necessarily mean that the person arrives to the property. |
| `paymentCard` | [`Payment Card`](#payment-card) object | optional | Represents the payment card of the [`Customer`](#customer) to cover for the booking. |
| ~~`channel`~~ | ~~[`Channel`](#channel) object~~ | ~~optional~~ | ~~Represents the channel \(i.e. Travel Agency\).~~ **Deprecated!** |
| `sources` | [`Source`](#source) collection | optional | Represents the sources for the booking. |
| `company` | [`Company`](#company) object | optional | Represents the company associated with the booking. |
| `travelAgency` | [`Travel Agency`](#travel-agency) object | optional | Represents the travel agency associated with the booking. |
| `reservations` | [`Reservation`](#reservation) collection | optional | Each reservation within the booking. If the value is null or an empty collection, this implies that the whole group will be cancelled. |
| `comments` | `string` collection | optional | Represents any comments related to the booking. |

#### Customer

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `email` | `string` | _recommended_ | Email. |
| `lastName` | `string` | required | Last name. |
| `firstName` | `string` | optional | First name. |
| `title` | `string` [Title](#title) | optional | Customer title, e.g. "Mister" |
| `telephone` | `string` | optional | Telephone. |
| `loyaltyCode` | `string` | optional | Loyalty code of the customer. Processed only within main customer. |
| `nationalityCode` | `string` | optional | [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) - two letter country code or [ISO 3166-1 alpha-3 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) - three letter country code. |
| `languageCode` | `string` | optional | Language [code](https://msdn.microsoft.com/en-us/library/ee825488) of the communication language of the customer. This language will be used as the default for communication with the customer. |
| `address` | [`Address`](configuration.md#address) object | optional | Represents address. |

#### Title

* `Mister`
* `Misses`
* `Miss`

#### Company

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `id` | `string` | optional | Identifier of company. |
| `name` | `string` | optional | Name of company. |
| ~~`iata`~~ | ~~`string`~~ | ~~optional~~ | ~~IATA number of travel agency.~~ **Deprecated!** |
| `contact` | [`Contact`](#contact) object | optional | Company contact information. |

#### Travel Agency

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `iata` | `string` | optional | IATA number of travel agency. |
| `name` | `string` | optional | Name of travel agency. |
| `contact` | [`Contact`](#contact) object | optional | Travel agency contact information. |

#### Contact

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `email` | `string` | _recommended_ | Contact email address. |
| `telephone` | `string` | optional | Contaact telephone number. |
| `address` | [`Address`](configuration.md#address) object | optional | Contact address. |

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

#### Source

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required \(exc. new unknown sources\) | Code number of the source or [channel](configuration.md#channel). |
| `name` | `string` | required | Name of the source or channel. |
| `type` | `int` | required | [Source Type](#source-types) code. |
| `isPrimary` | `bool` | required | Indicates which source is the primary source for the booking. |

#### Source Types

| Code | Description |
| :-- | :-- |
| `0` | Online Travel Agency |
| `1` | Central Reservation System |
| `2` | Global Distribution System |
| `3` | Alternative Distribution System |
| `4` | Sales And Catering System |
| `5` | Property Management System |
| `6` | Tour Operator System |
| `7` | Online Booking Engine |
| `8` | Kiosk |
| `9` | Agent |

#### Reservation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required \(exc. Mews to ChM.\) | Unique code of the reservation within the whole booking. Any value but `_` accepted. No characters limit. |
| `confirmationNumber` | `string` | required \(exc. ChM to Mews.\) | Unique code of the reservation within the whole booking in Mews. No characters limit. |
| `spaceTypeCode` | `string` | required \(exc. Cancellation\) | Space type code of the reservation. |
| `ratePlanCode` | `string` | required \(exc. Cancellation\) | Rate type code of the reservation. |
| `from` | `string` | required \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | required \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-31"` for New Year's Eve\). |
| `totalAmount` | [`Amount`](#amount) object | required \(exc. Cancellation\) | Total amount of the reservation. |
| ~~`adultCount`~~ | ~~`int`~~ | ~~required \(exc. Cancellation\)~~ | ~~Number of adults in the reservation.~~ **Deprecated!** |
| ~~`childCount`~~ | ~~`int`~~ | ~~optional \(exc. Cancellation\)~~ | ~~Number of children in the reservation.~~ **Deprecated!** |
| `guestCounts` | array of [`Guest Count`](#guest-count) | required | Number of guests in the reservation for each age category. |
| `state` | `int` | optional | [Reservation State](#reservation-states) code of reservation state. _If not provided, Mews will handle the reservation as `Created` or `Modified`._ |
| `amounts` | [`Amount`](#amount) collection | required \(exc. Cancellation\) | Collection of amounts for each night of the reservation. _The count of amounts in this collection has to correspond with number of nights in the reservation._ |
| `extras` | [`Extra`](#extra) collection | optional | Collection of extra ordered products for the reservation \(e.g. Breakfast\). _Their total amount is included in the _`totalAmount`_ of the reservation._ |
| `guests` | [`Customer`](#customer) collection | optional | Collection of guests that will arrive to the property. |

> **Codes:** It is required that `code` remains the same within each booking modification message and partial modification message. If this can't be achieved because the channel doesn't provide it, simple generation of codes "01", "02", ... will suffice as long as those codes are generated in the same way for each message regarding that one booking.

#### Guest Count

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | [Age category code](#age-category-code). |
| `count` | `int` | required | Number of persons for specified age category. |

#### Age Category Code

* `Infant`
* `Child`
* `Teenager`
* `Adult`
* `Senior citizen`

#### Reservation States

| Code | Description |
| :-- | :-- |
| `1` | Created |
| `2` | Modified |
| `3` | Cancelled |

#### Extra

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of the extra product. |
| `amount` | [`Amount`](#amount) object | required | Total amount of the extra product. |
| `count` | `int` | required | Count of extra products ordered. |
| `from` | `string` | optional \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | optional \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"` for New Year's Eve\). |
| `pricing` | `int` | required | [`Extra pricing Type`](#extra-pricing-types) code of the extra product pricing. |

> ### Extra product cost
> * The total cost of the extra product should be sent in `amount`.
> * `amount` can include only `gross`, only `net`, or both `gross` and `net`.
> * `from` and `to` can define any interval within the reservation dates, but they cannot be the same date. For example, if a reservation is from 2025-10-12 to 2025-10-15, then the extra `from` and `to` cannot be specified as 2025-10-12 to 2025-10-12.

> ### Negative amounts
> * The cost for an extra product can be negative, i.e. less than zero.
> * If a reservation night price is negative, the reservation will fail.
> * If extra product costs are negative, their value will be deducted from the reservation night price.
> * As long as the resulting reservation price (after deducting the cost of the extra products) is positive, all is well.
> * If the resulting reservation price (after deducting the cost of the extra products) is negative, the reservation will still pass and be processed, however this can create complications for the property and they may have to re-price it.
> * It is recommended to send extras per room or space, because it is less likely that the resulting reservation price will fall below zero.

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

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) will determine whether the reservations or bookings were accepted for processing or not.
Confirmation will be sent asynchronously using the [Confirm booking](../channel-manager-operations/reservations.md#confirm-booking) operation.

## Confirm group confirmation

### Request

`[PlatformAddress]/api/channelManager/v1/processGroupConfirmation`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "relatedMessageId": "[Id of message which request relates to]",
    "success": true,
    "code": 123,
    "reservations": [
        {
            "code": "Ext123",
            "confirmationNumber": "Mews456"
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `relatedMessageId` | `string` | required | Id of message which requests relates to. |
| `success` | `bool` | required | Determinines the result of the operation. |
| `errors` | array of [`Error`](../guidelines/responses.md#error) | optional | In case of `"success": false`, this property holds information about the errors that occurred. |
| `code` | `string` | required | Unique reference code from external system for the group. Will be sent with every group changes. |
| `reservations` | [`Reservation Confirmation`](#reservation-confirmation) collection | optional | Confirmation details for each individual reservation in the group. |

#### Reservation Confirmation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Unique reference code for the individual reservation. Will be sent back with group changes. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the individual reservation. |


### Response

[Synchronous simple response](../guidelines/responses.md#Synchronous-simple-response) is expected.
