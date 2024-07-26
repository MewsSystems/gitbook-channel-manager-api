# Restrictions

An explanation of what restrictions are and how they are described in the API.

## Contents

* [What are restrictions?](#what-are-restrictions)
* [Restrictions in Mews Operations](#restrictions-in-mews-operations)
* [Restrictions in the API](#restrictions-in-the-api)
* [Restrictions examples](#restrictions-examples)

## What are restrictions?

Restrictions give **Mews** customers more control over their reservations, by preventing guests from making bookings that meet certain conditions. Restrictions can be applied to rates and space types, to control the way guests can book them. For example:

* Use restrictions to implement a promotional rate which applies only to weekend bookings during July and August
* Use restrictions to prevent guests from booking a room in a particular space type category (room type)

## Restrictions in Mews Operations

Restrictions in the Mews application have a number of parameters:

| | |
| :-- | :-- |
| **Type** | This is the nature of the restriction, e.g. closed to stay or just closed to departure. |
| **Rate**, **Rate group** | The rates to which the restriction is linked, e.g. Flex Rate. |
| **Space Categor**, **Space Type** | The spaces to which the restriction is linked, e.g. Deluxe Double, or Parking bays. |
| **Applicable time** | The time period in which the restriction applies, e.g. only on Sundays in July 2025. |
| **Exceptions** | Additional conditions which are exceptions, i.e. the restriction does _not_ apply when this condition is true, e.g. not if the stay exceeds a minimum length, or not if the price is within certain bounds. |

| Parameters | |
| :-- | :-- |
| `Type` | This is the nature of the restriction, e.g. closed to stay or just closed to departure. |
| `Rate`, `Rate group` | The rates to which the restriction is linked, e.g. Flex Rate. |
| `Space Category`, `Space Type` | The spaces to which the restriction is linked, e.g. Deluxe Double, or Parking bays. |
| `Applicable time` | The time period in which the restriction applies, e.g. only on Sundays in July 2025. |
| `Exceptions` | Additional conditions which are exceptions, i.e. the restriction does _not_ apply when this condition is true, e.g. not if the stay exceeds a minimum length, or not if the price is within certain bounds. |

* **Type** – this is the nature of the restriction, e.g. closed to stay or just closed to departure
* **Rate, Rate group** – the rates to which the restriction is linked, e.g. Flex Rate
* **Space Category, Space Type** – the spaces to which the restriction is linked, e.g. Deluxe Double, or Parking bays
* **Applicable time** – the time period in which the restriction applies, e.g. only on Sundays in July 2025
* **Exceptions** – additional conditions which are exceptions, i.e. the restriction does _not_ apply when this condition is true, e.g. not if the stay exceeds a minimum length, or not if the price is within certain bounds

> ### Additional help on restrictions
> * [Understanding restrictions in Mews Operations](https://help.mews.com/s/article/Understanding-restrictions-in-Mews-Operations?language=en_US)
> * [How to create restrictions in Mews Operations](https://help.mews.com/s/article/How-to-create-restrictions-in-Mews-Operations?language=en_US)
> * [Search all Help articles related to restrictions](https://help.mews.com/s/global-search/restrictions?language=en_US)

## Restrictions in the API

In the **Mews Channel Manager API**, the channel manager side must support an endpoint for Mews to push restrictions to the channel manager, this is [Update restrictions](../channel-manager-operations/inventory.md#update-restrictions). Note there is no operation to add or delete restrictions, everything is done through Update restrictions. Thus any existing restrictions with the same scope and time period are overridden by the updated restrictions.

| <div style="width:350px">'How to' use case</div> | API Operations |
| :-- | :-- |
| How to get restrictions updates | [CHM: Update restrictions](../channel-manager-operations/inventory.md#update-restrictions) |

For historical reasons, restrictions are described differently in the API than in **Mews Operations**, so care is needed when translating between the two. The parameters are as follows:

| Parameters | |
| :-- | :-- |
| `ratePlanCode` | The mapped rate code to which the restriction is linked. |
| `spaceTypeCode` | The mapped space code to which the restriction is linked. |
| `state` | The state is the restriction type, using a code number, e.g. `8` for `Closed to stay`. Multiple codes can apply to the same restriction. |
| `minLos`, `maxLos` | These are minimum and maximum length-of-stay conditions; unlike in the Mews application, these are _not_ expressed as exceptions. |
| `from`, `to` | These are the time period start and end dates in which the restriction applies. |

> **Reverse logic**: It is important to note that the in respect of `minLos` and `maxLos`, the logic in the API is the reverse of that in **Mews Operations**.
> In the Mews application, length of stay conditions are exceptions to the main restriction, in other words the specified restriction type or state applies UNLESS the length-of-stay condition is true,
> however in the API they are not exceptions, meaning that the specified restriction type or state applies WHEN the length-of-stay condition is true.

> **Override logic**: Further, because there is a single API operation to update restrictions, a restriction having a specified scope (rate and space) and applicable time period will override any previous restriction with that same scope and time period.
> For this reason, `state` includes the `Open` state, where no restriction applies. This can be used to remove prior restrictions.

## Restrictions examples

### No Restriction (Open)

When all restrictions are removed, state `1` is sent. New restrictions always override old restrictions. State `1` is _not_ sent to remove old restrictions, if they were modified.
 
```javascript
{
    "ratePlanCode": "NR",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-24",
    "to": "2020-10-31"
}
```

### Closed to Stay

State `2` is sent in combination with state `8`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        8
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Stay with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2020-10-01",
    "to": "2020-10-14"
}
```

### Closed to Arrival

State `2` is sent in combination with state `6`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "STA",
    "state": [
        2,
        6
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Arrival with minLos and maxLos

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to arrival.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2019-10-01",
    "to": "2019-10-04"
}
```

### Closed to Departure

State `2` is sent in combination with state `7`.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Departure with minLos and maxLos 

State `1` is sent with specified minLos and/or maxLos. If `minLos` and/or `maxLos` is not met, then the restriction should be treated as Closed to departure.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "JST",
    "state": [
        1
    ],
    "minLos": 2,
    "maxLos": 10,
    "from": "2019-10-01",
    "to": "2019-10-04"
}
```

### Closed to Stay and Closed to Arrival 

Both states should be applied.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8,
        6
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Stay and Closed to Departure

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8,
        7
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Stay with minLos/maxLos and Closed to Arrival

This should be applied as no arrivals are possible and reservation minLos is 3 nights and maxLos is 7 nights.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        6
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Stay and Closed to Arrival with minLos/maxLos

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        8
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Departure and Closed to Arrival with minLos/maxLos

This combination should be treated as Closed to stay.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "DEL",
    "state": [
        2,
        7
    ],
    "minLos": 3,
    "maxLos": 7,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### Closed to Departure and Closed to Stay with minLos/maxLos

This combination should be treated as Closed to departure. Reservations can be made if the length-of-stay requirements are met.

```javascript
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        7
    ],
    "minLos": 2,
    "maxLos": 6,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```

### No minLos nor maxLos

When `minLos` or `maxLos` is not specified, `null` value is sent.

```javascript        
{
    "ratePlanCode": "FF",
    "spaceTypeCode": "4BD",
    "state": [
        2,
        8
    ],
    "minLos": null,
    "maxLos": null,
    "from": "2020-09-30",
    "to": "2020-10-06"
}
```
