# Channel Manager API Operations: Reservations

## Confirm Booking

\[`sync`\] This method is used when Mews confirms a booking sent via [Process Group](../mews-operations/reservations.md#process-group).
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
| `error` | [`Error`](../guidelines/responses.md#error) | optional | In case of processing failure, this provides a description of the error. |
| `reservations` | [`Reservation Confirmation`](#reservation-confirmation) collection | optional | Confirmation details for each individual reservation in the group. |

#### Reservation Confirmation

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Unique reference code for the individual reservation. |
| `confirmationNumber` | `string` | required | Mews confirmation number for the individual reservation. |

### Response

[Simple response](../guidelines/responses.md#simple-response) is expected to determine whether the update was accepted or not.
