# Mews API

This part defines the API on the Mews side.

## Content

* [Environments](mews-api.md#environments)
  * [Test Environment](mews-api.md#test-environment)
    * [Test Credit Cards](mews-api.md#test-credit-cards)
  * [Production environment](mews-api.md#production-environment)
* [Operations](mews-api.md#operations)
  * [Get Properties](mews-api.md#get-properties)
  * [Get Configuration](mews-api.md#get-configuration)
  * [Set Inventory](mews-api.md#set-inventory)
  * [Request ARI Update](mews-api.md#request-ari-update)
  * [Process Group](mews-api.md#process-group)
  * [Get Channels](mews-api.md#get-channels)

## Environments

### Test Environment

This environment is meant to be used during implementation of the client applications.

* **Platform Address** - `https://demo.mews.li`
* **Client Token** - will be provided to you by Mews upon request.
* **Connection Token** - will be provided to you by Mews upon request.
* **Reservation Push Endpoint** - unique to the environment and list under [Process Group](mews-api.md#process-group)

The property is based in Czech Republic, it accepts `CZK`, `EUR` and `GBP` currencies \(any of them may be used\).

To sign into the system, use the following credentials:

* **Sign in URL** - `https://demo.mews.li`
* **Email** - `chm-api@mews.li`
* **Password** - `chm-api`

#### Test Credit Cards

The property is configured to accept following test credit cards:

* **Accepted Test Credit Cards:**
  * _Expiration date_: 08/2018 or 10/2020
  * _Card holder name_: any value is accepted
  * _CVV_: any value is accepted
  * _Types and Numbers_:
    * Visa: 4111111111111111
    * MasterCard: 5555444433331111
    * Amex: 370000000000002
    * Diners: 36006666333344
    * Discover: 6445644564456445

### Production Environment

* **Platform Address** - `https://www.mews.li`
* **Client Token** - will be provided to you by Mews upon request \(e.g. `C66EF7B239D24632943D115EDE9CB810-JJ549OU4JF94692C940F6B5A8F9453D`\)
* **Connection Token** - will be provided to you by Mews or property on request or via [Get Properties](mews-api.md#get-properties) API call \(e.g. `NF9R27B239D24632943D115EDE9CFH3-EA00F8FD8294692C940F6B5A8F9453D`\)
* **Reservation Push Endpoint** - unique to the environment and listed under [Process Group](mews-api.md#process-group)


## Operations

### Get Properties

\[`sync`\] This method is used to obtain properties based on the `email` of an employee of the Mews enterprise (hotel, hostel or apartment group). It is required that the `email` is verified to belong to an employee on whose behalf the API call is made. Response returns list of all enterprises, where employee has access in Mews.

#### Request `[PlatformAddress]/api/channelManager/v1/getProperties`

```javascript
{
  "clientToken": "[Channel manager client token]",
  "email": "chm-api@mews.li"
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `email` | `string` | required | Verified email of a Mews employee. |

#### Response

This sample response shows that there are 2 properties where the employee with provided email works. First _Sample Hotel_ property has 3 named connections with the channel manager, second _White House Hotel_ has no connection yet.

```javascript
{
  "success": true,
  "properties": [
    {
      "id": "sample-hostel",
      "name": "Sample Hostel",
      "connections": [
        {
          "token": "[1st connectionToken]",
          "name": "Connection for dorms"
        },
        {
          "token": "[2nd connectionToken]",
          "name": "Connection for beds"
        },
        {
          "token": "[3rd connectionToken]"
        }
      ]
    },
    {
      "id": "whh",
      "name": "White House Hotel"
    }
  ]
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `properties` | [`Property Info`](mews-api.md#property-info) collection | required | List of all properties to connect. |

#### Property Info

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `name` | `string` | required | Name of the property. |
| `id` | `string` | required | Unique id of the property. |
| `connections` | [`Connection Info`](mews-api.md#connection-info) collection | optional | List of all existing connections. |

#### Connection Info

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `token` | `string` | required | Token of the existing connection. |
| `name` | `string` | optional | Name of the existing connection. |

### Get Configuration

Returns configuration of the enterprise and the client.

#### Request `[PlatformAddress]/api/channelManager/v1/getConfiguration`

\[`sync`\] Returns configuration of a concrete connection

```javascript
{
  "clientToken": "[Channel manager client token]",
  "connectionToken": "[Token of a concrete connection]",
  "extent": {
    "includeUnsynchronizedRates": true
  }
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `extent` | [`Configuration extent`](mews-api.md#configuration-extent) object | optional | Allows to specify certain data to be included in the response. |

#### Configuration Extent

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `includeUnsynchronizedRates` | `bool` | optional | If `true`, unsynchronized [`Rate plan`](mews-api.md#rate-plan)s will be returned as well. Unsynchronized rate plan means that Mews will not push prices for that rate plan, but when a reservation comes with the rate plan code, Mews will link the correct rate plan with the reservation. |

#### Response

This is example of a _successful_ response. In case an error occurred, the response will contain only [`Error`](mews-api.md#error) object with details.

```javascript
{
  "connectionToken": "[Token of the connection]",
  "property": {
    "name": "White House Hotel",
    "description": "A 5* hotel with a White House view.",
    "languageCode": "en-US",
    "timeZoneIdentifier": "America/New_York",
    "websiteUrl": "http://www.whh.com",
    "email": "reception@whh.com",
    "telephone": "+1 202-456-1111",
    "spaceCount": 21,
    "address": {
      "addressLine1": "White House Hotel",
      "addressLine2": "Pennsylvania Ave",
      "city": "Washington, DC",
      "country": "US",
      "latitude": 38.8976763,
      "longitude": -77.0365298,
      "region": "DC",
      "zip": "20500"
    },
    "images": [
      {
        "type": 1,
        "url": "http://images2.onionstatic.com/onion/5239/1/16x9/600.jpg"
      }
    ]
  },
  "ratePlans": [
    {
      "cancellationPolicies": [
        {
          "applicability": 2,
          "offset": "-1DT2H0M",
          "penalty": {
            "absolute": {
              "amount": 40,
              "currencyCode": "EUR"
            }
          }
        }
      ],
      "code": "FF",
      "currencyCode": "EUR",
      "name": "Fully Flexible",
      "description": null,
      "paymentType": 3,
      "isSynchronized": true
    },
    {
      "cancellationPolicies": [
        {
          "applicability": 1,
          "penalty": {
            "relative": {
              "value": 1,
              "nights": 2
            }
          }
        }
      ],
      "code": "NR",
      "currencyCode": "EUR",
      "name": "Non-refundable",
      "description": "Pre-paid rate",
      "paymentType": 1,
      "isSynchronized": true
    },
    {
      "code": "ROM",
      "name": "Romance Rate",
      "description": "Romantic weekend away",
      "currencyCode": "EUR",
      "breakfast": false,
      "cancellationPolicies": [],
      "paymentType": 3,
      "isSynchronized": false
    }
  ],
  "spaceCategories": [
    {
      "bedCount": 2,
      "bedType": 5,
      "classification": 9,
      "code": "KD",
      "description": "A room with 1 king or 2 double beds.",
      "images": [
        {
          "type": 2,
          "url": "http://images2.onionstatic.com/onion/5239/1/16x9/600.jpg"
        }
      ],
      "extraBedCount": 1,
      "name": "King Double Room",
      "spaceCount": 6
    },
    {
      "bedCount": 2,
      "bedType": 4,
      "classification": 9,
      "code": "QD",
      "description": "tr",
      "images": [
        {
          "type": 2,
          "url": "http://images2.onionstatic.com/onion/5239/1/16x9/600.jpg"
        }
      ],
      "extraBedCount": 0,
      "name": "Queen Double Rooms",
      "spaceCount": 6
    }
  ],
  "inventoryMappings": [
    {
      "ratePlanCode": "FF",
      "spaceTypeCode": "KD"
    },
    {
      "ratePlanCode": "FF",
      "spaceTypeCode": "QD"
    },
    {
      "ratePlanCode": "NR",
      "spaceTypeCode": "KD"
    },
    {
      "ratePlanCode": "NR",
      "spaceTypeCode": "QD"
    },
    {
      "ratePlanCode": "ROM",
      "spaceTypeCode": "KD"
    }
  ],
  "success": true
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `connectionToken` | `string` | required | Token of the connection. |
| `property` | [`Property`](mews-api.md#property) object | required | Details of the proerty. |
| `ratePlans` | [`RatePlan`](mews-api.md#rate-plan) collection | required | Rate plans of the property. |
| `spaceCategories` | [`Space Type`](mews-api.md#space-type) collection | required | Space types of the property. |
| `inventoryMappings` | [`Inventory Mapping`](mews-api.md#inventory-mapping) collection | required | Defines relations between rate plans and space categories. |

#### Property

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `name` | `string` | required | Name of the property. |
| `description` | `string` | optional | Description of the property. |
| `languageCode` | `string` | required | Language [code](https://msdn.microsoft.com/en-us/library/ee825488) of the default language of the property. All names and descriptions are in this language. |
| `timeZoneIdentifier` | `string` | required | [Identifier](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) of the property time zone. |
| `websiteUrl` | `string` | optional | Website of the property. |
| `email` | `string` | optional | Email contact of the property. |
| `telephone` | `string` | optional | Phone contact of the property. |
| `spaceCount` | `int` | required | Total count of spaces sold/offered by the property. |
| `address` | [`Address`](mews-api.md#address) object | optional | Address of the property. |
| `images` | [`Image`](mews-api.md#image) object | optional | Images of the property that may contain logo or property exterior photos. |

#### Address

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `addressLine1` | `string` | optional | First line of the address. |
| `addressLine2` | `string` | optional | Second line of the address. |
| `city` | `string` | optional | City. |
| `region` | `string` | optional | Region. |
| `zip` | `string` | optional | Zip code. |
| `country` | `string` | optional | [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) - two letter country code or [ISO 3166-1 alpha-3 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) - three letter country code. |
| `latitude` | `decimal` | optional | Latitude - from range \[-90, 90\]. |
| `longitude` | `decimal` | optional | Longitude - from range \[-180, 180\]. |

#### Image

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `type` | `int` | required | [`Image Type`](mews-api.md#image-types) code. |
| `url` | `string` | required | Public URL of the image. |

#### Image types

| Code | Description |
| --- | --- |
| `1` | Logo |
| `2` | Photo |

#### Rate Plan

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Mapping code of the rate plan. |
| `name` | `string` | required | Name of the rate plan. |
| `currencyCode` | `string` | required | 3 letter currency code of the rate plan price. |
| `description` | `string` | optional | Description of the rate plan. |
| `paymentType` | `int` | required | [`Payment Type`](mews-api.md#payment-types) code. |
| `cancellationPolicies` | [`Cancellation Policy`](mews-api.md#cancellation-policy) collection | optional | Cancellation policies of the rate plan. |
| `isSynchronized` | `bool` | required | Determines whether rate plan is synchronized, i.e. that Mews pushes prices for the rate plan. Otherwise, unsynchronized rate plan is used just for mapping correct rate plan for incomming reservations (as well as sychronized rate plan). |

#### Payment types

| Code | Description |  |
| --- | --- | --- |
| `1` | Prepaid | _When guest has already paid to the Channel \(i.e. OTA\)._ |
| `2` | Preauthorized | _When the booking is covered by a guarantee \(preauthorization or a payment card\)._ |
| `3` | OnSite | _When guest will pay on site._ |

#### Cancellation Policy

| Poperty | Type |  | Description |
| --- | --- | --- | --- |
| `applicability` | `int` | required | [`Cancellation Policy Applicability`](mews-api.md#cancellation-policy-applicability) code. |
| `offset` | `string` | optional | Offset specifying a "shift" from the moment given by `applicability` when the cancellation policy starts to apply. Format `"[days]DT[hours]H[minutes]M"` inspired by [ISO 8601 for durations](https://en.wikipedia.org/wiki/ISO_8601). E.g. `"-1DT2H0M"` means "-1 day and 2 hours before `applicability` moment", `"0DT2H0M"` means "2 hours after `applicability` moment". |
| `penalty` | [`Cancellation Penalty`](mews-api.md#cancellation-penalty) object | required | Defines penalty that applies based on the cancellation policy. |

#### Cancellation Policy Applicability

| Code | Description |  |
| --- | --- | -- |
| `1` | Creation | _Cancellation policy applies from the moment the booking is created._ |
| `2` | Start | _Cancellation policy applies from the moment the booking starts \(i.e. time included\)._ |
| `3` | Start Date | _Cancellation policy applies from the 0:00 on the day when the booking starts \(i.e. time is not included\)._ |

#### Cancellation Penalty

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `absolute` | [`Absolute Cancellation Penalty`](mews-api.md#absolute-cancellation-penalty) object | required | Defines absolute fee penalty. |
| `relative` | [`Relative Cancellation Penalty`](mews-api.md#relative-cancellation-penalty) object | required | Defines relative \(i.e. %\) fee penalty. |

#### Absolute Cancellation Penalty

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `amount` | `decimal` | required | Defines the amount of the absolute fee. Sent in `gross` |
| `currencyCode` | `string` | required | 3 letter currency code of the absolute fee. |

#### Relative Cancellation Penalty

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `value` | `decimal` | required | Defines the % value of the relative fee \(e.g `0.3` for "30%"\). |
| `nights` | `decimal` | optional | Determines maximum number of nights included in the relative fee calculation, empty means "all nights". |

#### Space Categories

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Mapping code of the space type. |
| `name` | `string` | required | Name of the space type. |
| `description` | `string` | optional | Description of the space type. |
| `spaceCount` | `int` | required | Number of sold/offered spaces of the type. |
| `bedCount` | `int` | optional | Number of beds of the space type - required if the type describes some room type. |
| `extraBedCount` | `int` | optional | Number of extra beds of the space type. |
| `classification` | `int` | required | [`Space classification`](mews-api.md#space-classifications) code. |
| `bedType` | `int` | optional | [`Bed Type`](mews-api.md#bed-types) - required if the type describes some room type. |
| `images` | [`Image`](mews-api.md#image) object | optional | Images of the space type.  These are always image [`type`](mews-api.md#image-types) 2 because they are photos, not logos. |

#### Space Classifications

| Code | Description |
| --- | --- |
| `1` | Apartment |
| `2` | Bungalow |
| `3` | Chalet |
| `4` | Double Room |
| `5` | Holiday Home |
| `6` | Mobile Home |
| `7` | Quadruple Room |
| `8` | Dormitory Bed |
| `9` | Single Room |
| `10` | Studio |
| `11` | Suite |
| `12` | Tent |
| `13` | Triple Room |
| `14` | Twin Room |
| `15` | Villa |
| `16` | Dormitory |

#### Bed Types

| Code | Description |
| --- | --- |
| `1` | Single bed |
| `2` | Twin bed |
| `3` | Double bed |
| `4` | Queen bed |
| `5` | King bed |
| `6` | Sofa bed |

#### Inventory Mappings

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan |
| `spaceTypeCode` | `string` | required | Mapping code of the space type related to the rate plan. |

### Set Inventory

\[`sync`\] This method can be used to update the rate plan - space type mapping relations in the connection. Always, all mapping relations need to be sent. Mapping relations that are in defined in Mews for the connection, but missing in the call, are deleted; and vice-versa.

#### Request `[PlatformAddress]/api/channelManager/v1/setInventory`

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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `inventoryMappings` | [`Inventory Mapping`](mews-api.md#inventory-mapping) collection | required | Defines all rate plan - space type mapping relations. |

#### Response

[Plain response](general-remarks.md#plain-response) is expected.

### Request ARI update

\[`async`\] This method allows channel manager to request an ARI data update for certain space types and rate plans in addition to the changes automatically sent in the next Delta update. The requested data will be sent by Mews asynchronously via push operations of channel manager API.

#### Request `[PlatformAddress]/api/channelManager/v1/requestAriUpdate`

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

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Token of a concrete connection. |
| `from` | `string` | required | Start of requested update in format `"yyyy-MM-dd"`. |
| `to` | `string` | required | End of requested update in format `"yyyy-MM-dd"` \(included\). |
| `ariType` | `int` collection | optional | [`ARI Types`](mews-api.md#ari-types)~~ to be updated. _Blank means all, empty _~~`[]`~~_ means none._~~ _Currently not supported, always updates all._ |
| `spaceTypeCodes` | `string` collection | optional | ~~Space types to be updated. _Blank means all, empty _~~`[]`~~_ means none._~~ _Currently not supported, always updates all._ |
| `ratePlanCodes` | `string` collection | optional | ~~Rate plans to be updated. _Blank means all, empty _~~`[]`~~_ means none._~~ _Currently not supported, always updates all._. |

#### ARI Types

| Code | Description |
| --- | --- |
| `1` | Availability |
| `2` | Prices |
| `3` | Restrictions |

#### Response

[Plain response](general-remarks.md#plain-response) will determine whether the ARI update was accepted for processing or not.

### Process Group

\[`async`\] This operation allows the channel manager to push a reservation group \(i.e. _booking_\) to Mews. This option allows creations, modifications and \(partial\) cancellations. Mews will process and confirm the booking asynchronously.

#### Request 

* **Demo Environment:** `https://sandbox.pci-proxy.com/v1/push/0426f19b66715a93`
* **Production Environment:** `https://api.pci-proxy.com/v1/push/b2721c9a0351d553`

The example shows a valid group definition with 2 space reservations + cancellation of the 3rd space reservation. First `reservation` definition shows all details, second `reservation` definition shows the minimal required details for creation / modification of a `reservation`. The 3rd `reservation` definition shows the partial cancellation - cancelling the 3rd space reservation.

There are certain rules that need to be followed in order for Mews to process the group correctly:

* It is required to send multiple related reservations in one group as part of one message.
  * The whole group is uniquely identified by `channelManagerId` in Mews and in the channel manager extranet.
  * Each reservation should have a unique code within the group. The same code for the reservation should be provided in any following modification message.
  * Each reservation in the group can have different `start`, `end`, `spaceTypeCode`, `ratePlanCode`.
* Group total cost `group`.`totalAmount` is the sum of all `reservation`.`totalAmount` objects in the group, which is the sum of all night amounts and total amounts of all `extras` of the `reservation`.
  * If for any reason the sum of the `reservation`.`totalAmount` is different, Mews will automatically distribute the missing/additional amount to the nights, so the `group`.`totalAmount` is achieved.
  * A reservation `group` amounts should be either `gross` or `net`. At least one value has to be sent. 
* When **modifying** some reservations from a multi-reservation group, the whole group definition with all other unchanged reservations needs to be sent \(i.e. Mews doesn't process diffs\).
* When **cancelling** a reservation from a multi-reservation group, all remaining reservations need to be present in the group definition as well.
  * There are 2 ways to cancel a reservation from a multi-reservation group.
    * A. If the `reservation`.`state` is set to [Reservation States](mews-api.md#reservation-states).`Cancelled`.
    * B. If the reservation is not included in the group definiton message.
* When **cancelling** a whole group, the `reservations` collection can be empty of all reservations provided as cancelled \(as per case A above\).

#### Successful Request Example

```javascript
{
  "clientToken": "[Channel manager client token]",
  "connectionToken": "[Token of a concrete connection]",
  "channelId": "EXP-123456",
  "channelManagerId": "123456",
  "comments": [
    "Approximate arrival: 16:30.",
    "Guest request a room with ocean view."
  ],
  "currencyCode": "EUR",
  "customer": {
    "address": {
      "addressLine1": "Some street 123",
      "addressLine2": "Some other detail",
      "city": "Some city",
      "country": "US",
      "latitude": 30,
      "longitude": 20,
      "region": "Some region",
      "zip": "123 45"
    },
    "email": "john@smith.com",
    "firstName": "John",
    "lastName": "Smith",
    "nationalityCode": "US",
    "languageCode": "en-US",
    "telephone": "1-3526-88918"
  },
  "channel": {
    "code": 1,
    "name": "Expedia"
  },
  "company": {
    "id": "MEWS",
    "iata": "65893",
    "name": "Mews Systems, s.r.o.",
    "contact": {
      "email": "mews@mews.li",
      "phone": "+420 775 684 983",
      "address": {
        "zip": "110 00",
        "city": "Prague",
        "country": "Czech Republic",
        "addressLine1": "586 Ulice Test",
        "addressLine2": "Patro 2"
      }
    }
  },
  "paymentCard": {
    "cvv": "666",
    "expireDate": "1222",
    "holderName": "John Smith",
    "number": "4111111111111111",
    "type": 1
  },
  "paymentType": 1,
  "reservations": [
    {
      "adultCount": 1,
      "childCount": 0,
      "code": "01",
      "extras": [
        {
          "code": "1",
          "cost": 20,
          "amount": {
            "net": 16.2,
            "gross": 20
          },
          "count": 1,
          "pricing": 3
        }
      ],
      "from": "2020-05-05",
      "guests": [
        {
          "address": {
            "addressLine1": "Some other street 123",
            "addressLine2": "Some other detail",
            "city": "Some other city",
            "country": "US",
            "latitude": 30,
            "longitude": 20,
            "region": "Some other region",
            "zip": "123 45"
          },
          "email": "jane@smith.com",
          "firstName": "Jane",
          "lastName": "Smith",
          "nationalityCode": "US",
          "languageCode": "en-US",
          "telephone": "1-369-81891"
        }
      ],
      "prices": [
        100,
        120
      ],
      "amounts" [
        {
          "net": 81,
          "gross": 100
        },
        {
          "net": 97.2,
          "gross": 120
        },
      ],
      "ratePlanCode": "FF",
      "spaceTypeCode": "SGL",
      "state": 1,
      "to": "2020-05-07",
      "totalCost": 260,
      "totalAmount": {
        "gross": 260,
        "net": 208
      }
    },
    {
      "adultCount": 2,
      "code": "02",
      "from": "2020-05-06",
      "prices": [
        100,
        120,
        120
      ],
      "amounts": [
        {
          "net": 81,
          "gross": 100
        },
        {
          "net": 97.2,
          "gross": 120
        },
        {
          "net": 97.2,
          "gross": 120
        }
      ],
      "ratePlanCode": "NR",
      "spaceTypeCode": "DBL",
      "state": 2,
      "to": "2020-05-09",
      "totalCost": 340,
      "totalAmount": {
        "net": 275.4,
        "gross": 340
      }
    },
    {
      "adultCount": 2,
      "code": "03",
      "from": "2020-05-06",
      "ratePlanCode": "FF",
      "spaceTypeCode": "DBL",
      "state": 3,
      "to": "2020-05-09"
    }
  ],
  "totalCost": 600,
  "totalAmount": {
    "net": 486,
    "gross": 600
  }
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required \(always\) | Client token of the channel manager. |
| `connectionToken` | `string` | required \(always\) | Token of a concrete connection. |
| `channelId` | `string` | required \(always\) | Unique identification of the booking in the Channel \(i.e. OTA\). |
| `channelManagerId` | `string` | required \(always\) | Unique identification of the booking in the channel manager. |
| `currencyCode` | `string` | required \(exc. Cancellation\) | 3 letter code of currency of any price within the booking. |
| ~~`totalCost`~~ | ~~`decimal`~~ | ~~required \(exc. Cancellation\)~~ | ~~Total cost of the whole booking.~~ |
| `totalAmount` | [`Amount`](mews-api.md#amount) object | required \(exc. Cancellation\) | Total amount of the whole booking. |
| `paymentType` | `int` | required \(exc. Cancellation\) | [Payment Type](mews-api.md#payment-types) code - determines whether the booking is prepaid or not. |
| `customer` | [`Customer`](mews-api.md#customer) object | required \(exc. Cancellation\) | Represents the main booker. Does not necessarily mean that the person arrives to the property. |
| `paymentCard` | [`Payment Card`](mews-api.md#payment-card) object | optional | Represents the payment card of the [`Customer`](mews-api.md#customer) to cover for the booking. |
| `channel` | [`Channel`](mews-api.md#channel) object | optional | Represents the Channel \(i.e. Travel Agency\). |
| `reservations` | [`Reservation`](mews-api.md#reservation) collection | optional | Each reservation within the bookings. _Empty \(_`null`_ or _`[]`_\) means whole group will be cancelled._ |
| `comments` | `string` collection | optional | Represents any comment related to the booking. |

#### Customer

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `email` | `string` | _recommended_ | Email. |
| `lastName` | `string` | required | Last name. |
| `firstName` | `string` | optional | First name. |
| `telephone` | `string` | optional | Telephone. |
| `nationalityCode` | `string` | optional | [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) - two letter country code or [ISO 3166-1 alpha-3 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) - three letter country code. |
| `languageCode` | `string` | optional | Language [code](https://msdn.microsoft.com/en-us/library/ee825488) of the communication language of the customer. This language will be used as the default for communication with the customer. |
| `address` | [`Address`](mews-api.md#address) object | optional | Represents address. |

#### Company

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `id` | `string` | optional | Identifier of company. |
| `iata` | `string` | optional | IATA number of travel agency. |
| `name` | `string` | optional | Name of company or travel agency. |
| `contact` | [`Contact`](mews-api.md#contact) object | optional | Company or travel agency contact information. |

#### Contact

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `email` | `string` | _recommended_ | Contact email address. |
| `telephone` | `string` | optional | Contaact telephone number. |
| `address` | [`Address`](mews-api.md#address) object | optional | Contact address. |

#### Payment card

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `type` | `int` | required | [Payment Card Type](mews-api.md#payment-card-types) code. |
| `number` | `string` | required | Payment card number. _Only numbers allowed_ |
| `expireDate` | `string` | required | Expiration date of card in `"MMyy"` format \(e.g `"0220"` for February 2020 expiration\). |
| `cvv` | `string` | optional | The card CVV code. |
| `holderName` | `string` | optional | Card holder name. |

#### Payment Card Types

| Code | Description |
| --- | --- |
| `1` | Visa |
| `2` | Master Card |
| `3` | American Express |
| `4` | Bank Card |
| `5` | Carte Bleu |
| `6` | Carte Blanche |
| `7` | Diners Club |
| `8` | Discover Card |
| `9` | Eurocard |
| `10` | Japanese Credit Bureau |
| `11` | Universal Air Travel |
| `12` | China Unionpay |
| `13` | Maestro |

#### Channel

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `int` | required | [Channel](channels.md#channels) code. |
| `name` | `string` | required | Name of the Channel |

#### Reservation

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required \(always\) | Unique code of the reservation within the whole booking.¹ |
| `spaceTypeCode` | `string` | required \(exc. Cancellation\) | Space type code of the reservation. |
| `ratePlanCode` | `string` | required \(exc. Cancellation\) | Rate type code of the reservation. |
| `from` | `string` | required \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g. `"2017-12-24"` for Christmas Eve\). |
| `to` | `string` | required \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g. `"2017-12-31"` for New Years Eve\). |
| ~~`totalCost`~~ | ~~`decimal`~~ | ~~required \(exc. Cancellation\)~~ | ~~Total cost of the reservation.~~ |
| `totalAmount` | [`Amount`](mews-api.md#amount) object | required \(exc. Cancellation\) | Total amount of the reservation. |
| `adultCount` | `int` | required \(exc. Cancellation\) | Number of adults per the reservation. |
| `childCount` | `int` | optional \(exc. Cancellation\) | Number of children per the reservation. |
| `state` | `int` | optional | [Reservation State](mews-api.md#reservation-states) code of reservation state. _If not provided, Mews will handle the reservation as _`Created`_ or _`Modified`_._ |
| ~~`prices`~~ | ~~`decimal` collection~~ | ~~required \(exc. Cancellation\)~~ | ~~Collection of prices for each night of the reservation.~~ ~~_The count of prices in this collection has to correspond with number of nights in the reservation._ ~~|
| `amounts` | [`Amount`](mews-api.md#amount) collection | required \(exc. Cancellation\) | Collection of amounts for each night of the reservation. _The count of amounts in this collection has to correspond with number of nights in the reservation._ |
| `extras` | [`Extra`](mews-api.md#extra) collection | optional | Collection of extra ordered products for the reservation \(e.g. Breakfast\). _Their total price is included in the _`totalCost`_ of the reservation._ |
| `guests` | [`Customer`](mews-api.md#customer) collection | optional | Collection of guests that will arrive to the property. |

_¹ It is required that the code remains the same within each booking modification message and partial modification message. If it can't be achieved because Channel doesn't provide it, simple generation of "01", "02", ... codes will suffice as long as those codes are generated in same way for each message regarding that one booking._

#### Reservation States

| Code | Description |
| --- | --- |
| `1` | Created |
| `2` | Modified |
| `3` | Cancelled |

#### Extra

* Total cost of the extra product should be sent in `net` or `gross` amounts. Either both `gross` and `net` amounts, or one of them should be sent.

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Mapping code of the extra product. |
| ~~`cost`~~ | ~~`decimal`~~ | ~~required~~ | ~~Total cost of the extra product.~~ |
| `amount` | [`Amount`](mews-api.md#amount) object | required | Total amount of the extra product. |
| `count` | `int` | required | Count of extra products ordered. |
| `pricing` | `int` | required | [`Extra pricing Type`](mews-api.md#extra-pricing-types) code of the extra product pricing. |

#### Amount

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `net` | `decimal` | optional | Price with taxes excluded. |
| `gross` | `decimal` | optional | Price with taxes included. |

#### Extra Pricing Types

| Code | Description |
| --- | --- |
| `1` | Once per reservation |
| `2` | Per person - for each guest of reservation. |
| `3` | Per night - for each night of reservation. |
| `4` | ~~Per person night - for each guest for each night of reservaion.~~ _Not supported yet._ |

#### Response

[Plain response](general-remarks.md#plain-response) will determine whether the booking was accepted for processing or not.

### Get Channels

\[`sync`\] This operation allows the channel manager to obtain all [Channel](channels.md#channels)s with assigned mapping codes.

#### Request `[PlatformAddress]/api/channelManager/v1/getChannels`

```javascript
{
  "clientToken": "[Channel manager client token]"
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `clientToken` | `string` | required \(always\) | Client token of the channel manager. |

#### Response

```javascript
{
  "channels": [
    {
      "code": 1,
      "name": "Booking.com"
    },
    ...
  ]
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `channels` | [`Channel`](mews-api.md#channel) collection | required | All mapped channels. |
