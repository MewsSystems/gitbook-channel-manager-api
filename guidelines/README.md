# Guidelines

Here we provide general guidelines and useful links to help you through the process of integrating with the __Mews Channel Manager API__.
The process is summarised in [Integration process](process.md). Note that unless specified otherwise, the guidelines in this section apply to both the Mews side of the API and the Channel Manager side of the API.

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

### Channels

For the list of supported sales channels, including Online Travel Agents or OTAs, please use the [Get channels](../mews-operations/configuration.md#get-channels) operation.
If you are connected to a sales channel which is not listed, please contact Mews via [partnersuccess@mews.com](mailto://partnersuccess@mews.com) and we can update the list.

### PCI Compliance

For information on Mews PCI compliance, including current certification, please follow the links below.

* [Mews PCI compliance](https://mews.force.com/s/article/pci-compliance?language=en_US)
* [Mews PCI certificate](https://www.mews.com/en/platform-documentation)

### Whitelisting

Whitelisting (also called 'allowlisting') is a common security measure which can be applied to a system to allow only specified external systems to talk to it. This has traditionally been achieved using IP address-based firewall rules. However, this approach does not work with modern cloud based architectures, which use dynamic and shared IP addresses, proxy servers and elastic resources. For this reason, we do not support the use of IP address whitelists for our APIs and we cannot supply a list of IP addresses for our APIs.
