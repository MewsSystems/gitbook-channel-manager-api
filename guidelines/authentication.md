# Authentication

To interact with the __Mews Channel Manager API__, you need to authenticate your requests using Client Tokens and Connection Tokens. This ensures that only authorized clients can access and manipulate data. Client Tokens are unique to the API client, i.e. the Channel Manager application, while Connection Tokens are unique to the connection with a property.

## Connections and tokens

There can be multiple connections between one property and one channel manager. All API operations on both sides require `clientToken` to be present in the request. On the Mews side, `clientToken` corresponds to a specific channel manager, so the same `clientToken` should be used for all connections.
On the Channel Manager side, the `clientToken` can be the same or different, as agreed between the parties, as long as it is unique for Mews and consistent across all Mews connections.
Additionally, all operations regarding a specific connection with a property require `connectionToken`, which is unique for each connection.

* **Client Token**: This token uniquely identifies the API client, i.e. the Channel Manager application consuming the API.
* **Connection Token**: This token identifies the connection with the property or enterprise whose data and services you have access to.

## Obtaining tokens

To get started using the API, you need to obtain the necessary tokens:

* **Client Token**: This token is provided by Mews and is essential for authenticating your Channel Manager application. You will receive it during the initial setup process.
* **Connection Token**: This token is specific to each connection with a property. You can obtain it in three ways:
  * **From Mews**: When authorized by the property, Mews will provide you with the Connection Token.
  * **Through the property**: The property can also obtain the token via __Mews Operations__.
  * **Using the API**: If your application supports [Mews: Get properties](../mews-operations/configuration.md#get-properties), you can recover the Connection Tokens for all connections the property supports for this client application. In this case, an employee email address is used as an authorization token.

> For detailed steps and additional information, refer to [Your integration journey](../your-journey/README.md).

## Using tokens in requests

Most API operations require both `clientToken` and `connectionToken` to be included in the request body.

**Example request body**:
```json
{
    "clientToken": "73F621085FD945J3A3BC01CFB5F30BD7", 
    "connectionToken": "806F7EFC114743KFL84FD5450F816AB2-BB2BF4D3F4FJ81AF85J1630108B1471",
    "messageId": "13620420",    
    "availabilityBlock": {
        ...
    }  
}
```

## Error handling

If authentication fails, the API will return an error response with details about the error. For full details about responses and error codes, see [Responses](responses.md).

**Example error response**:
```json
{
   "success": false,
   "errors":[
      {
         "code":8,
         "message":"Invalid 'clientToken' or 'connectionToken'."
      }
   ]
}
```

**Connection related error codes**:
- **Code 3**: "Connection not found" - The specified connection could not be found in the system.
- **Code 8**: "Invalid authentication" - The request has failed authentication, e.g. an authentication token is invalid or has expired.
