# Mews API Operations: Inventory

## Set Inventory

\[`sync`\] This method can be used to **update** the rate plan - space type mapping relations in the connection.
* All mapping relations need to be sent.
* Mapping relations that are defined in Mews for the connection, but missing in the call, are deleted.
* It is possible to create a new rate-space category combination for the rates and space categories that are already in Mews.
* It is impossible to add a new rate or space category.

### Request

`[PlatformAddress]/api/channelManager/v1/setInventory`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "inventoryMappings": [
        {
            "ratePlanCode": "FF",
            "spaceTypeCode": "KD"
        },
        {
            "ratePlanCode": "FF",
            "spaceTypeCode": "QD"
        }
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `inventoryMappings` | [`Inventory Mapping`](#inventory-mapping) collection | required | Defines all rate plan - space type mapping relations. |

### Response

[Simple response](../general/README.md#simple-response) is expected.

## Request ARI Update

\[`async`\] This method allows channel manager to request an ARI data update for certain space types and rate plans in addition to the changes automatically sent in the next [delta](channel-manager-api.md#delta-inventory-update-mode) update. The requested data will be sent by Mews asynchronously via push operations of channel manager API in the next delta update.

### Request

`[PlatformAddress]/api/channelManager/v1/requestAriUpdate`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "from": "2018-01-01",
    "to": "2018-02-01",
    "ariType": [
        1,
        2,
        3
    ],
    "spaceTypeCodes": [
        "KD",
        "QD"
    ],
    "ratePlanCodes": [
        "FF"
    ]
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `from` | `string` | required | Start of requested update in format `"yyyy-MM-dd"`. |
| `to` | `string` | required | End of requested update in format `"yyyy-MM-dd"` \(included\). |
| `ariType` | `int` collection | optional | [`ARI Types`](#ari-types) ~~to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._ |
| `spaceTypeCodes` | `string` collection | optional | ~~Space types to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._ |
| `ratePlanCodes` | `string` collection | optional | ~~Rate plans to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._. |

#### ARI Types

| Code | Description |
| :-- | :-- |
| `1` | Availability |
| `2` | Prices |
| `3` | Restrictions |

### Response

[Simple](../general/README.md#simple-response) will determine whether the ARI update was accepted for processing or not.
