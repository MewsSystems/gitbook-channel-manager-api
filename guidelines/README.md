# Guidelines

Here we provide general guidelines and useful links to help you through the process of integrating with the __Mews Channel Manager API__. For information on the partner journey and on the certification process, see [Your integration journey](../your-journey/README.md). Note that unless specified otherwise, the guidelines in this section apply to both the Mews side of the API and the Channel Manager side of the API.

> ### Terminology
> We use the term *Property* to describe the hotel, hostel or other enterprise; we use the term *Space* to refer to a hotel room, dormitory bed or other type of bookable space, and *Space Type* to refer to the category of *Space* offered by the *Property*. Although typically a room in a hotel, this could be for example a squash court in a sports facility.
> For a full description of all the terms used in the API, see the [Mews Glossary for Open API users](https://help.mews.com/s/article/Mews-Glossary-for-Open-API-users?language=en_US).

## Connections and tokens

There can be multiple connections between one property and one channel manager. All API operations on both sides require `clientToken` to be present in the request. On the Mews side, `clientToken` corresponds to a specific channel manager, so the same `clientToken` should be used for all connections. On the Channel Manager side, the `clientToken` can be the same or different, as agreed between the parties, as long as `clientToken` is unique for Mews and the same for all Mews connections. All operations regarding a specific connection with a specific property require `connectionToken` as well. `connectionToken` is unique for each connection.

## Channels

For the list of supported sales channels or sources, including Online Travel Agents (OTAs), please use the [Get channels](../mews-operations/configuration.md#get-channels) operation.

| <div style="width:350px">'How to' use case</div> | API Operations |
| :-- | :-- |
| How to get the list of sales channels or sources | [Mews: Get channels](../mews-operations/configuration.md#get-channels) |

### Sales channel not listed

If you are connected to a sales channel which is not listed, and you are certified to use Sources, then you can add the new channel source through the API. Simply send the reservation with the named sales channel or source, using [Mews: Process group](../mews-operations/reservations.md#process-group) and the `sources` array, but leave the new source `code` value as `null`. Mews will add the new channel to the database and auto-generate a new code. To verify the new channel has been added, and to fetch the new code, use [Get channels](../mews-operations/configuration.md#get-channels). You can then use that code for future reservations as normal.

For example:

```json
"sources": [
  {
    "code": null,
    "name": "my new source",
    "type" : 0,
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

## PCI Compliance

For information on Mews PCI compliance, including current certification, please follow the links below.

* [Mews PCI compliance](https://mews.force.com/s/article/pci-compliance?language=en_US)
* [Mews PCI certificate](https://www.mews.com/en/platform-documentation)

## Whitelisting

Whitelisting (also called 'allowlisting') is a common security measure which can be applied to a system to allow only specified external systems to talk to it. This has traditionally been achieved using IP address-based firewall rules. However, this approach does not work with modern cloud based architectures, which use dynamic and shared IP addresses, proxy servers and elastic resources. For this reason, we do not support the use of IP address whitelists for our APIs and we cannot supply a list of IP addresses for our APIs.
