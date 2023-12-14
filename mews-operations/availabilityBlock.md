# Mews API Operations: Availability blocks

## Confirm availability block confirmation

### Request

`[PlatformAddress]/api/channelManager/v1/processAvailabilityBlockConfirmation`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "relatedMessageId": "[Id of message which request relates to]",
    "success": false,
    "errors": [{
      "code": 10,
      "message": "Invalid category code",
      "categoryCode": "ABC"
   }]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `relatedMessageId` | `string` | required | Id of message which requests relates to. |
| `success` | `bool` | required | Determinines the result of the operation. |
| `errors` | array of [`Error`](../guidelines/responses.md#error) | optional | In case of `"success": false`, this property holds information about the errors that occurred. |
| `code` | `string` | required | Unique reference code from external system for the block. Will be sent with every block changes. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the block. |


### Response

[Synchronous simple response](../guidelines/responses.md#Synchronous-simple-response) is expected.