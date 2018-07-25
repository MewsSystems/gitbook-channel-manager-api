# Certification

## Contents

  - [Pre-Certification](#pre-certification)
  - [Certification Tests](#certification-tests)
  - [Evaluation](#evaluation)

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

**Note: Multi-property Mews customers (vacation rentals, apartments, villas, etc.) are often set up in Mews as spaces of one Mews property.  E.g. Property is the Mews Customer and Spaces are the rental properties.  These spaces (properties) can be sent to the channel manager as multiple connections, 1 connection token per space (property).**

## Certification Tests

These tests will be done during a 60-minute call with the Mews Integrations team and the channel manager.  

| Test                             | Requirements                         |
| -------------------------        | ------------------------------------ |
| **Receive multi-space, multi-rate availability push** | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped from the [Test Hotel](mews-api.md#test-environment) and the [Update Availability](channel-manager-api.md#update-availability) endpoint is functional. |
| **Receive multi-space, multi-rate rate push** | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped and the [Update Prices](channel-manager-api.md#update-prices) endpoint is functional. | 
| **Receive multi-space, multi-rate restriction push** | 5+ Spaces (3 Rooms, 1 Dorm and 1 Bed) and 2 Rates are mapped and the [Update Restrictions](channel-manager-api.md#update-restrictions) endpoint is functional. | 
| **Incorrect Space code error** | Connection returns an error in [plain response](general-remarks.md#plain-response) that includes the incorrect space code and channel manager support email address. |
| **Unmapped Space code error** | Connection returns an error in [plain response](general-remarks.md#plain-response) that includes the unmapped space code and channel manager support email address. |
| **Incorrect Rate code error** | Connection returns an error in [plain response](general-remarks.md#plain-response) that includes the incorrect rate code and channel manager support email address. |
| **Unmapped Rate code error** | Connection returns an error in [plain response](general-remarks.md#plain-response) that includes the unmapped rate code and channel manager support email address. |
| **Connection inactive or not set up error** | Connection returns an error in [plain response](general-remarks.md#plain-response) that states the channel manager's connection is inactive and includes the channel manager support email address. |
| **Receive product (package) rates - optional** | Package Rate and Breakfast product are mapped from the [Test Hotel](mews-api.md#test-environment). |
| **Product code error - optional** | Connection returns an error in [plain response](general-remarks.md#plain-response) that includes the product code and channel manager support email address |
| **Send a single reservation** | Reservation push must include [test credit card](mews-api.md#test-credit-cards) information in the [`paymentCard`](mews-api.md#payment-card) object, an [`adultCount`](mews-api.md#reservation) of 1 and a [`childCount`](mews-api.md#reservation) of 2. |
| **Modify a single reservation** | Modification push must have the same [`channelManagerId`](mews-api.md#main-body) and Reservation [`code`](mews-api.md#reservation) for each booking and at least 1 change to the [`reservations`](mews-api.md#reservation) collection. |
| **Cancel a single reservation** | Cancellation push must have the same [`channelManagerId`](mews-api.md#main-body) as the original reservation. |
| **Send a reservation with 3 spaces on 2 rate plans** | Reservation push must include [test credit card](mews-api.md#test-credit-cards) information in the [`paymentCard`](mews-api.md#payment-card) object and three bookings in the [`reservations`](mews-api.md#reservation) collection with unique `ratePlanCode` properties. | 
| **Add/remove a space and add/remove dates from the reservation** | Modification push must have the same [`channelManagerId`](mews-api.md#main-body) and Reservation [`code`](mews-api.md#reservation) as the original reservation and changes to 2 of the 3 bookings in the [`reservations`](mews-api.md#reservation) collection. |
| **Cancel the reservation** | Cancellation push must have the same [`channelManagerId`](mews-api.md#main-body) as the original reservation. |
| **Send a reservation with 2+ spaces and 1+ companions** | Reservation push must include [test credit card](mews-api.md#test-credit-cards) information in the [`paymentCard`](mews-api.md#payment-card) object, 2+ bookings in the [`reservations`](mews-api.md#reservation) collection and 1 companion in the `guests` property of a booking. |
| **Add/remove a companion and a booking from the reservation** | Modification push must have the same [`channelManagerId`](mews-api.md#main-body) and Reservation [`code`](mews-api.md#reservation) as the original reservation, the `guests` property removed from a booking, and an entire booking removed from the [`reservations`](mews-api.md#reservation) collection. |
| **Cancel the reservation** | Cancellation push must have the same [`channelManagerId`](mews-api.md#main-body) as the original reservation. | 
| **Receive 1 year of inventory data from Mews** | Connection must be able to handle 52 consecutive pushes in XXX minutes. |

## Evaluation

* If the required tests cannot be completed, Mews will not allow the Channel Manager to advance to the production environment.
  * Once issues are resolved, certification will start over.
* If there are no critical issues discovered during certification, the Channel Manager will be advanced to the production environment.
  * Mews will require unique https production endpoint URLs for the [Channel Manager API](certification.md#channel-manager-api).
  * Mews will issue a live production token for the Channel manager.
  * The Channel Manager will issue documenation of their setup process for approval.
