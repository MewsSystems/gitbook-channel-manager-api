# Responses

Every API request is expected to yield a response. The HTTP status code of the response is `200`. This is true regardless of whether the API request processing is successful, or if it fails, e.g. due to errors during reservation processing or validation of an inventory update message. In the event of a failure, the response body contains details about the error. 

> **Important:** In the event of errors involving rate codes or category codes, details of the affected codes must be returned in the response body.
> These codes will also be unsynchronized as a result of the error, i.e. disabled.

## Synchronous simple response

This response object represents the default response, in case of success.

```javascript
{
   "success": true,
   "asyncConfirmation": false
}
```

## Synchronous error response

In case of error, the response object will extend the simple response object with details about the error or errors. In case of any errors with rate codes or category codes, details of the affected codes must also be returned. See the [Error codes](#error-codes) table below for further details about specific errors, including guidance on system behaviour.

### Example \#1

```javascript
{
   "success": false,
   "errors":[
      {
         "code":8,
         "message":"Invalid 'clientToken' or 'connectionToken'."
      }
   ]
}
```

### Example \#2

```javascript
{
   "success": false,
   "errors":[
      {
         "code":9,
         "message":"Invalid rate code",
         "rateCode":"ABC"
      }
   ]
}
```

### Example \#3

```javascript
{
   "success": false,
   "errors":[
      {
         "code":10,
         "message":"Invalid category code",
         "categoryCode":"XYZ"
      }
   ]
}
```

### Example \#4

```javascript
{
   "success": false,
   "errors":[
      {
         "code":9,
         "message":"Invalid rate code",
         "rateCode":"ABC"
      },
      {
         "code":10,
         "message":"Invalid category code",
         "categoryCode":"XYZ"
      }
   ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `success` | `bool` | required | Determinines the result of the operation. |
| `asyncConfirmation` | `bool` | optional | Determinines if there will be following asynchronous response. |
| ~~`error`~~ | ~~[`Error`](#error)~~ | ~~optional~~ | ~~In case of `"success": false`, this property holds information about the error that occurred.~~ **[Deprecated!](../deprecations/README.md)** |
| `errors` | array of [`Error`](#error) | optional | In case of `"success": false`, this property holds information about the error or errors that occurred. |

### Error

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required | Determines the type of error \(see [Error codes](#error-codes)\). |
| `message` | `string` | required | Error message with more details about the error. |
| `categoryCode` | `string` | optional\* | Category code which caused the error. This is _required_ for category errors and rate category errors. The category will be automatically unsynchronized in Mews. |
| `rateCode` | `string`  | optional\* | Rate code which caused the error. This is _required_ for rate errors and rate category errors. The rate will be automatically unsynchronized in Mews. |

> **\*Important**: If the error is a category error (code 10) or rate category error (code 11), then `categoryCode` must be specified.
> If the error is a rate error (code 9) or rate category error (code 11), then `rateCode` must be specified.

### Error codes

| Code | Description |
| :-- | :-- |
| `1` | **System error**<br>Unspecified system error. Message may be re-sent after an interval of time. |
| `2` | **Reservation not found**<br>The specified reservation could not be found in the system. When this error code is returned, it should be acknowledged and the reservation should _not_ be re-tried. |
| `3` | **Connection not found**<br>The specified connection, i.e. property integration, could not be found in the system. |
| ~~`4`~~ | ~~**Space not found**<br>The specified space type could not be found in the system.~~ (deprecated - use code 10 instead) |
| ~~`5`~~ | ~~**Rate not found**<br>The specified rate plan could not be found in the system.~~ (deprecated - use code 9 instead) |
| `6` | **Validation error**<br>The message is incorrectly formed or contains an invalid field, or a field contains an invalid value. |
| `7` | **Processing error**<br>Business logic error occurred when processing the message, for example the message contains a negative amount that must be positive, or a reservation contains a start date that is later than its end date. The `message` field contains details of the specific processing error.
| `8` | **Invalid authentication**<br>The request has failed authentication, e.g. an authentication token is invalid or has expired. |
| `9` | **Rate error** OR **Disabled operation**<br>This error code has a different meaning, depending on which side is sending the error.<br><br>**Rate error** (CHM side): The specified rate code is invalid, or the rate cannot be accepted. The affected rate code must be supplied in the `rateCode` property. This rate code will then be unsynchronized.<br><br>**Disabled operation** (Mews side): This API operation is not enabled for this integration, e.g. the property administrator has disabled the ability to receive reservations via the Channel Manager integration in Mews. |
| `10` | **Category error**<br>Space category error, e.g. space category XX availability updates cannot be accepted due to the settings in the Channel Manager. The affected category code must be supplied in the `categoryCode` property. The category will then be deleted from the rate. | 
| `11` | **Rate category error**<br>Rate category error, e.g. the specified space category was removed from the specified rate plan in the Channel Manager. The affected rate code and category code must be supplied. The category will be deleted from the rate. |
| `12` | **Availability error**<br>Availability configuration error, e.g. availability updates are blocked by the Channel Manager. This error automatically disables availability updates. |
| `13` | **Prices error**<br>Prices configuration error, e.g. price updates are blocked by the Channel Manager. This error automatically disables price updates. |
| `14` | **Restrictions error**<br>Restrictions configuration error, e.g. restriction updates are blocked by the Channel Manager. This error automatically disables restrictions updates. |
