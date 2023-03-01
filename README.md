# Mews Channel Manager API

Welcome to the __Mews Channel Manager API__. This is the Mews API for distribution and sales channels, supporting two main use cases:
distributing availability, rates and inventory data to sales channels; and accepting reservations from sales channels.
Typically, the users of this API are Channel Managers, which act as hubs for managing the various sales channels that a property may be connected to.
The API connects these Channel Managers with __Mews Operations__.

The integration functionality is two-way, so the API is implemented in two parts:

* The [Mews side](mews-operations/README.md) receives requests from external Channel Managers, including new reservations
* The [Channel Manager side](channel-manager-operations/README.md) receives requests from Mews, including availability updates

To fully implement the functionality of the API, you will need to not only make requests to Mews through defined endpoints on the Mews side,
but also create your own endpoints to accept data from Mews on the Channel Manager side.

To get started, see our [Guidelines](guidelines/README.md) for information on how to connect, what authentication tokens you need, Mews terminology, and more besides.
If you encounter any issues using the API, or you have a question or special request, please get in touch via [partnersuccess@mews.com](mailto://partnersuccess@mews.com).

> ### Changes to this API
> * For the history of changes to the API, see the [Changelog](changelog/README.md)
> * To track changes and updates, you can follow the [GitHub repository](https://github.com/MewsSystems/gitbook-channel-manager-api/tree/master)
