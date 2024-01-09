# Responses

Each API request is expected to return a response. The **HTTP status code** of the response is `200` in case the API request processing is successful as well as if the API request processing fails, e.g. if there is an error during reservation processing, or an inventory update message fails validation.
In case of such failures, details about the error are provided in the response body.

> **Important:** In the case of errors with rate codes or category codes, details of the affected codes must be returned in the response body.
> These codes will also be unsynchronized, i.e. disabled.

## Synchronous simple response

This response object represents the default response, in case of success.

```javascript
{
   "success": true,
   "asyncConfirmation": false
}
```

## Synchronous error response

In case of error, the response object will extend the simple response object with details about the error.
In case of errors with rate codes or category codes, details of the affected codes must also be returned.
See the [Error codes](#error-codes) table below for further details about specific errors, including guidance on system behaviour.

### Example \#1

```javascript
{
   "success": false,
   "error":{
      "code":8,
      "message":"Invalid 'clientToken' or 'connectionToken'."
   }
}
```

### Example \#2

```javascript
{
   "success":false,
   "error":{
      "code":9,
      "message":"Invalid rate code",
      "rateCode":"ABC"
   }
}
```

### Example \#3

```javascript
{
   "success":false,
   "error":{
      "code":10,
      "message":"Invalid category code",
      "categoryCode":"XYZ"
   }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `success` | `bool` | required | Determinines the result of the operation. |
| `asyncConfirmation` | `bool` | optional | Determinines if there will be following asynchronous response. |
| `errors` | array of [`Error`](#error) | optional | In case of `"success": false`, this property holds information about the errors that occurred. |

### Error

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required | Determines the type of error \(see [Error codes](#error-codes)\). |
| `message` | `string` | required | Error message with more details about the error. |
| `categoryCode` | `string` | optional | Category code which caused the error. This is _required_ for category errors and rate category errors. The category will be automatically unsynchronized in Mews. |
| `rateCode` | `string`  | optional | Rate code which caused the error. This is _required_ for rate errors and rate category errors. The rate will be automatically unsynchronized in Mews. |


### Error codes

| Code | Description |
| :-- | :-- |
| `1` | **System error**<br>Unspecified system error. Message may be re-sent after an interval of time. |
| `2` | **Reservation error**<br>Reservation does not exist. When this error code is returned, it should be acknowledged and the reservation should _not_ be re-tried. |
| `3` | **Property error**<br>Property does not exist. |
| ~~`4`~~ | ~~**Space error**<br>Space type does not exist.~~ (deprecated - use code 10 instead) |
| ~~`5`~~ | ~~**Rate error**<br>Rate plan does not exist.~~ (deprecated - use code 9 instead) |
| `6` | **Validation error**<br>Validation error, e.g. invalid value "XXX" of field "YYY". |
| `7` | **Processing error**<br>Processing error, e.g. processing of the booking would violate some internal PMS limitation. |
| `8` | **Invalid authorization** |
| `9` | **Rate error**<br>Rate error, e.g. rate plan XX prices updates cannot be accepted due to the settings in the channel manager extranet. The affected rate code must be supplied in the `rateCode` property. This rate code will then be unsynchronized. |
| `10` | **Category error**<br>Category error, e.g. space category XX availability updates cannot be accepted due to the settings in the channel manager extranet. The affected category code must be supplied in the `categoryCode` property. The category will then be deleted from the rate. | 
| `11` | **Rate category error**<br>Rate category error, e.g. category was removed from the rate plan in channel manager extranet. The affected rate code and category code must be supplied. The category will be deleted from the rate. |
| `12` | **Availability error**<br>Availability configuration error, e.g. availability updates blocked from the channel manager extranet. This error automatically disables availability updates. |
| `13` | **Prices error**<br>Prices configuration error, e.g. price updates blocked from the channel manager extranet. This error automatically disables price updates. |
| `14` | **Restrictions error**<br>Restrictions configuration error, e.g. restriction updates blocked from the channel manager extranet. This error automatically disables restrictions updates. |