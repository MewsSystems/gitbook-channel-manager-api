# Responses

Each API request is expected to return a response. The **HTTP status code** of the response will be `200` in case the API request processing is successful as well as if the API request processing fails \(e.g. error during reservation processing, or a validation error of inventory update message\).
In case of such failures, details about the error are provided in the response body.

## Simple response

This response object represents the default response, in case of success.

```javascript
{
   "success":true
}
```

## Error response

In case of error, the response object will extend the simple response object with details about the error.

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

### Error

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `int` | required | Determines the type of error \(see [Error codes](#error-codes)\). |
| `message` | `string` | required | Error message with more details about the error. |
| `categoryCode` | `string` | optional | Category code which caused the error. This is required for category errors and rate category errors. The category will be automatically unsynchronized in Mews. |
| `rateCode` | `string`  | optional | Rate code which caused the error. This is required for rate errors and rate category errors. The rate will be automatically unsynchronized in Mews. |


### Error codes

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
