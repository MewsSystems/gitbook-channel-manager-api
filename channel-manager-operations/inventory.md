# Channel Manager API Operations: Inventory

## Update prices

This method is used when Mews updates prices of rate plans.
Mews always pushes both `gross` and `net` prices, the channel manager chooses which of these to use.

### Request

`[ChannelManagerApiAddress]/updatePrices`

```javascript
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
                    "currencyCode": "USD",
                    "guestCount": 1,
                    "ageCategoryCode": "Adult"
                },
                {
                    "grossAmount": 100.0,
                    "netAmount": 100.0,
                    "currencyCode": "USD",
                    "guestCount": 2,
                    "ageCategoryCode": "Child"
                }
            ],
            "from": "2020-02-05",
            "to": "2020-02-07"
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
| `ratePrices` | [`Rate Price`](#rate-price) collection | required | Collection of prices for all combinations of rate plan, space type, date and person count. |

#### Rate Price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `prices` | [`Price`](#price) collection | required | Collection of prices for each person count for the specified rate plan - space type - date combination. |
| `agePrices` | [`Age price`](#age-price) collection | required | Collection of prices for each person count and age category for the specified rate plan - space type - date combination. |

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

```javascript
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

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not and whether will be synchronous or asynchronous.

## Update restrictions

This method is used when Mews updates restrictions.

### Request

`[ChannelManagerApiAddress]/updateRestrictions`

```javascript
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
| `from` | `string` | required | Start date of the restriction period in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(inclusive\) of the restriction period in `"yyyy-MM-dd"` format. |
| `minLos` | `int` | optional | Minimum Length-Of-Stay applicable during the period. Must be at least `1` if specified. Set to `null` if no minimum applies.
| `maxLos` | `int` | optional | Maximum Length-Of-Stay applicable during the period. Must be at least equal to `minLos` if specified. Set to `null` if no maximum applies.
| `state` | `int` collection | required | [Restriction State](#restriction-state). |

#### Restriction State

| Code | Description |
| :-- | :-- |
| `1` | Open |
| `2` | Closed (used in conjunction with 6, 7 and 8) |
| `3` | ~~Open to arrival~~ _Not supported._ |
| `4` | ~~Open to departure~~ _Not supported._ |
| `5` | ~~Open to stay~~ _Not supported._ |
| `6` | Closed to arrival |
| `7` | Closed to departure |
| `8` | Closed to stay |

> Note: State `2` is always sent in combination with states `6`, `7` and/or `8`.

### Restriction Examples

#### No Restriction (Open)

When all restrictions are removed, state `1` is sent. New restrictions always override old restrictions. State `1` is _not_ sent to remove old restrictions, if they were modified.
 
```javascript
{
    "ratePlanCode": "NR",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-24",
    "to": "2020-10-31"
}
```

#### Closed to Stay

State `2` is sent in combination with state `8`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        8
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Stay with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2020-10-01",
    "to": "2020-10-14"
}
```

#### Closed to Arrival

State `2` is sent in combination with state `6`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "STA",
    "state": [
        2,
        6
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Arrival with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to arrival.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2019-10-01",
    "to": "2019-10-04"
}
```

#### Closed to Departure

State `2` is sent in combination with state `7`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Departure with minLos and maxLos 

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to departure.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2019-10-01",
    "to": "2019-10-04"
}
```

#### Closed to Stay and Closed to Arrival 

Both states should be applied.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8,
        6
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Stay and Closed to Departure

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8,
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Stay with minLos/maxLos and Closed to Arrival

This should be applied as no arrivals are possible and reservation minLos is 3 nights and maxLos is 7 nights.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        6
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Stay and Closed to Arrival with minLos/maxLos

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Departure and Closed to Arrival with minLos/maxLos

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        7
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### Closed to Departure and Closed to Stay with minLos/maxLos

This combination should be treated as Closed to departure. Reservations can be made if the length-of-stay requirements are met.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        7
    ],
    "minLos": 2,
    "maxLos": 6,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

#### No minLos nor maxLos

When `minLos` or `maxLos` is not specified, `null` value is sent.

```javascript        
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        8
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Response

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not and whether will be synchronous or asynchronous.
