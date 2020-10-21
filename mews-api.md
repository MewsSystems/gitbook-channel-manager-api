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

This environment is meant to be used during building, testing, and certification of the client applications. Test properties are based in the Netherlands and accept `EUR` currency.

* **Platform Address** - `https://demo.mews.li`
* **Reservation Push Endpoint** - unique to the environment and listed under [Process Group](mews-api.md#process-group)
* **Client Token** - will be provided by Mews upon request.
* **Connection Token** - will be provided by Mews upon request.
* **Test Property** - user credentials will be provided by Mews upon request.

#### Test Credit Cards

The property is configured to accept following test credit cards:

* **Accepted Test Credit Cards:**
  * _Expiration date_: 08/2021 or 10/2022
  * _Card holder name_: any value is accepted
  * _CVV_: any value, except for ```000``` is accepted
  * _Types and Numbers_:
    * Visa: 4111111111111111
    * MasterCard: 5555444433331111
    * Amex: 370000000000002
    * Diners: 36006666333344
    * Discover: 6445644564456445

### Production Environment

* **Platform Address** - `https://www.mews.li`
* **Reservation Push Endpoint** - unique to the environment and listed under [Process Group](mews-api.md#process-group)
* **Client Token** - will be provided to you by Mews following certification in the test environment \(e.g. `C66EF7B239D24632943D115EDE9CB810-JJ549OU4JF94692C940F6B5A8F9453D`\)
* **Connection Token** - will be provided to you by Mews or property on request or via [Get Properties](mews-api.md#get-properties) API call \(e.g. `NF9R27B239D24632943D115EDE9CFH3-EA00F8FD8294692C940F6B5A8F9453D`\)


## Operations

### Get Properties

\[`sync`\] This method is used to obtain properties based on the `email` of an employee of the Mews enterprise (hotel, hostel or apartment group). It is required that the `email` is verified to belong to an employee on whose behalf the API call is made. Response returns list of all enterprises, where employee has access in Mews, and the `connectionToken`s of all connections for the requesting `clientToken`.

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
        "includeUnsynchronizedRates": true,
        "includeUnsynchronizedCategories": true,
        "includeProducts": true,
        "includeCompanies": true
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
| `includeUnsynchronizedRates` | `bool` | optional | If `true`, unsynchronized [`Rate plan`](mews-api.md#rate-plan)s will be returned as well. Unsynchronized rate plan means that Mews will not push prices and restrictions for that rate plan, but when a reservation comes with the rate plan code, Mews will link the correct rate plan with the reservation. |
| `includeProducts` | `bool` | optional | If `true`, products mapped to a channel manager rate plan will be returned. Products mapped to a channel manager rate plan means that Mews sends a total price combined of night and product price in [`updatePrices`](https://mews-systems.gitbook.io/channel-manager-api/channel-manager-api#update-prices) request. |
| `includeCompanies` | `bool` | optional | If `true`, mapped company (i.e., Microsoft) and/or travel agency (i.e., Expedia) profiles will be returned. A company profile needs to be mapped with a [`Channel Code`](https://mews-systems.gitbook.io/channel-manager-api/channels). To map a travel agency follow [`this guide`](https://help.mews.com/hc/en-us/articles/360001849358-Set-up-travel-agencies). |
| `includeUnsynchronizedCategories` | `bool` | optional | If `true`, unsynchronized space categories will be returned as well. Unsynchronized space category means that Mews will not push availability for that space category, but when a reservation comes with the space category code, Mews will link the correct space category with the reservation. |


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
        "pricingMode": 0,
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
    "spaceCategories": [
        {
            "code": "DEL",
            "name": "Deluxe Room",
            "isSynchronized": false,
            "description": "Our deluxe rooms with a mountain view.",
            "classification": 9,
            "bedType": 5,
            "bedCount": 2,
            "extraBedCount": 1,
            "spaceCount": 4,
            "images": [
                {
                    "url": "https://cdn.demo.mews.li/Media/Image/78e5d3db-7af6-46b7-96ed-b598c447be19",
                    "type": 2
                }
            ]
        },
        {
            "code": "STA",
            "name": "Standard Room",
            "isSynchronized": true,
            "description": "Standard Room with Shared Facilities.",
            "classification": 9,
            "bedType": 4,
            "bedCount": 2,
            "extraBedCount": 0,
            "spaceCount": 7,
            "images": [
                {
                    "url": "https://cdn.demo.mews.li/Media/Image/bffcb480-32d5-4784-9c71-aec792b3ef89",
                    "type": 2
                }
            ]
        }
    ],
    "ratePlans": [
        {
            "code": "NR",
            "name": "Non Refundable",
            "description": "This is our lowest price available. However full payment is required at the time of booking. ",
            "currencyCode": "EUR",
            "paymentType": 1,
            "isSynchronized": true,
            "rateType": 1,
            "cancellationPolicies": [
                {
                    "offset": "-100DT0H0M",
                    "applicability": 2,
                    "penalty": {
                        "relative": {
                            "value": 1.00000000,
                            "nights": null
                        },
                        "absolute": {
                            "amount": 0.00,
                            "currencyCode": "EUR"
                        }
                    }
                }
            ]
        },
        {
            "code": "FF",
            "name": "Fully Flexible",
            "description": "This rate is the most flexible rate we offer. Bookings can be cancelled up to 48 hours  in advance of your arrival date by 2.30 pm (and 7 days before the arrival dates of the 29th, 30th and 31st of December), without charge. The total price of the reservation will be charged 48 hours before arrival. ",
            "currencyCode": "EUR",
            "paymentType": 3,
            "isSynchronized": false,
            "rateType": 1,
            "cancellationPolicies": [
                {
                    "offset": "-1DT0H0M",
                    "applicability": 2,
                    "penalty": {
                        "relative": {
                            "value": 1.00000000,
                            "nights": null
                        },
                        "absolute": {
                            "amount": 0.00,
                            "currencyCode": "EUR"
                        }
                    }
                }
            ]
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
    "products": [
        {
            "code": "AUR",
            "name": "Aurora watch",
            "description": "",
            "pricing": 3,
            "unitAmount": {
                "currencyCode": "EUR",
                "netValue": 16.53,
                "grossValue": 20.0,
                "taxValues": [
                    {
                        "code": "CZ-S",
                        "value": 3.47
                    }
                ]
            }
        }
    ],
    "productMappings": [
        {
            "ratePlanCode": "NR",
            "productCode": "AUR"
        },
        {
            "ratePlanCode": "FF",
            "productCode": "L"
        }
    ],
    "companies": [
        {
            "id": "",
            "iata": "",
            "name": "Some corporation",
            "email": null,
            "contact": "Some contact",
            "phone": "",
            "addresses": [
                {
                    "addressLine1": "Some street",
                    "addressLine2": "",
                    "city": "Some city",
                    "zip": "111111",
                    "region": null,
                    "country": null,
                    "longitude": null,
                    "latitude": null
                }
            ],
            "channel": {
                "code": 438,
                "name": "Some Channel Name"
            }
        }
    ],
    "travelAgencies": [
        {
            "id": "",
            "iata": "11111111",
            "name": "Expedia",
            "email": null,
            "contact": "Some contact",
            "phone": "",
            "addresses": [
                {
                    "addressLine1": "Some street",
                    "addressLine2": "",
                    "city": "Some city",
                    "zip": "1111111",
                    "region": null,
                    "country": null,
                    "longitude": null,
                    "latitude": null
                }
            ],
            "channel": {
                "code": 2,
                "name": "Expedia"
            }
        }
    ],
    "success": true
}
```

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `connectionToken` | `string` | required | Token of the connection. |
| `property` | [`Property`](mews-api.md#property) object | required | Details of the property. |
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
| `pricingMode` | `int` | required | [`Property pricing environment`](mews-api.md#pricing-mode-types). Determines whether `net` or `gross` prices is sent to a Channel Manger. |
| `address` | [`Address`](mews-api.md#address) object | optional | Address of the property. |
| `images` | [`Image`](mews-api.md#image) object | optional | Images of the property that may contain logo or property exterior photos. |

#### Pricing Mode types

| Code | Description |
| --- | --- |
| `0` | Gross |
| `1` | Net |

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
| `isSynchronized` | `bool` | required | Determines whether rate plan is synchronized, i.e. that Mews pushes prices and restrictions for the rate plan. Otherwise, unsynchronized rate plan is used just for mapping correct rate plan for incoming reservations (as well as sychronized rate plan). |
| `rateType` | `int` | required | Determines whether rate plan is private (available for channel reservations only) or public (bookable via Mews Distributor as well). |

#### Payment types

| Code | Description |  |
| --- | --- | --- |
| `1` | Prepaid | _When guest has already paid to the Channel \(i.e. OTA\)._ |
| `2` | Preauthorized | _When the booking is covered by a guarantee \(preauthorization or a payment card\)._ |
| `3` | OnSite | _When guest will pay on site._ |

#### Rate types

| Code | Description |
| --- | --- |
| `0` | Private |
| `1` | Public |

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
| `amount` | `decimal` | required | Defines the amount of the absolute fee. Sent in `gross`. |
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
| `bedCount` | `int` | optional | Number of beds of the space type - required if the type describes some room type. Represents default occupancy.|
| `extraBedCount` | `int` | optional | Number of extra beds of the space type. |
| `classification` | `int` | required | [`Space classification`](mews-api.md#space-classifications) code. |
| `bedType` | `int` | optional | [`Bed Type`](mews-api.md#bed-types) - required if the type describes some room type. |
| `images` | [`Image`](mews-api.md#image) object | optional | Images of the space type.  These are always image [`type`](mews-api.md#image-types) 2 because they are photos, not logos. |

#### Space Classifications

| Code | Description |
| --- | --- |
| `1` | Apartment |
|~~`2`~~ | ~~Bungalow~~  |
|~~`3`~~ | ~~Chalet~~  |
| `4` | Double Room |
|~~`5`~~ |~~Holiday Home~~ |
| ~~`6`~~ | ~~Mobile Home~~ |
| ~~`7`~~ | ~~Quadruple Room~~ |
| `8` | Dormitory Bed |
| `9` | Single Room |
| `10` | Studio |
| `11` | Suite |
| ~~`12`~~ | ~~Tent~~ |
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

#### Products

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Mapping code of the product. |
| `name` | `string` | required | Name of the product. |
| `description` | `string` | optional | Description of the product. |
| `unitAmount` | `[`Amount object`](https://mews-systems.gitbook.io/channel-manager-api/mews-api#amount)` | required | A product cost. |
| `currencyCode` | `string` | required | 3 letter currency code of the product. |
| `netValue` | `decimal` | required | Tax exclusive product cost. |
| `grossValue` | `decimal` | required | Tax inclusive product cost. |
| `cancellationPolicies` | [`Cancellation Policy`](mews-api.md#cancellation-policy) collection | optional | Cancellation policies of the rate plan. |
| `taxValues` | `object` | required | Identifies legal environment specific taxes. |
| `code` | `string` | required | Tax code corresponding to legal environment. |
| `value` | `decimal` | required | Tax amount. |
| `pricing` | `string` | required | Identified in [`pricing type`](https://mews-systems.gitbook.io/channel-manager-api/mews-api#extra-pricing-types). |

#### Product Mapping

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `productCode` | `string` | required | Mapping code of the product related to the rate plan. |

#### Companies

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `id` | `string` | optional | ~~Company identifier.~~ *Not supported yet.*  |
| `iata` | `string` | optional | Related to Travel Agencies only. |
| `name` | `string` | required | Company name. |
| `contact` | `string` | optional | Company contact. |
| `phone` | `string` | optional | Company phone number. |
| `addresses` | `[`Address`](https://mews-systems.gitbook.io/channel-manager-api/mews-api#address) object` | optional | Company address. |
| `channel` | ` [`Channel`] (https://mews-systems.gitbook.io/channel-manager-api/mews-api#channel)` | optional | Mapping code of the company. |

#### Travel Agencies

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `id` | `string` | optional | ~~Company identifier.~~ *Not supported yet.*  |
| `iata` | `string` | optional | IATA code. |
| `name` | `string` | required | Travel Agency name. |
| `contact` | `string` | optional | Travel Agency contact. |
| `phone` | `string` | optional | Travel Agency phone number. |
| `addresses` | `[`Address`](https://mews-systems.gitbook.io/channel-manager-api/mews-api#address) object` | optional | Travel Agency address. |
| `channel` | ` [`Channel`] (https://mews-systems.gitbook.io/channel-manager-api/mews-api#channel)` | optional | Mapping code of the company. |


### Set Inventory

\[`sync`\] This method can be used to **update** the rate plan - space type mapping relations in the connection.
* All mapping relations need to be sent.
* Mapping relations that are defined in Mews for the connection, but missing in the call, are deleted.
* It is possible to create a new rate-space category combination for the rates and space categories that are already in Mews.
* It is impossible to add a new rate or space category.

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

\[`async`\] This method allows channel manager to request an ARI data update for certain space types and rate plans in addition to the changes automatically sent in the next [delta](channel-manager-api.md#delta-inventory-update-mode) update. The requested data will be sent by Mews asynchronously via push operations of channel manager API in the next delta update.

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
| `ariType` | `int` collection | optional | [`ARI Types`](mews-api.md#ari-types) ~~to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._ |
| `spaceTypeCodes` | `string` collection | optional | ~~Space types to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._ |
| `ratePlanCodes` | `string` collection | optional | ~~Rate plans to be updated. _Blank means all, empty _`[]`_ means none._~~ _Currently not supported, always requests updates for all._. |

#### ARI Types

| Code | Description |
| --- | --- |
| `1` | Availability |
| `2` | Prices |
| `3` | Restrictions |

#### Response

[Plain response](general-remarks.md#plain-response) will determine whether the ARI update was accepted for processing or not.

### Process Group

\[`async`\] This operation allows the channel manager to push a reservation group \(i.e. _booking_\) to Mews. This option allows creations, modifications, and partial or complete cancellations. Mews will process and confirm the booking asynchronously.

#### Request 

* **Demo Environment:** `https://sandbox.pci-proxy.com/v1/push/0426f19b66715a93`
* **Production Environment:** `https://api.pci-proxy.com/v1/push/b2721c9a0351d553`

The example shows a valid group definition with 2 space reservations + cancellation of the 3rd space reservation. First `reservation` definition shows all details, second `reservation` definition shows the minimal required details for creation / modification of a `reservation`. The 3rd `reservation` definition shows the partial cancellation - cancelling the 3rd space reservation.

There are certain rules that need to be followed in order for Mews to process the group correctly:

* It is required to send multiple related reservations in one group as part of one message.
  * The whole group is uniquely identified by `channelManagerId` in Mews and in the channel manager extranet.
  * Each reservation should have a unique code within the group. The same code for the reservation should be provided in any following modification message.
  * Each reservation in the group can have different `start`, `end`, `spaceTypeCode`, `ratePlanCode`.
* Group total cost `group.totalAmount` is the sum of all `reservation.totalAmount` objects in the group, which is the sum of all night amounts and total amounts of all `extras` of the `reservation`.
  * If for any reason the sum of the `reservation.totalAmount` objects is different, Mews will automatically distribute the missing/additional amount to the nights, so the `group.totalAmount` is achieved.
  * Reservation `group` amounts should be either `gross` or `net`. Either both `gross` and `net` amounts, or one of them should be sent.
* When **modifying** some reservations from a multi-reservation group, the whole group definition with all other unchanged reservations needs to be sent \(i.e. Mews doesn't process diffs\).
* When **cancelling** a reservation from a multi-reservation group, all remaining reservations need to be present in the group definition as well.
  * There are 2 ways to cancel a reservation from a multi-reservation group.
    * A. If the `reservation.state` is set to [Reservation States](mews-api.md#reservation-states).`Cancelled`.
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
                    "count": 1,                    
                    "amount": {
                        "net": 16.2,
                        "gross": 20
                    },
                    "from": "2021-05-06",
                    "to": "2021-05-07",
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
                    "telephone": "1-369-81891",
                    "loyaltyCode": "PG60972345"
                }
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
            ],
            "ratePlanCode": "FF",
            "spaceTypeCode": "SGL",
            "state": 1,
            "to": "2020-05-07",
            "totalAmount": {
                "gross": 260,
                "net": 208
            }
        },
        {
            "adultCount": 2,
            "code": "02",
            "from": "2020-05-06",
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
| `channelId` | `string` | required \(always\) | Unique identification of the booking in the channel \(i.e. OTA\). |
| `channelManagerId` | `string` | required \(always\) | Unique identification of the booking in the channel manager. |
| `currencyCode` | `string` | required \(exc. Cancellation\) | 3 letter code of currency of all prices within the booking. |
| `totalAmount` | [`Amount`](mews-api.md#amount) object | required \(exc. Cancellation\) | Total amount of the whole booking. |
| `paymentType` | `int` | required \(exc. Cancellation\) | [Payment Type](mews-api.md#payment-types) code - determines whether the booking is prepaid or not. |
| `customer` | [`Customer`](mews-api.md#customer) object | required \(exc. Cancellation\) | Represents the main booker. Does not necessarily mean that the person arrives to the property. |
| `paymentCard` | [`Payment Card`](mews-api.md#payment-card) object | optional | Represents the payment card of the [`Customer`](mews-api.md#customer) to cover for the booking. |
| `channel` | [`Channel`](mews-api.md#channel) object | optional | Represents the channel \(i.e. Travel Agency\). |
| `reservations` | [`Reservation`](mews-api.md#reservation) collection | optional | Each reservation within the booking. _Empty \(_`null`_ or _`[]`_\) means whole group will be cancelled._ |
| `comments` | `string` collection | optional | Represents any comments related to the booking. |

#### Customer

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `email` | `string` | _recommended_ | Email. |
| `lastName` | `string` | required | Last name. |
| `firstName` | `string` | optional | First name. |
| `telephone` | `string` | optional | Telephone. |
| `loyaltyCode` | `string` | optional | Loyalty code of the customer. |
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
| `number` | `string` | required | Payment card number. _Only numbers allowed._ |
| `expireDate` | `string` | required | Expiration date of card in `"MMyy"` format \(e.g `"0222"` for February 2022 expiration\). |
| `cvv` | `string` | optional | Card CVV code. The value cannot be ```000```.|
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
| `name` | `string` | required | Name of the Channel. |

#### Reservation

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required \(always\) | Unique code of the reservation within the whole booking. Any value but `_` accepted. No characters limit. |
| `spaceTypeCode` | `string` | required \(exc. Cancellation\) | Space type code of the reservation. |
| `ratePlanCode` | `string` | required \(exc. Cancellation\) | Rate type code of the reservation. |
| `from` | `string` | required \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | required \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g. `"2021-12-31"` for New Year's Eve\). |
| `totalAmount` | [`Amount`](mews-api.md#amount) object | required \(exc. Cancellation\) | Total amount of the reservation. |
| `adultCount` | `int` | required \(exc. Cancellation\) | Number of adults in the reservation. |
| `childCount` | `int` | optional \(exc. Cancellation\) | Number of children in the reservation. |
| `state` | `int` | optional | [Reservation State](mews-api.md#reservation-states) code of reservation state. _If not provided, Mews will handle the reservation as `Created` or `Modified`._ |
| `amounts` | [`Amount`](mews-api.md#amount) collection | required \(exc. Cancellation\) | Collection of amounts for each night of the reservation. _The count of amounts in this collection has to correspond with number of nights in the reservation._ |
| `extras` | [`Extra`](mews-api.md#extra) collection | optional | Collection of extra ordered products for the reservation \(e.g. Breakfast\). _Their total amount is included in the _`totalAmount`_ of the reservation._ |
| `guests` | [`Customer`](mews-api.md#customer) collection | optional | Collection of guests that will arrive to the property. |

_¹ It is required that the code remains the same within each booking modification message and partial modification message. If it can't be achieved because Channel doesn't provide it, simple generation of "01", "02", ... codes will suffice as long as those codes are generated in same way for each message regarding that one booking._

#### Reservation States

| Code | Description |
| --- | --- |
| `1` | Created |
| `2` | Modified |
| `3` | Cancelled |

#### Extra

* Total cost of the extra product should be sent in `net` or `gross` amounts.
* Either both `gross` and `net` amounts, or one of them should be sent.
* `from` and `to` dates cannot be the same, but it can be any other interval within the reservation dates. (e.g., reservation is from 2025-10-12 to 2025-10-15, extra `from` and `to` cannot be 2025-10-12 to 2025-10-12).

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `code` | `string` | required | Mapping code of the extra product. |
| `amount` | [`Amount`](mews-api.md#amount) object | required | Total amount of the extra product. |
| `count` | `int` | required | Count of extra products ordered. |
| `from` | `string` | optional \(exc. Cancellation\) | Start date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-24"` for Christmas Eve\). |
| `to` | `string` | optional \(exc. Cancellation\) | End date in format `"yyyy-MM-dd"` \(e.g., `"2021-12-31"` for New Year's Eve\). |
| `pricing` | `int` | required | [`Extra pricing Type`](mews-api.md#extra-pricing-types) code of the extra product pricing. |


#### Amount

| Property | Type |  | Description |
| --- | --- | --- | --- |
| `net` | `decimal` | optional | Price with taxes excluded. |
| `gross` | `decimal` | optional | Price with taxes included. |

#### Extra Pricing Types

| Code | Description |
| --- | --- |
| `1` | Once per reservation. |
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
