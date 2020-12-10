# Channel Manager API

This is the API to be implemented on channel manager side.

* [Mews Inventory Update Modes](channel-manager-api.md#mews-inventory-update-modes)
  * [Full Inventory Update Mode](channel-manager-api.md#full-inventory-update-mode)
  * [Delta Inventory Update Mode](channel-manager-api.md#delta-inventory-update-mode)
* [Expected Operations](channel-manager-api.md#expected-operations)
  * [Update Prices](channel-manager-api.md#update-prices) \(required\)
  * [Update Availability](channel-manager-api.md#update-availability) \(required\)
  * [Update Restrictions](channel-manager-api.md#update-restrictions) \(required\)
  * [Confirm Booking](channel-manager-api.md#confirm-booking) \(required\)
  * [Change Notification](channel-manager-api.md#change-notification) \(optional\)

The channel manager side accepts Inventory updates \(i.e. prices / availability / restrictions\) and booking confirmations. Similarly to Mews side, there should be 2 environments with different `clientToken`s from each other. The test \(or development\) environment will be used to verify the implemented connection by Mews before connecting the live environment.

## Mews Inventory Update Modes

Mews sends Inventory in 2 modes, both modes use the same API messages.

### Full Inventory Update Mode

It is possible to request the Inventory to be updated for some period via API call - [Request ARI Update](mews-api.md#request-ari-update). Or it is possible that property employee uses this mode to push latest data manually. Data sent in this mode is always data for **all** connected rate plans and space types combinations.

### Delta Inventory Update Mode

Mews automatically sends changes in Inventory \(once connection is set up\). Data sent in this mode is just the changed data from the last update. This is a completely automated process and there is no way to trigger just the delta update to be sent. Delta inventory updates are sent repeatedly until they are successfully accepted by channel manager.

## Expected Operations

### Update Prices

\[`sync`\] This method is used when Mews updates prices of rate plans.

#### Request `[ChannelManagerApiAddress]/updatePrices`

Mews always pushes both `gross` and `net` prices. Correct value needs to be picked up by the channel manager. 

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
                    "amount": 100.00,
                    "grossAmount": 100.00,
                    "netAmount": 93.46,
                    "currencyCode": "EUR",
                    "guestCount": 1
                },
                {
                    "amount": 100.00,
                    "grossAmount": 100.00,
                    "netAmount": 93.46,
                    "currencyCode": "EUR",
                    "guestCount": 2
                },
                {
                    "amount": 100.00,
                    "grossAmount": 100.00,
                    "netAmount": 93.46,
                    "currencyCode": "EUR",
                    "guestCount": 3
                },
                {
                    "amount": 100.00,
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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `ratePrices` | [`Rate Price`](channel-manager-api.md#rate-price) collection | required | Collection of prices for all rate plan - space type - date - person count combinations. |

##### Rate Price

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `prices` | [`Price`](channel-manager-api.md#price) collection | required | Collection of prices for each person count for certain rate plan - space type - date combination. |

##### Price

| Property | Type |  | Description |
| --- | --- | --- | --- |
| ~~`amount`~~ | ~~`decimal`~~ | ~~required~~ | ~~The price amount.~~ Deprecated.|
| `grossAmount` | `decimal` | required | Price with taxes included. |
| `netAmount` | `decimal` | required | Price with taxes excluded. |
| `currencyCode` | `string` | required | The 3 letter code of the rate price currency. |
| `guestCount` | `int` | required | The person count for the rate price. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Update Availability

\[`sync`\] This method is used when Mews updates availability of space types.

#### Request `[ChannelManagerApiAddress]/updateAvailability`

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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `availabilities` | [`Availability`](channel-manager-api.md#availability) collection | required | Collection of availability of space types. |

##### Availability

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `availability` | `int` | required | The availability of the space type in the updated interval. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Update Restrictions

\[`sync`\] This method is used when Mews updates restrictions.

#### Request `[ChannelManagerApiAddress]/updateRestrictions`

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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `restrictions` | [`Restriction`](channel-manager-api.md#restriction) collection | required | Collection of restrictions. |

##### Restriction

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `minLos` | `int` | optional | Minimal LOS during the interval. _Has to be at least 1._ |
| `maxLos` | `int` | optional | Maximal LOS during the interval. _Has to be at least _`minLos`_._ |
| `state` | `int` collection | required | [Restriction State](channel-manager-api.md#restriction-state) code. |

##### Restriction State

| Code | Description |
| --- | --- |
| `1` | Open |
| `2` | Closed |
| `3` | ~~Open to arrival~~ _Not supported._ |
| `4` | ~~Open to departure~~ _Not supported._ |
| `5` | ~~Open to stay~~ _Not supported._ |
| `6` | Closed to arrival |
| `7` | Closed to departure |
| `8` | Closed to stay |

#### Restriction Examples

##### No Restriction (Open)

When all restrictions are removed, state `1` is sent. New restrictions always override old restrictions. State `1` is not sent to remove old restrictions, if they were modified.
 
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

##### Closed Restrictions

State `2` is always sent in combination with state `6`, `7`, or `8`, or all together.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8,
        6,
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-24",
    "to": "2020-10-31"
}
```
##### Closed to Stay

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

##### Closed to Stay with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as closed to stay.

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

##### Closed to Arrival

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

##### Closed to Arrival with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as closed to arrival.

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

##### Closed to Departure

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

##### Closed to Departure with minLos and maxLos 

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as closed to departure.

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

##### Closed to Stay and Closed to Arrival 

Both states should be applied by a partner.

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

##### Closed to Stay and Closed to Departure

This is not an industry standard and should be disregarded by a partner.

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

##### Closed to Stay with MLos and CLosed to Arrival

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
##### Closed to Stay and Closed to Arrival with Mlos

This is not an industry standard and should be disregarded by a partner.

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
##### Closed to Departure and Closed to Arrival with Mlos

This is not an industry standard and should be disregarded by a partner.

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

##### No minLos or maxLos

When `minLos` is not specified, `null` value is sent.

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

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Confirm Booking

\[`sync`\] This method is used when Mews confirms a booking sent via [Process Group](mews-api.md#process-group). It is used for both successful confirmation or notification that processing failed.

#### Request `[ChannelManagerApiAddress]/confirmGroup`

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

or

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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `channelManagerId` | `string` | required | Booking code. |
| `error` | [`Error`](https://github.com/mews-systems/channel-manager-api/tree/da74c52f29ad04bf712acb397e311d8e5e8ba90b/general-api.md#error) | optional | If booking processing failed, this holds the explanation. |
| `reservations` | [`Reservation Confirmation`](channel-manager-api.md#reservation-confirmation) collection | optional | Confirmation of each reservation. |

##### Reservation Confirmation

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Unique code of the reservation. |
| `confirmationNumber` | `string` | required | Mews confirmation number of the reservation. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Change Notification

\[`sync`\] This method is used when Mews informs channel manager that there was change in Mews that affects the connection configuration. This API call is optional to implement, it is used in case of fully-automated connection.

The following changes trigger notification:

* A new space was added to a mapped space category.
* A space category was unmapped or unsynchronized.
* A new rate was mapped.
* A rate was unmapped or unsyncronized.
* A new rate-space category combination was created.

#### Request `[ChannelManagerApiAddress]/changeNotificaton`

```javascript
{
    "clientToken": "[Mews Client token]",
    "connectionToken": "[Token of a concrete connection]",
    "type": 0
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `type` | `int` | required | [`Change Notification Type`](channel-manager-api.md#change-notification-type) code. |

##### Change Notification Type

| Code | Description |
| --- | --- |
| `0` | Created |
| `1` | Updated |
| `2` | Activated |
| `3` | Deactivated _In Mews, whole connection can be disabled._ |
| `4` | Deleted |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.
