# Channel Manager API Operations: Availability blocks

## Process availability block

\[`async`\] This method is used when Mews push availability blocks.
It is used to send availability blocks changes such as create, modification, cancellation.

### Request 

`[ChannelManagerApiAddress]/processAvailabilityBlock`

The example shows a valid availability block definition.

```javascript
{
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "66511e1d-2405-4e49-914b-b0c800d802c9",
    "responseUrl": "https://api.mews-demo.com/api/channelManager/v1/processAvailabilityBlockConfirmation",
    "availabilityBlock": {
        "code": null,
        "confirmationNumber": "2",
        "status": "Create",
        "name": "SampleBlock",
        "dates": {
            "start": "2023-12-05",
            "end": "2023-12-11",
            "releasedDate": "2023-12-04"
        },
        "spaceCategories": [
            {
                "start": "2023-12-05",
                "end": "2023-12-11",
                "spaceTypeCode": "D",
                "allocatedSpaces": [
                    {
                        "start": "2023-12-05",
                        "end": "2023-12-07",
                        "count": 2
                    },
                    {
                        "start": "2023-12-08",
                        "end": "2023-12-11",
                        "count": 5
                    }
                ],
                "occupiedSpaces": [
                    {
                        "start": "2023-12-05",
                        "end": "2023-12-07",
                        "count": 1
                    },
                    {
                        "start": "2023-12-08",
                        "end": "2023-12-12",
                        "count": 0
                    }
                ],
                "rates": [
                    {
                        "start": "2023-12-05",
                        "end": "2023-12-12",
                        "currencyCode": "EUR",
                        "prices": [
                            {
                                "grossAmount": 100.00,
                                "netAmount": 93.46,
                                "currencyCode": "EUR",
                                "guestCount": 1
                            },
                            {
                                "grossAmount": 100.00,
                                "netAmount": 93.46,
                                "currencyCode": "EUR",
                                "guestCount": 2
                            },
                            {
                                "grossAmount": 100.00,
                                "netAmount": 93.46,
                                "currencyCode": "EUR",
                                "guestCount": 3
                            },
                            {
                                "grossAmount": 100.00,
                                "netAmount": 93.46,
                                "currencyCode": "EUR",
                                "guestCount": 4
                            }
                        ]
                    }
                ]
            }
        ],
        "booker": {
            "email": "angelina.jolie@10001-438-sample-portfolio-hotel-10001.com",
            "firstName": "Angelina",
            "lastName": "Jolie",
            "title": null,
            "telephone": null,
            "nationalityCode": null,
            "languageCode": null,
            "loyaltyCode": null,
            "address": null
        },
        "company": null
    }  
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations |
| `responseUrl` | `string` | required \(always\) | Url which should be used for asynchronous confirmation. |
| `availabilityBlock` | [`Availability block`](#availability-block) object | required | Availablity block data |

#### Availability block

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | optional | Unique reference code from external system for the block. Will be returned after confirmation containing this code. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the block. |
| `status` | [`Status`](#status) object | required | Represents state of block. |
| `name` | `string` | required | Name of block. |
| `dates` | [`Dates`](#dates) object | required | Represents dates of block. |
| `spaceCategories` | [`Space category allocation`](#space-category-allocation) collection | required | Represents allocated categories of block. |
| `booker` | [`Customer`](../mews-operations/reservations.md#customer) object | required | Represents the main booker. Does not mean that the person arrives to the property. |
| `company` | [`Company`](../channel-manager-operations/reservations.md#company) object | optional | Represents the company associated with the block. |

#### Dates

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\).  |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). This date can't be booked for stay anymore. This date is like a reservation departure. |
| `releasedDate` | `string` | required | End date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-24"`\). |

> **Dates**: Interval for 1 night (e.g. is represented 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`).

#### Space category allocation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `spaceTypeCode` | `string` | required | Space type code of the reservation. |
| `allocatedSpaces` | [`Space count`](#space-count) collection | required | Represents number of spaces **allocated** in time. |
| `occupiedSpaces` | [`Space count`](#space-count) collection | required | Represents number of **occupied** spaces in time. |
| `rates` | [`Rates`](#rate) collection | required | Represents prices for current space category. |

> **Dates**: Interval for 1 night (e.g. is represented 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`).

### Space count

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `count` | `int` | required | Number of spaces (base of usage either occupied or allocated.)

> **Dates**: Interval for 1 night (e.g. is represented 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`).

### Rate 
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `currencyCode` | `string` | required | 3 letter code of currency. |
| `prices` | [`Price`](../channel-manager-operations/inventory.md#price) collection | required | Collection of prices for each person count for the specified rate plan - space type - date combination. |        

> **Dates**: Interval for 1 night (e.g. is represented 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`).   

#### Status

* `Create`
* `Update`
* `Cancel`

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not.

[Async availability block confirmation](../mews-operations/availabilityBlock.md#confirm-availability-block-confirmation) is expected to determine whether the update was processed afterwards or not.