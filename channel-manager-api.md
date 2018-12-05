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

Mews automatically sends changes in Inventory \(once connection is set up\). Data sent in this mode is just the changed data from the last update. This is a completely automated process and there is no way to trigger just the delta update to be sent. Delta Inventory updates are sent repeatedly until they are successfully accepted by channel manager.

## Expected Operations

### Update Prices

\[`sync`\] This method is used when Mews updates prices of rate plans.

#### Request `[ChannelManagerApiAddress]/updatePrices`

```javascript
{
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "ratePrices":[
      {
         "spaceTypeCode":"QD",
         "ratePlanCode":"FF",
         "from":"2018-01-01",
         "to":"2018-01-31",
         "prices":[
            {
               "guestCount":1,
               "amount":95,
               "currencyCode":"USD"
            },
            {
               "guestCount":2,
               "amount":100,
               "currencyCode":"USD"
            }
         ]
      }
   ]
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `ratePrices` | [`Rate Price`](channel-manager-api.md#rate-price) collection | required | Collection of prices for all rate plan - space type - date - person count combinations. |

#### Rate Price

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `prices` | [`Price`](channel-manager-api.md#price) collection | required | Collection of prices for each person count for certain rate plan - space type - date combination. |

#### Price

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `guestCount` | `int` | required | The person count for the rate price. |
| `amount` | `decimal` | required | The price amount. |
| `currencyCode` | `string` | required | The 3 letter code of the rate price currency. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Update Availability

\[`sync`\] This method is used when Mews updates availability of space types.

#### Request `[ChannelManagerApiAddress]/updateAvailability`

```javascript
{
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "availabilities":[
      {
         "spaceTypeCode":"QD",
         "from":"2018-01-01",
         "to":"22018-01-31",
         "availability":10
      },
      {
         "spaceTypeCode":"KD",
         "from":"2018-01-01",
         "to":"2018-01-30",
         "availability":5
      }
   ]
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `availabilities` | [`Availability`](channel-manager-api.md#availability) collection | required | Collection of availability of space types. |

#### Availability

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
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "restrictions":[
      {
         "spaceTypeCode":"QD",
         "ratePlanCode":"FF",
         "from":"2018-01-01",
         "to":"2018-01-31",
         "state":[
            1
         ],
         "minLos":1,
         "maxLos":7
      },
      {
         "spaceTypeCode":"KD",
         "ratePlanCode":"FF",
         "from":"2018-01-01",
         "to":"2018-01-30",
         "minLos":2,
         "maxLos":7
      },
      {
         "spaceTypeCode":"KD",
         "ratePlanCode":"NR",
         "from":"2018-01-01",
         "to":"2018-01-01",
         "state":[
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

#### Restriction

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `spaceTypeCode` | `string` | required | Mapping code of the space type. |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `from` | `string` | required | Start date of the updated interval in `"yyyy-MM-dd"` format. |
| `to` | `string` | required | End date \(included\) of the updated interval in `"yyyy-MM-dd"` format. |
| `minLos` | `int` | optional | Minimal LOS during the interval. _Has to be at least 1._ |
| `maxLos` | `int` | optional | Maximal LOS during the interval. _Has to be at least _`minLos`_._ |
| `state` | `int` collection | required | [Restriction State](channel-manager-api.md#restriction-state) code. |

#### Restriction State

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

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Confirm Booking

\[`sync`\] This method is used when Mews confirms a booking sent via [Process Group](mews-api.md#process-group). It is used for both successful confirmation or notification that processing failed.

#### Request `[ChannelManagerApiAddress]/confirmGroup`

```javascript
{
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "channelManagerId":"123456",
   "reservations":[
      {
         "code":"01",
         "confirmationNumber":"PMS-001"
      }
   ]
}
```

or

```javascript
{
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "channelManagerId":"123456",
   "error":{
      "code":4,
      "message":"There is no space type with mapping code 'ST'."
   }
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `channelManagerId` | `string` | required | Booking code. |
| `error` | [`Error`](https://github.com/mews-systems/channel-manager-api/tree/da74c52f29ad04bf712acb397e311d8e5e8ba90b/general-api.md#error) | optional | If booking processing failed, holds the explanation. |
| `reservations` | [`Reservation Confirmation`](channel-manager-api.md#reservation-confirmation) collection | optional | Confirmation of each reservation. |

#### Reservation Confirmation

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Unique code of the reservation. |
| `confirmationNumber` | `string` | required | Mews confirmation number of the reservation. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

### Change Notification

\[`sync`\] This method is used when Mews informs channel manager that there was change in Mews that affects the connection configuration. This API call is optional to implement, it is used in case of fully-automated connection.

#### Request `[ChannelManagerApiAddress]/changeNotificaton`

```javascript
{
   "clientToken":"[Mews Client token]",
   "connectionToken":"[Token of a concrete connection]",
   "type": 0
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `type` | `int` | required | [`Change Notification Type`](channel-manager-api.md#change-notification-type) code |

#### Change Notification Type

| Code | Description |
| --- | --- |
| `0` | Created |
| `1` | Updated |
| `2` | Activated |
| `3` | Deactivated _In Mews, whole connection can be disabled._ |
| `4` | Deleted |

#### Response

[Plain response](general-remarks.md#plain-response) is expected to determine whether the update was accepted or not.

