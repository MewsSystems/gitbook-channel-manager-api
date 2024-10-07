# Channel Manager API Operations: Process availability blocks

## Process availability block

[`async`] Use this operation to push availability blocks to Mews. The operation is used for add/update/cancel availability block.

### Request

[PlatformAddress]/api/channelManager/v1/processAvailabilityBlock

```json
{
    "clientToken": "[Channel manager client token]", 
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "[Id of message which request relates to]",    
    "availabilityBlock": {
        "code": "33",
        "confirmationNumber": "2",
        "status": "Created",
        "name": "SampleBlock",
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
                "spaceTypeCode": "D",
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
                        "grossAmount": 100.00
                    },
                    {
                        "start": "2024-12-08",
                        "end": "2024-12-11",
                        "grossAmount": 50.00
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
        "notes": "This is a note."
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
| `confirmationNumber` | `string` | required(exc. create) | Mews confirmation number for the block. |
| `status` | [`Status`](#status) object| required | State of the availability block. |
| `name` | `string` | required | Name of the availability block. |
| `dates` | [`Dates`](#dates) object | required| Dates of the availability block. |
| `releaseStrategy` | [`Release strategy`](#release-strategy) object | required | Release strategy of the availability block. |
| `originalRateCode` | `string` | required | Original rated code of the availability block. |
| `spaceCategories` | [`Space category allocation`](#space-category-allocation) collection | required | Allocated categories of the availability block. |
| `booker` | [`Customer`](./reservations.md#customer) object | required | The main booker. This does not mean that the person has arrived at the property. |
| `company` | [`Company`](./reservations.md#company) object | optional | The company associated with the availability block. |
| `notes` | `string` | optional | It's a notes. |


#### Dates
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `start` | `string` | required | Start date in format `yyyy-mm-dd`, e.g. `2024-12-05`, this is the first day of the block and so can be booked for stay. |
| `end` | `string` | required | End date in format `yyyy-mm-dd`, e.g. `2024-12-08`, this day is _not_ included in the block and so cannot be booked for stay (it is analogous to reservation departure). |


#### Release strategy
| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `type` | `string` | [`Release strategy type`](#release-strategy-type) required | release strategy for the availability block. |
| `fixedDate` | `string` | required | release date in format `"yyyy-MM-dd"` \(e.g., `"2024-12-08"`\). |


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
| `grossAmount` | `string` | required | Price with taxes included. | 


#### Status

* `Created`
* `Updated`
* `Canceled`

#### Release strategy type

* `Fixed`

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the operation was accepted or not.

[[Async] availability block processing confirmation](#availability-block-processing-confirmation) is expected to determine whether the operation was processed afterward or not.

### Availability block processing confirmation
```json
{
 "clientToken": "[Mews Client token]",
 "connectionToken": "[Token of a concrete connection]",
 "messageId": "23423",
 "code": "code",
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

