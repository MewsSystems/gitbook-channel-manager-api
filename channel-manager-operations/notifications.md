# Channel Manager API Operations: Notifications

## Change notification

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

[Synchronous simple response](../guidelines/responses.md#synchronous-simple-response) is expected to determine whether the update was accepted or not.
