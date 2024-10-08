# Channel Manager API Operations: Availability blocks

## Process availability block

\[`async`\] This operation is used by Mews to push availability blocks to the Channel Manager. It is also used to push availability block modifications and cancellations.

### Request 

`[ChannelManagerApiAddress]/processAvailabilityBlock`

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
        "company": null,
        "notes": "This is the block for Mews conference."
    }  
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `messageId` | `string` | required | Unique identification of the message. This is used for asynchronous confirmations. |
| `responseUrl` | `string` | required \(always\) | URL which should be used for asynchronous confirmation. |
| `availabilityBlock` | [`Availability block`](#availability-block) object | required | Availablity block data. |

#### Availability block

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | optional | Unique reference code from external system for the block. Will be returned after confirmation containing this code. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the block. |
| `status` | [`Status`](#status) object | required | State of the block. |
| `name` | `string` | required | Name of the block. |
| `dates` | [`Dates`](#dates) object | required | Dates of the block. |
| `spaceCategories` | [`Space category allocation`](#space-category-allocation) collection | required | Allocated categories of the block. |
| `booker` | [`Customer`](../mews-operations/reservations.md#customer) object | required | The main booker. This does not mean that the person has arrived at the property. |
| `company` | [`Company`](../channel-manager-operations/reservations.md#company) object | optional | The company associated with the block. |
| `notes` | `string` | optional | Notes for the block. |

#### Dates

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Inclusive start date of the block, in format `yyyy-mm-dd`, e.g. `2021-12-24`; this is the first day of the block and so can be booked for stay.  |
| `end` | `string` | required | Exclusive end date of the block, in format `yyyy-mm-dd`, e.g. `2021-12-31`; this day is _not_ included in the block and so cannot be booked for stay (it is analogous to reservation departure). |
| `releasedDate` | `string` | required | End date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-24"`\). |

> **Dates**: Interval for e.g. 1 night for 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`.

#### Space category allocation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `spaceTypeCode` | `string` | required | Space type code of the reservation. |
| `allocatedSpaces` | [`Space count`](#space-count) collection | required | The number of spaces **allocated** in the time interval. |
| `occupiedSpaces` | [`Space count`](#space-count) collection | required | The number of **occupied** spaces in the time interval. |
| `rates` | [`Rates`](#rate) collection | required | Prices for the current space category. |

> **Dates**: Interval for e.g. 1 night for 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`.

### Space count

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `count` | `int` | required | Number of spaces (base of usage either occupied or allocated.)

> **Dates**: Interval for e.g. 1 night for 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`.

### Rate 
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"`\). |
| `currencyCode` | `string` | required | Three letter code of the currency. |
| `prices` | [`Price`](../channel-manager-operations/inventory.md#price) collection | required | Collection of prices for each person count, for the specified rate plan - space type - date combination. |        

> **Dates**: Interval for 1 night (e.g. is represented 2021-12-24/25 is represented as `"start": "2021-12-24", "end": "2021-12-24"`).   

#### Status

* `Create`
* `Update`
* `Cancel`

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not.

[Async availability block confirmation](../mews-operations/availabilityBlock.md#confirm-availability-block-confirmation) is expected to determine whether the update was processed afterwards or not.
