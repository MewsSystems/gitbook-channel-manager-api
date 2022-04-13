# Mews API Operations: Configuration

## Get Properties

This operation is used to get the list of available properties and their connection details, based on your `Client Token` and an employee email address.
A valid email address must be supplied which corresponds to an employee of the enterprise to which the properties belong.
The system will verify the email address and return the list of properties and connections (including `Connection Tokens`) for which the owner of the email address has access.

### Request

`[PlatformAddress]/api/channelManager/v1/getProperties`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "email": "chm-api@mews.li"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `email` | `string` | required | Verified email of an enterprise employee held on the Mews system. |

### Response

This sample response shows that the owner of the email address has access to two properties: _Sample Hostel_ and _White House Hotel_.
_Sample Hostel_ has two connections to this channel manager, whilst _White House Hotel_ has no connections.

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

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `properties` | [`Property Info`](#property-info) collection | required | List of available properties. |

#### Property Info

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `name` | `string` | required | Name of the property. |
| `id` | `string` | required | Unique ID of the property. |
| `connections` | [`Connection Info`](#connection-info) collection | optional | List of supported connections. |

#### Connection Info

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `token` | `string` | required | Connection Token for the connection. |
| `name` | `string` | optional | Name of the connection. |

## Get Configuration

This operation returns the configuration of the given property connection.

### Request

`[PlatformAddress]/api/channelManager/v1/getConfiguration`

```javascript
{
    "clientToken": "[Channel manager client token]",
    "connectionToken": "[Token of a concrete connection]",
    "extent": {
        "includeUnsynchronizedRates": true,
        "includeUnsynchronizedCategories": true,
        "includeProducts": true,
        "includeCompanies": true,
        "includeAgeCategories": true
    }
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required | Client token of the channel manager. |
| `connectionToken` | `string` | required | Connection token of a property connection. |
| `extent` | [`Configuration extent`](#configuration-extent) object | optional | Specifies what to include in the return data. |

#### Configuration Extent

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `includeUnsynchronizedRates` | `bool` | optional | If `true`, unsynchronized [`Rate plans`](#rate-plan) will be returned as well. Unsynchronized rate plan means that Mews will not push prices and restrictions for that rate plan, but when a reservation comes with the rate plan code, Mews will link the correct rate plan with the reservation. |
| `includeProducts` | `bool` | optional | If `true`, products mapped to a channel manager rate plan will be returned. Products mapped to a channel manager rate plan means that Mews sends a total price combining nightly price and product price in [`Update Prices`](../channel-manager-operations/inventory.md#update-prices) requests. |
| `includeCompanies` | `bool` | optional | If `true`, mapped profiles for companies (e.g. Microsoft) and travel agencies (e.g. Expedia) will be returned. A company profile needs to be mapped with a [`Channel Code`](../channels/README.md). To map a travel agency, follow the guide [Setting up travel agencies](https://help.mews.com/s/article/set-up-travel-agencies?language=en_US). |
| `includeUnsynchronizedCategories` | `bool` | optional | If `true`, unsynchronized space categories will be returned as well. Unsynchronized space category means that Mews will not push availability for that space category, but when a reservation comes with the space category code, Mews will link the correct space category with the reservation. |
| `includeAgeCategories` | `bool` | optional | If `true`, age categories mapped to a channel manager integration will be returned. |

### Response

This is an example of a _successful_ response. In case an error occurred, the response will contain only the [`Error`](../guidelines/responses.md#error) object.

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
                    "url": "https://cdn.mews-demo.com/Media/Image/78e5d3db-7af6-46b7-96ed-b598c447be19",
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
                    "url": "https://cdn.mews-demo.com/Media/Image/bffcb480-32d5-4784-9c71-aec792b3ef89",
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
    "ageCategories": [
        {
            "code": "10",
            "name": "Adult",
            "minimalAge": null,
            "maximalAge": null
        },
        {
            "code": "8",
            "name": "Child",
            "minimalAge": 0,
            "maximalAge": 18
        }
    ],
    "success": true
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `connectionToken` | `string` | required | Token of the connection. |
| `property` | [`Property`](#property) object | required | Details of the property. |
| `ratePlans` | [`RatePlan`](#rate-plan) collection | required | Rate plans of the property. |
| `spaceCategories` | [`Space Categories`](#space-categories) collection | required | Space categories (space types) of the property. |
| `inventoryMappings` | [`Inventory Mappings`](#inventory-mappings) collection | required | Defines relations between rate plans and space categories. |
| `ageCategories` | [`Age categories`](#age-categories) collection | required | Age categories of the property. |

#### Property

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `name` | `string` | required | Name of the property. |
| `description` | `string` | optional | Description of the property. |
| `languageCode` | `string` | required | [Language code](https://msdn.microsoft.com/en-us/library/ee825488) of the default language of the property. All names and descriptions are in this language. |
| `timeZoneIdentifier` | `string` | required | Property [time zone identifier](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). |
| `websiteUrl` | `string` | optional | Website of the property. |
| `email` | `string` | optional | Email contact of the property. |
| `telephone` | `string` | optional | Phone contact of the property. |
| `spaceCount` | `int` | required | Total count of spaces sold/offered by the property. |
| `pricingMode` | `int` | required | [`Pricing Mode type`](#pricing-mode-types). Determines whether `net` or `gross` prices are sent to the channel manger. |
| `address` | [`Address`](#address) object | optional | Address of the property. |
| `images` | [`Image`](#image) object | optional | Images associated with the property, e.g. brand logos and exterior photographs. |

#### Pricing Mode types

| Code | Description |
| :-- | :-- |
| `0` | Gross |
| `1` | Net |

#### Address

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `addressLine1` | `string` | optional | First line of the address. |
| `addressLine2` | `string` | optional | Second line of the address. |
| `city` | `string` | optional | City. |
| `region` | `string` | optional | Region. |
| `zip` | `string` | optional | Zip code. |
| `country` | `string` | optional | [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) - two letter country code or [ISO 3166-1 alpha-3 country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) - three letter country code. |
| `latitude` | `decimal` | optional | Latitude - from range \[-90, 90\]. |
| `longitude` | `decimal` | optional | Longitude - from range \[-180, 180\]. |

#### Image

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `type` | `int` | required | [`Image type`](#image-types) code. |
| `url` | `string` | required | Public URL of the image. |

#### Image types

| Code | Description |
| :-- | :-- |
| `1` | Logo |
| `2` | Photo |

#### Rate Plan

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of the rate plan. |
| `name` | `string` | required | Name of the rate plan. |
| `currencyCode` | `string` | required | Three-letter currency code of the rate plan price. |
| `description` | `string` | optional | Description of the rate plan. |
| `paymentType` | `int` | required | [`Payment type`](#payment-types) code. |
| `cancellationPolicies` | [`Cancellation Policy`](#cancellation-policy) collection | optional | Cancellation policies of the rate plan. |
| `isSynchronized` | `bool` | required | Determines whether rate plan is synchronized, i.e. that Mews pushes prices and restrictions for the rate plan. Otherwise, unsynchronized rate plan is used just for mapping correct rate plan for incoming reservations (as well as sychronized rate plan). |
| `rateType` | `int` | required | Determines whether rate plan is private (available for channel reservations only) or public (bookable via Mews Distributor as well). |

#### Payment types

| Code | Description | Notes |
| :-- | :-- | :-- |
| `1` | Prepaid | _When guest has already paid to the Channel \(i.e. OTA\)._ |
| `2` | Preauthorized | _When the booking is covered by a guarantee \(preauthorization or a payment card\)._ |
| `3` | OnSite | _When guest will pay on site._ |

#### Rate types

| Code | Description |
| :-- | :-- |
| `0` | Private |
| `1` | Public |

#### Cancellation Policy

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `applicability` | `int` | required | [`Cancellation Policy Applicability`](#cancellation-policy-applicability) code. |
| `offset` | `string` | optional | Offset specifying a "shift" from the moment given by `applicability` when the cancellation policy starts to apply. Format `"[days]DT[hours]H[minutes]M"` inspired by [ISO 8601 for durations](https://en.wikipedia.org/wiki/ISO_8601). E.g. `"-1DT2H0M"` means "-1 day and 2 hours before `applicability` moment", `"0DT2H0M"` means "2 hours after `applicability` moment". |
| `penalty` | [`Cancellation Penalty`](#cancellation-penalty) object | required | Defines penalty that applies based on the cancellation policy. |

#### Cancellation Policy Applicability

| Code | Description | Notes |
| :-- | :-- | :-- |
| `1` | Creation | _Cancellation policy applies from the moment the booking is created._ |
| `2` | Start | _Cancellation policy applies from the moment the booking starts \(i.e. time included\)._ |
| `3` | Start Date | _Cancellation policy applies from the 0:00 on the day when the booking starts \(i.e. time is not included\)._ |

#### Cancellation Penalty

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `absolute` | [`Absolute Cancellation Penalty`](#absolute-cancellation-penalty) object | required | Defines absolute fee penalty. |
| `relative` | [`Relative Cancellation Penalty`](#relative-cancellation-penalty) object | required | Defines relative \(i.e. %\) fee penalty. |

#### Absolute Cancellation Penalty

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `amount` | `decimal` | required | Defines the amount of the absolute fee. Sent in `gross`. |
| `currencyCode` | `string` | required | 3 letter currency code of the absolute fee. |

#### Relative Cancellation Penalty

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `value` | `decimal` | required | Defines the % value of the relative fee \(e.g `0.3` for "30%"\). |
| `nights` | `decimal` | optional | Determines maximum number of nights included in the relative fee calculation, empty means "all nights". |

#### Space Categories

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of the space type. |
| `name` | `string` | required | Name of the space type. |
| `description` | `string` | optional | Description of the space type. |
| `spaceCount` | `int` | required | Number of sold/offered spaces of the type. |
| `bedCount` | `int` | optional | Number of beds of the space type - required if the type describes some room type. Represents default occupancy.|
| `extraBedCount` | `int` | optional | Number of extra beds of the space type. |
| `classification` | `int` | required | [`Space classification`](#space-classifications) code. |
| `bedType` | `int` | optional | [`Bed Type`](#bed-types) - required if the type describes some room type. |
| `images` | [`Image`](#image) object | optional | Images of the space type.  These are always image [`type`](mews-api.md#image-types) 2 because they are photos, not logos. |

#### Space Classifications

| Code | Description |
| :-- | :-- |
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
| `17` | Site |
| `18` | Office |
| `19` | MeetingRoom |
| `20` | ParkingSpot |
| `21` | Desk |
| `22` | TeamArea |

#### Bed Types

| Code | Description |
| :-- | :-- |
| `1` | Single bed |
| `2` | Twin bed |
| `3` | Double bed |
| `4` | Queen bed |
| `5` | King bed |
| `6` | Sofa bed |

#### Inventory Mappings

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan |
| `spaceTypeCode` | `string` | required | Mapping code of the space type related to the rate plan. |

#### Products

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of the product. |
| `name` | `string` | required | Name of the product. |
| `description` | `string` | optional | Description of the product. |
| `unitAmount` | [`Amount`](reservations.md#amount) object | required | A product cost. |
| `currencyCode` | `string` | required | 3 letter currency code of the product. |
| `netValue` | `decimal` | required | Tax exclusive product cost. |
| `grossValue` | `decimal` | required | Tax inclusive product cost. |
| `cancellationPolicies` | [`Cancellation Policy`](#cancellation-policy) collection | optional | Cancellation policies of the rate plan. |
| `taxValues` | `object` | required | Identifies legal environment specific taxes. |
| `code` | `string` | required | Tax code corresponding to legal environment. |
| `value` | `decimal` | required | Tax amount. |
| `pricing` | `string` | required | Identified in [`pricing types`](../mews-operations/reservations.md#extra-pricing-types). |

#### Product Mapping

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `ratePlanCode` | `string` | required | Mapping code of the rate plan. |
| `productCode` | `string` | required | Mapping code of the product related to the rate plan. |

#### Companies

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `id` | `string` | optional | ~~Company identifier.~~ *Not supported yet.*  |
| `iata` | `string` | optional | Related to Travel Agencies only. |
| `name` | `string` | required | Company name. |
| `contact` | `string` | optional | Company contact. |
| `phone` | `string` | optional | Company phone number. |
| `addresses` | [`Address`](#address) object | optional | Company address. |
| `channel` | [`Channel`](reservations.md#channel) | optional | Mapping code of the company. |

#### Travel Agencies

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `id` | `string` | optional | ~~Company identifier.~~ *Not supported yet.*  |
| `iata` | `string` | optional | IATA code. |
| `name` | `string` | required | Travel Agency name. |
| `contact` | `string` | optional | Travel Agency contact. |
| `phone` | `string` | optional | Travel Agency phone number. |
| `addresses` | [`Address`](#address) object | optional | Travel Agency address. |
| `channel` | [`Channel`](reservations.md#channel) | optional | Mapping code of the company. |

#### Age categories

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `code` | `string` | required | Mapping code of age category.  |
| `name` | `string` | required | Display name. |
| `minimalAge` | `int` | optional | Minimal age for the age category. |
| `maximalAge` | `int` | optional | Maximal age for the age category. |

## Get Channels

This operation allows the channel manager to obtain all [Channels](../channels/README.md#channels) with assigned mapping codes.

### Request

`[PlatformAddress]/api/channelManager/v1/getChannels`

```javascript
{
      "clientToken": "[Channel manager client token]"
}
```

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `clientToken` | `string` | required \(always\) | Client token of the channel manager. |

### Response

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

| Property | Type | Contract | Description |
| :-- | :-- | :-- | :-- |
| `channels` | [`Channel`](../channels/README.md#channels) collection | required | All mapped channels. |
