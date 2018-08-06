# Certification

## Pre-Certification

1. Email integrations@mewssystems.com to request a Client token to be set up in the Mews [Test Environment](mews-api.md#test-environment).
2. Create the following endpoint URLs for the Mews [Test Environment](mews-api.md#test-environment)
    * [Update Availability](channel-manager-api.md#update-availability) 
    * [Update Prices](channel-manager-api.md#update-prices)
    * [Update Restrictions](channel-manager-api.md#update-restrictions)
    * [Confirm Booking](channel-manager-api.md#confirm-booking) 
    * [Change Notification](channel-manager-api.md#change-notification) \(optional\)
3. Email the endpoints to integrations@mewssystems.com.
4. Set up the connection in the channel manager system.
5. Use the [Get Properties](mews-api.md#get-properties) call to receive the `connectionToken` for the [Test Hotel](mews-api.md#test-environment).
6. Pull hotel information, space mapping and rate mapping information using the [Get Configuration](mews-api.md#get-configuration) call.
7. Map the [Test Hotel](mews-api.md#test-environment) in the UI of the channel manager backend using the data received in the [Get Configuration](mews-api.md#get-configuration) call.

**Note:** Multi-property Mews customers (vacation rentals, apartments, villas, etc.) are often set up in Mews as spaces of one Mews property.  E.g. Property is the Mews Customer and Spaces are the rental properties.  These spaces (properties) can be sent to the channel manager as multiple connections, 1 connection token per space (property), or as a single connection for the entire property manager.

## Certification Tests

These tests will be done during a 60-minute call with the Mews Integrations team and the channel manager.  

### Inventory Push Tests

| Test | Requirement | Scenario | Expected Result | Notes |
| --- | --- | --- | --- | --- |
| **Receive multi-space, multi-rate availability push** | Required | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped from the [Test Hotel](mews-api.md#test-environment) and pushed to the [Update Availability](channel-manager-api.md#update-availability) endpoint. | The channel manager returns ```{ "success": true}``` and data is updated in the channel manager extranet. | Mapping bed and dorm space types is recommended, not required. |
| **Receive multi-space, multi-rate rate push** | Required | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped and pushed to the [Update Prices](channel-manager-api.md#update-prices) endpoint. | The channel manager returns ```{ "success": true}``` and data is updated in the channel manager extranet. | Mapping bed and dorm space types is recommended, not required. |
| **Receive multi-space, multi-rate restriction push** | Required | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped and pushed to the [Update Restrictions](channel-manager-api.md#update-restrictions) endpoint. | The channel manager returns ```{ "success": true}``` and data is updated in the channel manager extranet. | Mapping bed and dorm space types is recommended, not required. |
| **Unmapped Space code error** | Required | An availability push with an unmapped Space type code is sent from the the [Test Hotel](mews-api.md#test-environment) to the [Update Availability](channel-manager-api.md#update-availability) endpoint. | The channel manager returns ```{ "success": false, "error": { "code": XXX, "message": "e.g. Unknown space type mapping code 'ABC'." }}``` as per [plain response](general-remarks.md#plain-response). | The response must have the unmapped Space code. |
| **Unmapped Rate code error** | Required | A rate push with an unmapped Rate plan code is sent from the the [Test Hotel](mews-api.md#test-environment) to the [Update Prices](channel-manager-api.md#update-prices) endpoint. | The channel manager returns ```{ "success": false, "error": { "code": XXX, "message": "e.g. Unknown rate type mapping code 'ABC'." }}``` as per [plain response](general-remarks.md#plain-response) | The response must have the unmapped Rate code. |
| **Connection inactive or not set up error** | Required | The channel manager connection is deactivated from the channel manager extranet and then an full inventory push is sent from the [Test Hotel](mews-api.md#test-environment) to the channel manager's endpoints. | The channel manager returns { "success": false, "error": { "code": XXX, "message": "e.g. The connection is missing or inactive." }} as per [plain response](general-remarks.md#plain-response) | The response must state the connection doesn't exist in the channel manager. |
| **Receive product (package) rates** | Optional | Package Rate and Breakfast product are mapped from the [Test Hotel](mews-api.md#test-environment). Then, a rate push with an package Rate plan code is sent from the the [Test Hotel](mews-api.md#test-environment) to the [Update Prices](channel-manager-api.md#update-prices) endpoint. | The channel manager returns ```{ "success": true}``` and data is updated in the channel manager extranet. |    |

### Reservation Tests

| Test | Requirement | Scenario | Expected Result | Notes |
| --- | --- | --- | --- | --- |
| **Send a single reservation** | Required | A reservation push with [test credit card](mews-api.md#test-credit-cards) information in the [`paymentCard`](mews-api.md#payment-card) object, an [`adultCount`](mews-api.md#reservation) of 2, a [`childCount`](mews-api.md#reservation) of 1, and a companion in the `guests` property of the booking is sent to the [Process Group](mews-api.md#process-group) endpoint. | The channel manager receives a ```javascript { "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Id sent in the reservation push]", "reservations":[ {"code":"[Code sent in the reservation push]", "confirmationNumber":"[Mews confirmation number]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. | The [`paymentCard`](mews-api.md#payment-card) object is required for all reservation pushes in the live environment. |
| **Modify the single reservation** | Required | A modification push with the same [`channelManagerId`](mews-api.md#main-body), the same Reservation [`code`](mews-api.md#reservation) for each booking and at least 1 change to the [`reservations`](mews-api.md#reservation) collection is sent to the [Process Group](mews-api.md#process-group) endpoint. | The channel manager receives a ```{ "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Same id as the original push]", "reservations":[ {"code":"[Same code as the original push]", "confirmationNumber":"[Same confirmation number as the original push]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. |     |
| **Cancel the single reservation** | Required | An cancellation push with the same [`channelManagerId`](mews-api.md#main-body) as the original reservation and no additional data is sent to the [Process Group](mews-api.md#process-group) endpoint. | The channel manager receives a ```{ "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Same id as the original push]", "reservations":[ {"code":"[Same code as the original push]", "confirmationNumber":"[Same confirmation number as the original push]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. |    |
| **Send a reservation with 3 spaces on 2 rate plans and varied daily rates** | Recommended | A reservation push with three bookings in the [`reservations`](mews-api.md#reservation) collection, unique `ratePlanCode` properties and different daily rates for each date is sent to the [Process Group](mews-api.md#process-group) endpoint. | The channel manager receives a ```{ "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Id sent in the reservation push]", "reservations":[ {"code":"[Code sent in the reservation push]", "confirmationNumber":"[Mews confirmation number]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. |     |
| **Add/remove a space and add/remove dates from the multi-room reservation** | Recommended | A modification push with the same [`channelManagerId`](mews-api.md#main-body) and Reservation [`code`](mews-api.md#reservation)s as the original reservation, and changes to 2 of the 3 bookings in the [`reservations`](mews-api.md#reservation) collection. | The channel manager receives a ```{ "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Same id as the original push]", "reservations":[ {"code":"[Same code as the original push]", "confirmationNumber":"[Same confirmation number as the original push]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. |
| **Cancel the multi-room reservation** | Recommended | An cancellation push with the same [`channelManagerId`](mews-api.md#main-body) as the original reservation and no additional data is sent to the [Process Group](mews-api.md#process-group) endpoint. | The channel manager receives a ```{ "clientToken":"[Mews Client Token]", "connectionToken":"[Token of a concrete connection]", "channelManagerId":"[Same id as the original push]", "reservations":[ {"code":"[Same code as the original push]", "confirmationNumber":"[Same confirmation number as the original push]"}]}``` push to the [Confirm Booking](channel-manager-api.md#confirm-booking) endpoint. |    |
| **Receive 1 year of inventory data from Mews** | Required | The connection is enabled and Mews sends the next 365 days of inventory data in 52 consecutive pushes to the [Update Availability](channel-manager-api.md#update-availability), [Update Prices](channel-manager-api.md#update-prices) and [Update Restrictions](channel-manager-api.md#update-restrictions) endpoints.  | The channel manager returns ```{ "success": true}``` and data is updated in the channel manager extranet. | Being able to accept this data is required but push size or frequency can be decreased if the channel manager system can't accept it. |

## Evaluation

* If the required tests cannot be completed, Mews will not allow the Channel Manager to advance to the production environment.
  * Once issues are resolved, certification will start over.
* If there are no critical issues discovered during certification, the Channel Manager will be advanced to the production environment.
  * Mews will require unique https production endpoint URLs for the [Channel Manager API](certification.md#channel-manager-api).
  * Mews will issue a new Client token for the Channel manager to use in the live environment.
  * The Channel Manager will issue documenation of their setup process for approval.
