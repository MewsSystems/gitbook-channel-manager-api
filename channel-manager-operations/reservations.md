# Channel Manager API Operations: Reservations

## Process group

\[`async`\] This operation allows the Mews to push a group of reservations (bookings) to channel manager.
This option allows creations, modifications, and partial or complete cancellations. Confirmation is done asynchronous.

### Request 

`[ChannelManagerApiAddress]/processGroup`


```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "66511e1d-2405-4e49-914b-b0c800d802c9",
    "responseUrl": "https://api.mews-demo.com/api/channelManager/v1/processGroupConfirmation",
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
| `clientToken` | `string` | required \(always\) | Client token of the channel manager. |
| `connectionToken` | `string` | required \(always\) | Token of a concrete connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations |
| `responseUrl` | `string` | required \(always\) | Url which should be used for asynchronous confirmation. |
| `channelId` | `string` | optional | Unique identification of the booking in the channel \(i.e. OTA\). Sent always once provided by channel manager. |
| `channelManagerId` | `string` | optional | Unique identification of the booking in the channel manager. Sent always once provided by channel manager. |
| `availabilityBlockCode` | `string` | optional | Unique identification of the availability block in the channel manager. |
| `availabilityBlockConfirmationNumber` | `string` | optional | Unique identification of the availability block in the Mews. |
| `currencyCode` | `string` | required \(exc. Cancellation\) | 3 letter code of currency of all prices within the booking. |
| `totalAmount` | [`Amount`](../mews-operations/reservations.md#amount) object | required \(exc. Cancellation\) | Total amount of the whole booking. |
| `paymentType` | `int` | required \(exc. Cancellation\) | [Payment Type](configuration.md#payment-types) code - determines whether the booking is prepaid or not. |
| `customer` | [`Customer`](../mews-operations/reservations.md#customer) object | required \(exc. Cancellation\) | Represents the main booker. Does not necessarily mean that the person arrives to the property. |
| `sources` | [`Source`](../mews-operations/reservations.md#source) collection | optional | Represents the sources for the booking. |
| `company` | [`Company`](../mews-operations/reservations.md#company) object | optional | Represents the company associated with the booking. |
| `travelAgency` | [`Travel Agency`](../mews-operations/reservations.md#travel-agency) object | optional | Represents the travel agency associated with the booking. |
| `reservations` | [`Reservation`](../mews-operations/reservations.md#reservation) collection | optional | Each reservation within the booking. If the value is null or an empty collection, this implies that the whole group will be cancelled. |
| `comments` | `string` collection | optional | Represents any comments related to the booking. |

## Confirm booking

This method is used when Mews confirms a booking sent via [Process group](../mews-operations/reservations.md#process-group).
It is used to send confirmation of success as well as confirmation of failure.

### Request

`[ChannelManagerApiAddress]/confirmGroup`

#### Confirmation of success:

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "relatedMessageId": "MyWeddingMessage789456123",
    "channelManagerId": "123456",
    "reservations": [
        {
            "code": "01",
            "confirmationNumber": "PMS-001"
        }
    ]
}
```

#### Confirmation of failure:

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "relatedMessageId": "MyWeddingMessage789456123",
    "channelManagerId": "123456",
    "error": {
        "code": 4,
        "message": "There is no space type with mapping code 'ST'."
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
| `relatedMessageId` | `string` | required | Id of message which requests relates to. |
| `channelManagerId` | `string` | required | Channel Manager booking reference. |
| `error` | [`Error`](../guidelines/responses.md#error) | optional | In case of processing failure, this provides a description of the error. |
| `reservations` | [`Reservation Confirmation`](#reservation-confirmation) collection | optional | Confirmation details for each individual reservation in the group. |

#### Reservation Confirmation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Unique reference code for the individual reservation. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the individual reservation. |

### Response

[Simple response](../guidelines/responses.md#simple-response) is expected to determine whether the update was accepted or not.
