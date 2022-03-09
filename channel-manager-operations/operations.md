# API Operations (Channel Manager)

## Update Prices

\[`sync`\] This method is used when Mews updates prices of rate plans.
Mews always pushes both `gross` and `net` prices, the channel manager chooses which of these to use.

### Request

`[ChannelManagerApiAddress]/updatePrices`

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
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
| `ratePrices` | [`Rate Price`](#rate-price) collection | required | Collection of prices for all combinations of rate plan, space type, date and person count. |

#### Rate Price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `prices` | [`Price`](channel-manager-api.md#price) collection | required | Collection of prices for each person count for the specified rate plan - space type - date combination. |

#### Price

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| ~~`amount`~~ | ~~`decimal`~~ | ~~required~~ | ~~The price amount.~~ Deprecated.|
| `grossAmount` | `decimal` | required | Price with taxes included. |
| `netAmount` | `decimal` | required | Price with taxes excluded. |
| `currencyCode` | `string` | required | The three-letter code of the rate price currency. |
| `guestCount` | `int` | required | The person count for the rate price. |

### Response

[Simple response](../general/README.md#simple-response) is expected to determine whether the update was accepted or not.

## Update Availability

\[`sync`\] This method is used when Mews updates availability of space types.

### Request

`[ChannelManagerApiAddress]/updateAvailability`

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
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
| `availabilities` | [`Availability`](#availability) collection | required | Collection of availability of space types. |

#### Availability

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `availability` | `int` | required | The availability of the space type in the updated interval. |

### Response

[Simple response](../general/README.md#simple-response) is expected to determine whether the update was accepted or not.

## Update Restrictions

\[`sync`\] This method is used when Mews updates restrictions.

### Request

`[ChannelManagerApiAddress]/updateRestrictions`

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
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
                2
            ]
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
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

#### Open (No Restrictions)

When all restrictions are removed, state `1` is sent and Length-Of-Stay properties are set to `null`.
New restrictions always override old restrictions. State `1` is _not_ sent to remove old restrictions, if they were modified.
 
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

#### Open with LOS Restrictions

State `1` is sent but with specified minLos and/or maxLos. If `minLos` or `maxLos` is not met, then the restriction should be treated as Closed to stay.

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

#### Closed to Arrival with LOS Restrictions

State `2` is sent in combination with state `6`, with specified minLos and/or maxLos.
If `minLos` or `maxLos` is not met, then the restriction should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        2,
        6
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

#### Closed to Departure with LOS Restrictions 

State `2` is sent in combination with state `7`, with specified minLos and/or maxLos.
If `minLos` or `maxLos` is not met, then the restriction should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        2,
        7
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2019-10-01",
    "to": "2019-10-04"
}
```

#### Closed to Stay and Closed to Arrival 

Both restriction states are in effect, which is equivalent to Closed to stay.

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

#### Closed to Stay with LOS restrictions

Regardless of LOS restrictions, this is equivalent to Closed to stay.

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
#### Closed to Arrival and Closed to Departure

Both restriction states are in effect, so no reservations can start or end during this period.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        6
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Response

[Simple response](../general/README.md#simple-response) is expected to determine whether the update was accepted or not.

## Confirm Booking

\[`sync`\] This method is used when Mews confirms a booking sent via [Process Group](../mews-operations/operations.md#process-group).
It is used to send confirmation of success as well as confirmation of failure.

### Request

`[ChannelManagerApiAddress]/confirmGroup`

#### Confirmation of success:

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
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
| `channelManagerId` | `string` | required | Channel Manager booking reference. |
| `error` | [`Error`](../general/README.md#error) | optional | In case of processing failure, this provides a description of the error. |
| `reservations` | [`Reservation Confirmation`](#reservation-confirmation) collection | optional | Confirmation details for each individual reservation in the group. |

#### Reservation Confirmation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Unique reference code for the individual reservation. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the individual reservation. |

### Response

[Simple response](../general/README.md#simple-response) is expected to determine whether the update was accepted or not.

## Change Notification

\[`sync`\]
This operation is used by Mews to notify the channel manager when there is a change in the connection configuration.
It is optional for the channel manager to implement this endpoint, it is used in case of a fully-automated connection.

The following changes trigger notification:

* Connection is disabled
* Connection is enabled
* Connection is deleted
* A new connection is created
* A new space is added to a mapped space category
* A space category is unmapped or unsynchronized
* A new rate is mapped
* A rate is unmapped or unsyncronized
* A new rate-space category combination is created

### Request

`[ChannelManagerApiAddress]/changeNotificaton`

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "type": 0
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
| `type` | `int` | required | [`Change Notification Type`](#change-notification-type). |

#### Change Notification Type

| Code | Description |
| :-- | :-- |
| `0` | Created |
| `1` | Updated |
| `2` | Activated |
| `3` | Deactivated (in Mews, the whole connection can be disabled) |
| `4` | Deleted |

### Response

[Simple response](../general/README.md#simple-response) is expected to determine whether the update was accepted or not.
