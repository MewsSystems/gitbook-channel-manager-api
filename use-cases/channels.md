# Channels

The Channels use case describes how to retrieve the list of sales channels and how to add a new sales channel if it is not already listed.

## Fetching the list of channels

For the list of supported sales channels or sources, including Online Travel Agents (OTAs), use the [Mews: Get channels](../mews-operations/configuration.md#get-channels) operation.

| <div style="width:350px">'How to' use case</div> | API Operations |
| :-- | :-- |
| How to get the list of sales channels or sources | [Mews: Get channels](../mews-operations/configuration.md#get-channels) |

## Adding a new channel

If you are connected to a sales channel which is not listed, and you are certified to use Sources, then you can add the new channel source through the API. Simply send the reservation using [Mews: Process group](../mews-operations/reservations.md#process-group) and use the `sources` array, but when specifying the unlisted source, leave the `code` value as `null`. Take care to specify the correct `name` and `type` for the source. Mews will add the new channel to the database and auto-generate a new code for it.
To verify the new channel has been added, and to fetch the new code, use [Mews: Get channels](../mews-operations/configuration.md#get-channels). You can then use that code for future reservations as normal.

For example:

```json
"sources": [
  {
    "code": null,
    "name": "my new source",
    "type" : 6,
    "isPrimary": true
  }
]
```

If you are connected to a sales channel which is not listed, but you are _not_ certified to use Sources, then please contact Mews via [partnersuccess@mews.com](mailto://partnersuccess@mews.com) and we can update the list manually.

| <div style="width:350px">'How to' use case</div> | API Operations |
| :-- | :-- |
| How to add a new sales channel or source (if certified) | [Mews: Process group](../mews-operations/reservations.md#process-group) |
| How to add a new sales channel or source (if _not_ certified) | Contact Mews via [partnersuccess@mews.com](mailto://partnersuccess@mews.com) |
| How to verify the new sales channel or source | [Mews: Get channels](../mews-operations/configuration.md#get-channels) |
