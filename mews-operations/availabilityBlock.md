# Mews API Operations: Availability blocks

## Confirm availability block confirmation

### Request

`[PlatformAddress]/api/channelManager/v1/processAvailabilityBlockConfirmation`

```json
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "relatedMessageId": "[Id of message which request relates to]",
    "success": false,
    "errors": [{
      "code": 10,
      "message": "Invalid category code",
      "categoryCode": "ABC"
   }]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `relatedMessageId` | `string` | required | Id of message which requests relates to. |
| `success` | `bool` | required | Determinines the result of the operation. |
| `errors` | array of [`Error`](../guidelines/responses.md#error) | optional | In case of `"success": false`, this property holds information about the errors that occurred. |
| `code` | `string` | required | Unique reference code from external system for the block. Will be sent with every block changes. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the block. |


### Response

[Synchronous simple response](../guidelines/responses.md#Synchronous-simple-response) is expected.

## Process availability block

[`async`] Use this operation to push availability blocks to Mews. The same operation is used for adding, updating and canceling availability blocks.

### Request

* **Test environment:** `TODO`
* **Production environment:** `TODO`

```json
{
    "clientToken": "73F621085FD945J3A3BC01CFB5F30BD7", 
    "connectionToken": "806F7EFC114743KFL84FD5450F816AB2-BB2BF4D3F4FJ81AF85J1630108B1471",
    "messageId": "13620420",    
    "availabilityBlock": {
        "code": "33",
        "confirmationNumber": "2",
        "status": "Created",
        "name": "Smith wedding",
        "dates": {
            "start": "2024-12-05",
            "end": "2024-12-11"
        },
        "releaseStrategy": {
            "type": "fixed",
            "fixedDate": "2024-12-08"    
        },
        "originalRateCode": "FF",
        "spaceCategories": [
            {
                "spaceTypeCode": "Double",
                "allocatedSpaces": [
                    {
                        "start": "2024-12-05",
                        "end": "2024-12-07",
                        "count": 2
                    },
                    {
                        "start": "2024-12-08",
                        "end": "2024-12-11",
                        "count": 5
                    }
                ],
                "rates": [
                    {
                        "start": "2024-12-05",
                        "end": "2024-12-07",
                        "prices": [
                          {
                              "grossAmount": 80.00,
                              "netAmount": 70.50,
                              "currencyCode": "EUR",
                              "guestCount": 1
                          },
                          {
                              "grossAmount": 100.00,
                              "netAmount": 90.00,
                              "currencyCode": "EUR",
                              "guestCount": 2
                          },
                          {
                              "grossAmount": 105.00,
                              "netAmount": 91.00,
                              "currencyCode": "EUR",
                              "guestCount": 3
                          }
                      ]
                    },
                    {
                        "start": "2024-12-08",
                        "end": "2024-12-11",
                        "prices": [
                            {
                              "grossAmount": 50.00,
                              "netAmount": 40.50,
                              "currencyCode": "EUR",
                              "guestCount": 1
                            },
                            {
                              "grossAmount": 60.00,
                              "netAmount": 50.00,
                              "currencyCode": "EUR",
                              "guestCount": 2
                            },
                            {
                              "grossAmount": 66.00,
                              "netAmount": 55.00,
                              "currencyCode": "EUR",
                              "guestCount": 3
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
        "paymentCard": {
            "cvv": "666",
            "expireDate": "1222",
            "holderName": "John Smith",
            "number": "4111111111111111",
            "type": 1
        },
        "company": null,
        "notes": "This is a note.",
        "pickupType": "AllInOneGroup"
    }  
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations. |
| `availabilityBlock` | [`Availability block`](#availability-block) object | required | Availablity block data. |


#### Availability block
| Property | Type | Contract| Description |
| :-- | :-- | :-- | :-- |
| `code`| `string`| required | Unique reference code from external system for the availability block. Will be returned after confirmation containing this code. |
| `confirmationNumber` | `string` | optional | Mews confirmation number for the block. If updating or deleting an existing block, this property is required. |
| `status` | [`Status`](#status) object| required | State of the availability block. |
| `name` | `string` | required | Name of the availability block. |
| `dates` | [`Dates`](#dates) object | required| Dates of the availability block. |
| `releaseStrategy` | [`Release strategy`](#release-strategy) object | required | Release strategy of the availability block. |
| `originalRateCode` | `string` | required | Original rated code of the availability block. |
| `spaceCategories` | [`Space category allocation`](#space-category-allocation) collection | required | Allocated categories of the availability block. |
| `booker` | [`Customer`](./reservations.md#customer) object | required | The main booker. This does not mean that the person has arrived at the property. |
| `paymentCard` | [`Payment Card`](./reservations.md#payment-card) object | optional | Represents the payment card of the [`Customer`](./reservations.md#customer) . |
| `company` | [`Company`](./reservations.md#company) object | optional | The company associated with the availability block. |
| `notes` | `string` | optional | Notes related to the availability block. |
| `pickupType`| [`PickupType`](#pickupType) object | required | Specifies how new reservations in block should be picked up. |

#### Dates
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `yyyy-mm-dd`, e.g. `2024-12-05`, this is the first day of the block and so can be booked for stay. |
| `end` | `string` | required | End date in format `yyyy-mm-dd`, e.g. `2024-12-08`, this day is _not_ included in the block and so cannot be booked for stay (it is analogous to reservation departure). |


#### Release strategy
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `type` | `string` [Release strategy type](#release-strategy-type)  | required | release strategy for the availability block.|
| `fixedDate` | `string` | optional | release date in format YYYY-MM-DD, for example "2024-12-08". Required if `type` is `Fixed`. |
| `offset` | `string` | optional | Offset duration from the start date before the block is released, if following the rolling release strategy. The format follows [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) for _duration_, i.e. "P[days]DT". Required if `type` is `Rolling`. |


#### Space category allocation
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Space type code of the block. |
| `allocatedSpaces` | [`Space count`](#space-count) collection | required | The number of spaces **allocated** in the time interval. |
| `rates` | [`Rates`](#rate) collection | optional | Prices for the space category. |


### Space count
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2024-12-05"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2024-12-08"`\). |
| `count` | `int` | required | Number of spaces (base of usage either occupied or allocated.) |


### Rate
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `"yyyy-MM-dd"` \(e.g. `"2024-12-05"`\). |
| `end` | `string` | required | End date \(excluded\) in format `"yyyy-MM-dd"` \(e.g., `"2024-12-31"`\). |
| `prices` | [`Price`](../channel-manager-operations/inventory.md#price) collection | required | Collection of prices for each guest count. |
| ~~`grossAmount`~~ | ~~`string`~~ | Deprecated | ~~Price with taxes included.~~ Use `prices` instead. |

#### Status

* `Created`
* `Updated`
* `Canceled`

#### PickupType

* `AllInOneGroup`
* `IndividualGroups`

#### Release strategy type

* `Fixed`
* `Rolling`

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the operation was accepted or not.

[[Async] availability block processing confirmation](#availability-block-processing-confirmation) is expected to determine whether the operation was processed afterward or not.

### Availability block processing confirmation
```json
{
    "clientToken": "73F621085FD945J3A3BC01CFB5F30BD7",
    "connectionToken": "806F7EFC114743KFL84FD5450F816AB2-BB2BF4D3F4FJ81AF85J1630108B1471",
    "messageId": "13620420",
    "code": "33",
    "confirmationNumber": "55",
    "success": "true",
    "errors": null 
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `messageId` | `string` | required | Id of message which requests relates to. |
| `code` | `string` | required | Unique reference code from external system for the block. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the block. |
| `success` | `bool` | required | Determinines the result of the operation. |
| `errors` | [`Error`](../guidelines/responses.md#error) collection | optional | In case of `"success": false`, this property holds information about the errors that occurred. |
