# Guidelines

Here we provide general guidelines and useful links to help you through the process of integrating with the __Mews Channel Manager API__.
The process is summarised in [Integration Process](process.md).

> ### Terminology
> We use the term *Property* to describe the hotel, hostel or other enterprise;
> we use the term *Space* to refer to a hotel room, dormitory bed or other type of bookable space, and *Space Type* to refer to the category of *Space* offered by the *Property*.
> Although typically a room in a hotel, this could be for example a squash court in a sports facility.
> For a full description of all the terms used in the API, see the [Mews Glossary for Open API users](https://help.mews.com/s/article/Mews-Glossary-for-Open-API-users?language=en_US).

### Connections and tokens

There can be multiple connections between one property and one channel manager.
All API operations on both sides require `clientToken` to be present in the request.
On the Mews side, `clientToken` corresponds to a specific channel manager, so the same `clientToken` should be used for all connections.
On the Channel Manager side, the `clientToken` can be the same or different, as agreed between the parties, as long as `clientToken` is unique for Mews and the same for all Mews connections.
All operations regarding a specific connection with a specific property require `connectionToken` as well.
`connectionToken` is unique for each connection.

> ### Channels
> For the list of supported sales channels, including Online Travel Agents or OTAs, see [Channels](../channels/README.md).
> If you are connected to a sales channel which is not listed, please contact Mews via partnersuccess@mews.com and we can update the list.

## Requests

Both sides of the API accept only `HTTPS POST` requests with Content-Type set to `application/json` and with JSON body content depending on the operation to be performed.

### Mews side

All Mews API Operations follow this address pattern:

```text
[PlatformAddress]/api/channelManager/v1/[Operation]
```

* **PlatformAddress** - the base address of the MEWS platform, this depends on the environment \(Testing, Staging, Production\)
* **Operation** - the name of the API operation

For each environment, the `clientToken` will be provided to you by Mews. For development purposes, use the [Test Environment](../mews-operations/README.md#test-environment).

#### Security

For security reasons, Mews API endpoints support TLS security protocol of version 1.2 or higher.

### Channel Manager side

All Channel Manager API Operations are expected to follow this address pattern:

```text
[PlatformAddress]/[Operation]
```

* **PlatformAddress** - the base address of the channel manager side of the API
* **Operation** - the name of the API operation

> Note: Please provide Mews with endpoint URLs for the following API operations once they are deployed.
> Mews needs them for both Test environment and Production environment to be able to send data.
> Mews doesn't require all endpoint URLs to be provided at once.
> We recommend to have a different `[PlatformAddress]` for each environment to prevent test data and live data getting mixed up;
> for the same reason _`clientToken`_ should differ between each environment.

__Required__:

* [Update Prices](../channel-manager-operations/operations.md#update-prices)
* [Update Availability](../channel-manager-operations/operations.md#update-availability)
* [Update Restrictions](../channel-manager-operations/operations.md#update-restrictions)
* [Confirm Booking](../channel-manager-operations/operations.md#confirm-booking)

__Optional__:

* [Change Notification](../channel-manager-operations/operations.md#change-notification)

## Responses

Each API request is expected to return a response. The **HTTP status code** of the response will be `200` in case the API request processing is successful as well as if the API request processing fails \(e.g. error during reservation processing, validation error of inventory update message, etc.\).
In case of such failures, the error is provided in the response body.

### Simple response

This response object represents the default, simple response.

```javascript
{
   "success":true
}
```

### Complex response

In case a more complex response is provided, the response object will extend the simple response object.

```javascript
{
   "success":false,
   "error":{
      "code":8,
      "message":"Invalid 'clientToken' or 'connectionToken'."
   }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `success` | `bool` | required | Determinines the result of the operation. |
| `errors` | array of [`Error`](#error) | optional | In case of `"success": false`, this property holds information about the errors that occurred. |
| ~~`error`~~ | ~~[`Error`](#error)~~ | ~~optional~~ | ~~In case of `"success": false`, this property holds information of the error that occurred.~~ |

#### Error

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required | Determines the type of error \(see [ErrorCode](#error-codes)\). |
| `message` | `string` | required | Error message with more details about the error. |
| `categoryCode` | `string` | optional | Category code which caused the error. This is required for category errors and rate category errors. The category will be automatically unsynchronized in Mews. |
| `rateCode` | `string`  | optional | Rate code which caused the error. This is required for rate errors and rate category errors. The rate will be automatically unsynchronized in Mews. |


#### Error codes

| Code | Description |
| :-- | :-- |
| `1` | System error. |
| `2` | Reservation does not exist. When this error code is returned, it should be acknowledged and the reservation should _not_ be re-tried. |
| `3` | Property does not exist. |
| `4` | Space type does not exist. |
| `5` | Rate plan does not exist. |
| `6` | Validation error, e.g. invalid value "XXX" of field "YYY". |
| `7` | Processing error, e.g. processing of the booking would violate some internal PMS limitation. |
| `8` | Invalid authorization. |
| `9` | Rate error, e.g. rate plan XX prices updates cannot be accepted due to the settings in the channel manager extranet. |
| `10` | Category error, e.g. space category XX availability updates cannot be accepted due to the settings in the channel manager extranet. | 
| `11` | Rate category error, e.g. category was removed from the rate plan in channel manager extranet. |
| `12` | Availability configuration error, e.g. availability updates blocked from the channel manager extranet. |
| `13` | Prices configuration error, e.g. price updates blocked from the channel manager extranet. |
| `14` | Restrictions configuration error, e.g. restriction updates blocked from the channel manager extranet. |
