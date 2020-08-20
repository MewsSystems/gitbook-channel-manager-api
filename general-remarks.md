# General remarks

## Contents

* [Requests](general-remarks.md#requests)
  * [Mews API side](general-remarks.md#mews-api-side)
    * [Security](general-remarks.md#security)
  * [Channel Manager API side](general-remarks.md#channel-manager-api-side)
* [Responses](general-remarks.md#responses)
  * [Plain response](general-remarks.md#plain-response)
    * [Error](general-remarks.md#error)
* [Setup process](general-remarks.md#setup-process)
  * [Integration setup](general-remarks.md#integration-setup)
  * [Connect new property](general-remarks.md#connect-new-property)
  * [Certification](general-remarks.md#certification)
  * [Examples](general-remarks.md#examples)

The documentation uses term **Property** to describe a client of channel manager or PMS and **Space Type** to refer to a space sold/offered by the **Property**. In hospitality world, a **Property** is a hotel \(hostel, etc.\) and **Space Type** is a room \(bed, etc.\). But **Property** could also \(for example\) be some sport facility and then the **Space Type** would refer to a squash court, that is why those unusual, yet general, terms.

There can be multiple connections between one property and one channel manager. All operations of both API sides require `clientToken` to be present in the request, On Mews API side, the `clientToken` determines a concrete channel manager, so it is same for all connections. The `clientToken` on the [Channel Manager API](channel-manager-api.md) side can be same or different, it is up to an agreement, as long as the `clientToken` is unique for Mews and same for all Mews connections. All operations regarding an established connection with property require `connectionToken` as well, `connectionToken` determines a concrete connection between the channel manager and a Mews hotel, it is unique for each connection.

If you are interested in changes and updates of this API, check [Changelog](changelog.md#changelog).

If you are interested in the list of Channels \(i.e. OTAs\), check [Channels](channels.md#channels). If you are connected to some Channels that are not in the list, please provide them to us, we will add them with some code.

## Requests

Both sides of the API accept only `HTTP POST` requests with `Content-Type` set to `application/json` and JSON content depending on the operation to be performed.

### Mews API side

All operations follow this address pattern:

```text
[PlatformAddress]/api/channelManager/v1/[Operation]
```

* **PlatformAddress** - Base address of the MEWS platform, depends on environment \(testing, staging, production\).
* **Operation** - Name of the operation to be performed.

In each environment, the `clientToken` will be provided to you by Mews. For development purposes, use the [Test Environment](mews-api.md#test-environment).

#### Security

For security reasons, Mews API endpoints support TLS security protocol of version 1.2 or higher.

### Channel Manager API side

Expected pattern is

```text
[PlatformAddress]/[Operation]
```

* **PlatformAddress** - Base address of the channel manager API.
* **Operation** - Name of the operation to be performed.

_Note: Please provide us endpoint URLs for following operations once they are implemented / deployed. Mews needs them for both test environment and production environment to be able to send data. Mews doesn't require all endpoint URLs to be provided at once. We recommend to have different \[PlatformAddress\] for both environments to prevent test data and live data getting mixed up; from the same reason the _`clientToken`_ will differ in each environment._

The required operations to be implemented are

* [Update prices](channel-manager-api.md#update-prices)
* [Update availability](channel-manager-api.md#update-availability)
* [Update restrictions](channel-manager-api.md#update-restrictions)

The recommended operation to be implemented is

* [Confirm booking](channel-manager-api.md#confirm-booking)

The optional operation is

* [Change notification](channel-manager-api.md#change-notification)

## Responses

Each API call is expected to return a response. The **Http status code** of response will be `200` in case the API call processing was successful as well as if the API call processing failed \(e.g. error during booking processing, validation error of inventory update message, etc\). In later case, the error will be provided in the response.

### Plain response

This response object representing the basic response. In case there is a more complex response is expected, that response object will extend this simple response object.

```javascript
{
   "success":true
}
```

Or

```javascript
{
   "success":false,
   "error":{
      "code":8,
      "message":"Invalid 'clientToken' or 'connectionToken'."
   }
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `success` | `bool` | required | `true` or `false` determining result of operation. |
| `error` | [`Error`](general-remarks.md#error) | optional | In case of `"success": false`, this property holds information of the error that happened. |

#### Error

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `int` | required | [ErrorCode](general-remarks.md#error-codes) code - determines type of the error. |
| `message` | `string` | required | Error message with more details of the error. |

#### Error codes

| Code | Description |
| --- | --- |
| `1` | System error |
| `2` | Reservation does not exist. - e.g When this error code returned, it should be acknowledged and reservation should not be re-tried. |
| `3` | Property does not exist |
| `4` | Space type does not exist |
| `5` | Rate plan does not exist |
| `6` | Validation error - e.g. Invalid value "XXX" of field "YYY". |
| `7` | Processing error - e.g. Processing of the booking would violate some internal PMS limitation. |
| `8` | Invalid authorization |

## Setup Process

### Integration setup

1. Obtain **Access Token** from Mews, which will identify the channel manager as a Mews client in the requests to the Mews API. The token is same for all connected hotels via the channel manager.
2. Setup **Access Token** for Mews, which will identify Mews as a client of the channel manager in the request to the Channel Manager API. It can be same token \(recommended, simpler\) or it can be different one.
3. Provide URL addresses of the Channel Manager API operations.
   * When implemening the Channel Manager API side, you can provide the endpoint URLs one by one.
   * Production URLs can be provided after certification is completed. Ideally production environment should differ from test evironment.

### Connect new property

1. When a property wants to be connected with a channel manager \(you\), the connection should be created in Mews first \(by the property or Mews support\).
   * When a connection is created in Mews, all spaces and rates gets assigned default mapping codes automatically. If the channel manager requires own mapping codes, they need to be passed as part of this step as the Mews default mapping codes can be easily altered by the property / Mews support.
2. Obtain **Conection code** generated by Mews
   * You can download all connections via [Get Properties](mews-api.md#get-properties) API call and from that list you will know which connection is new to you.
   * You can ask the property / Mews support for the `connectionToken` when setting up the connection on your side.
3. Obtain connection configuration
   * You can download configuration of a concrete connection via [Get Configuration](mews-api.md#get-configuration) API call
   * You can ask the property / Mews support for the file with configuration.
4. Configure the connection on the channel manager extranet.

_Note: On _[_Test Environment_](mews-api.md#test-environment)_ our Integration specialists will be taking care of all those requests on Mews side._

### Certification

Once the Channel manager API is developed, Mews needs to be sure it is developed correctly before the integration can be moved to production environment.

During [Certification](certification.md) process, a series of tests will be performed to check that all requests contain all required elements, requests are not unnecesarily, errors are handled properly, etc.

#### Examples

_Comming soon._
