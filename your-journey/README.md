# Your integration journey

For a general overview and step-by-step guide to the integration process for all **Mews Open API** integrations, see [Your journey with Mews](https://mews-systems.gitbook.io/open-api/your-journey). This page focuses specifically on developing an integration with the **Mews Channel Manager API**.

## Setting up the integration

To get started, follow these steps:

1. **Obtain your Client Token**: Request your Client Token from Mews. This token identifies you as a Mews client when making requests to the Mews Channel Manager API. Note: The Client Token is consistent across all connected properties, but differs between environments (e.g. Test and Production).

2. **Set up Channel Manager side endpoints**: Set up your channel manager side API endpoints to accept requests from Mews, using the same Client Token provided for Mews-side authentication. While you can use a different token, we recommend using the same one for simplicity.

3. **Share endpoint URLs with Mews**: Provide Mews with the endpoint URLs for supported Channel Manager API operations. Tip: You can submit these URLs gradually, especially during development. Production URLs can be shared after certification if preferred.

## Connecting a property

To connect a property, follow these steps:

1. **Create the connection in Mews**: A connection must first be established in Mews Operations. This can be done either by the property or by Mews Support.
    * When created, all spaces and rates are assigned default mapping codes automatically.
    * If your system requires specific mapping codes, provide them during this step, as Mews default codes can be modified by the property or Mews Support.
   
2. **Obtain the Connection Token**: The Connection Token identifies the specific link between the property and your system. You can get it in two ways:
    * Use the [Get properties](../mews-operations/configuration.md#get-properties) API operation to retrieve all connections and identify new ones.
    * Request the token directly from the property or Mews Support.

3. **Retreive the connection configuration**: Fetch the connection’s configuration to complete the setup:
    * Use the [Get configuration](../mews-operations/configuration.md#get-configuration) API operation.
    * Alternatively, request the configuration details from the property or Mews Support.

4. **Configure the connection**: Configure the connection on your channel manager system to finalize the integration.

> **Test environment support**: For integrations in the [Test environment](../guidelines/environments.md), Mews integration specialists will handle connection requests.

## Getting certified

Before moving your integration to the Production environment, you’ll need to complete the Mews certification process:

* Submit the [certification form](https://mews.typeform.com/to/ehTUz7) to begin.
* Learn more:
  * [Certification](certification.md)
  * [Certification tests](certification-tests.md)
  * [Channel API Certification: What to expect](https://help.mews.com/s/article/channel-api-certification-what-to-expect?language=en_US)

For additional support, visit the [Mews Help Center](https://help.mews.com).
