# Channel Manager API Operations: Inventory

## Update prices

This method is used when Mews updates prices of rate plans.
Mews always pushes both `gross` and `net` prices, the channel manager chooses which of these to use.

### Request

`[ChannelManagerApiAddress]/updatePrices`

```json
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "66511e1d-2405-4e49-914b-b0c800d802c9",
    "responseUrl": "https://api.mews-demo.com/api/channelManager/v1/processRateConfirmation",
    "ratePrices": [
        {
            "spaceTypeCode": "D1",
            "ratePlanCode": "FF",
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
            ],
            "agePrices": [
                {
                    "grossAmount": 100.0,
                    "netAmount": 100.0,
                    "currencyCode": "EUR",
                    "guestCount": 1,
                    "ageCategoryCode": "Adult"
                },
                {
                    "grossAmount": 100.0,
                    "netAmount": 100.0,
                    "currencyCode": "EUR",
                    "guestCount": 2,
                    "ageCategoryCode": "Child"
                }
            ],
            "from": "2020-02-05",
            "to": "2020-02-07"
        }
    ],
    "dependentRates": [
        {
            "rateCode": "123456",
            "baseRateCode": "789",
            "adjustments": [
                {
                    "startDate": "2020-02-05",
                    "endDate": "2020-02-07",
                    "relativeValue": 10.0,
                    "absoluteValue": 10
                }
            ]
        }
    ]
}
```

| Property | Type                                           | Contract | Description                                                                                |
| :-- |:-----------------------------------------------| :-- |:-------------------------------------------------------------------------------------------|
| `clientToken` | `string`                                       | required | Client token of the channel manager.                                                       |
| `connectionToken` | `string`                                       | required | Connection token of a property connection.                                                 |
| `messageId` | `string`                                       | required | Unique identification of the message. Used for asynchronous confirmations                  |
| `responseUrl` | `string`                                       | required \(always\) | Url which should be used for asynchronous confirmation.                                    |
| `ratePrices` | [`Rate Price`](#rate-price) collection         | required | Collection of prices for all combinations of rate plan, space type, date and person count. |
| `dependentRates` | [`Dependant Rate`](#Dependant-Rate) collection | required | Collection of dependant rates                                                              |

#### Dependant Rate

| Property         | Type                                   | Contract | Description                            |
|:-----------------|:---------------------------------------| :-- |:---------------------------------------|
| `rateCode`       | `string`                               | required | Dependant rate code.                   |
| `baseRateCode`   | `string`                               | required | Base rate code of Dependant rate code. |
| `adjustments`    | [`Adjustment`](#adjustment) collection | required | Collection of Adjustments              |

#### Adjustment

| Property         | Type       | Contract | Description                                                     |
|:-----------------|:-----------|:---------|:----------------------------------------------------------------|
| `startDate`      | `string`   | optional | Start date of the adjustment interval in `"yyyy-MM-dd"` format. |
| `endDate`        | `string`   | optional | End date of the adjustment interval in `"yyyy-MM-dd"` format.   |
| `relativeValue`  | `decimal`  | required | Relative value of the adjustment.                               |
| `absoluteValue`  | `decimal`  | required | Absolute value of the adjustment.                               |

#### Rate Price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `prices` | [`Price`](#price) collection | required | Collection of prices for each person count for the specified rate plan - space type - date combination. |
| `agePrices` | [`Age price`](#age-price) collection | required | Collection of prices for each person count and age category for the specified rate plan - space type - date combination. |

> **Dates**: `from` and `to` represent the first and last date on which the price applies. The prices are the same for each date between `from` and `to`. So `"from": "2021-12-24", "to": "2021-12-25"` means, the price applies on nights 2021-12-24/25 and 2021-12-25/26, but not anymore for night 2021-12-26/27.

#### Price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `grossAmount` | `decimal` | required | Price with taxes included. |
| `netAmount` | `decimal` | required | Price with taxes excluded. |
| `currencyCode` | `string` | required | The three-letter code of the rate price currency. |
| `guestCount` | `int` | required | The person count for the rate price. |

#### Age price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| ~~`amount`~~ | ~~`decimal`~~ | ~~required~~ | ~~The price amount.~~ **[Deprecated!](../deprecations/README.md)** |
| `grossAmount` | `decimal` | required | Price with taxes included. |
| `netAmount` | `decimal` | required | Price with taxes excluded. |
| `currencyCode` | `string` | required | The three-letter code of the rate price currency. |
| `guestCount` | `int` | required | The person count for the rate price. |
| `ageCategoryCode` | `string` | required | Mapping code of the age category. |

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not and whether will be synchronous or asynchronous.

## Update availability

This method is used when Mews updates availability of space types.

### Request

`[ChannelManagerApiAddress]/updateAvailability`

```json
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "66511e1d-2405-4e49-914b-b0c800d802c9",
    "responseUrl": "https://api.mews-demo.com/api/channelManager/v1/processAvailabilityConfirmation",
    "availabilities": [
        {
            "spaceTypeCode": "QD",
            "from": "2020-01-01",
            "to": "2020-01-31",
            "availability": 10
        },
        {
            "spaceTypeCode": "KD",
            "from": "2020-01-01",
            "to": "2020-01-30",
            "availability": 5
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations |
| `responseUrl` | `string` | required \(always\) | Url which should be used for asynchronous confirmation. |
| `availabilities` | [`Availability`](#availability) collection | required | Collection of availability of space types. |

#### Availability

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `availability` | `int` | required | The availability of the space type in the updated interval. |

> **Dates**: `from` and `to` represent the first and last date on which the availability applies. The availiablity is the same for each date between `from` and `to`. So `"from": "2021-12-24", "to": "2021-12-25"` means, the availability applies on nights 2021-12-24/25 and 2021-12-25/26, but not anymore for night 2021-12-26/27.

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not and whether will be synchronous or asynchronous.

## Update restrictions

This method is used by Mews to push restrictions updates to the channel manager.
Note there is no operation to add or delete restrictions, everything is done through Update restrictions.
Thus any existing restrictions with the same scope and time period are overridden by the updated restrictions.

> ### Restrictions
> Restrictions can be a little complicated, and for historical reasons they work differently in the API than they do in **Mews Operations**.
> For this reason, care is advised. For a full explanation of restrictions and the issues around them, as well as extended examples using the API, see [Concepts > Restrictions](../concepts/restrictions.md).
> This is recommended reading if you implement this API operation.

### Request

`[ChannelManagerApiAddress]/updateRestrictions`

```json
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "messageId": "66511e1d-2405-4e49-914b-b0c800d802c9",
    "responseUrl": "https://api.mews-demo.com/api/channelManager/v1/processRestrictionConfirmation",
    "restrictions": [
        {
            "spaceTypeCode": "QD",
            "ratePlanCode": "FF",
            "from": "2020-01-01",
            "to": "2020-01-31",
            "state": [
                1
            ],
            "minLos": 1,
            "maxLos": 7
        },
        {
            "spaceTypeCode": "KD",
            "ratePlanCode": "NR",
            "from": "2020-01-01",
            "to": "2020-01-01",
            "state": [
                2,
                8
            ]
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
| `messageId` | `string` | required | Unique identification of the message. Used for asynchronous confirmations |
| `responseUrl` | `string` | required \(always\) | Url which should be used for asynchronous confirmation. |
| `restrictions` | [`Restriction`](#restriction) collection | required | Collection of restrictions. |

#### Restriction

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Inclusive start date of the restriction period in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | Inclusive end date of the restriction period in `"yyyy-MM-dd"` format. |
| `minLos` | `int` | optional | Minimum length of stay applicable during the period. Must be at least `1` if specified. Set to `null` if no minimum applies. |
| `maxLos` | `int` | optional | Maximum length of stay applicable during the period. Must be at least equal to `minLos` if specified. Set to `null` if no maximum applies. |
| `state` | `int` collection | required | [Restriction State](#restriction-state). |

> **Dates**: `from` and `to` represent the first and last date on which the restriction applies. The restriction is the same for each date between `from` and `to`. So `"from": "2021-12-24", "to": "2021-12-25"` means the restriction applies on nights Dec 24/25 and Dec 25/26, but not any more for night Dec 26/27.

#### Restriction State

| Code | Description |
| :-- | :-- |
| `1` | Open |
| `2` | Closed (sent in combination with 6, 7 and 8) |
| `3` to `5` | _Not supported_ |
| `6` | Closed to arrival |
| `7` | Closed to departure |
| `8` | Closed to stay |

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not and whether will be synchronous or asynchronous.
