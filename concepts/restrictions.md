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
| `state` | The restriction type, expressed as a code, e.g. `8` for `Closed to stay`. Multiple codes can apply to the same restriction. |
| `minLos`, `maxLos` | These are minimum and maximum length-of-stay conditions; unlike in the Mews application, these are _not_ expressed as exceptions. |
| `from`, `to` | These are the time period start and end dates in which the restriction applies. |

#### Restriction state

| Code | Description |
| :-- | :-- |
| `1` | Open |
| `2` | Closed (sent in combination with 6, 7 and 8) |
| `3` to `5` | _Not supported_ |
| `6` | Closed to arrival |
| `7` | Closed to departure |
| `8` | Closed to stay |

> **Reverse logic**: It is important to note that in respect of `minLos` and `maxLos`, the logic in the API is the reverse of that in **Mews Operations**.
> In the Mews application, length of stay conditions are exceptions to the main restriction, in other words the specified restriction type or state applies UNLESS the length-of-stay condition is true,
> however in the API they are not exceptions, meaning that the specified restriction type or state applies IF AND WHEN the length-of-stay condition is true.

> **Override logic**: Further, because there is a single API operation to update restrictions, a restriction having a specified scope (rate and space) and applicable time period will override any previous restriction with that same scope and time period.
> For this reason, `state` includes the `Open` state, where no restriction applies. This can be used to remove prior restrictions.

## Restrictions examples

### Use case: Remove all restrictions

When all restrictions are removed, state `1` (Open) is sent. New restrictions always override old restrictions, in this case `Open` overrides any previous state.
 
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

### Use case: Close to stay

State `2` (Closed) is sent in combination with state `8` (Closed to stay).

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

### Use case: Close to arrival

State `2` (Closed) is sent in combination with state `6` (Closed to arrival).

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

### Use case: Close to departure

State `2` (Closed) is sent in combination with state `7` (Closed to departure).

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

### Use case: Close to stay and to arrival 

Both states are applied. State `2` (Closed) is sent in combination with state `8` (Closed to stay) and state `6` (Closed to arrival).

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

### Use case: Close to stay and to departure

Both states are applied. State `2` (Closed) is sent in combination with state `8` (Closed to stay) and state `7` (Closed to departure).

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

### Use case: Close to stay, unless length of stay between 2 and 10 days

State `1` (Open) is sent in conjunction with the specified `minLos` and `maxLos`. If the length of stay is between 2 and 10 days, the restriction state is Open.
If the length of stay is outside the range 2 to 10 days, the state is assumed to be Closed. We do not in this case provide further information on the Closed state.

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

### Use case: Close to arrival, unless length of stay between 2 and 10 days

State `1` (Open) is sent in conjunction with the specified `minLos` and `maxLos`. If the length of stay is between 2 and 10 days, the restriction state is Open.
If the length of stay is outside the range 2 to 10 days, the state is assumed to be Closed. We do not in this case provide further information on the Closed state.

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
### Use case: Close to departure, unless length of stay between 2 and 10 days

State `1` (Open) is sent in conjunction with the specified `minLos` and `maxLos`. If the length of stay is between 2 and 10 days, the restriction state is Open.
If the length of stay is outside the range 2 to 10 days, the state is assumed to be Closed. We do not in this case provide further information on the Closed state.

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

### Use case: Close to arrival if length of stay is 3 to 7 days

State `2` (Closed) is sent in conjunction with state `6` (Closed to arrival), and with a `minLos` of 3 and a `maxLos` of 7. If the length of stay is between 3 and 7 days, the applicable restriction is Closed to arrival.
If the length of stay is outside the range 3 to 7 days, the state is assumed to be Closed. We do not in this case provide further information on the Closed state.
 
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

### Use case: Close to stay if length of stay is 3 to 7 days

State `2` (Closed) is sent in conjunction with state `8` (Closed to stay), and with a `minLos` of 3 and a `maxLos` of 7. If the length of stay is between 3 and 7 days, the applicable restriction is Closed to stay.
If the length of stay is outside the range 3 to 7 days, the state is assumed to be Closed. Therefore, regardless of length of stay, this is treated as Closed to stay.

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

### Use case: Close to departure if length of stay is 3 to 7 days

State `2` (Closed) is sent in conjunction with state `7` (Closed to departure), and with a `minLos` of 3 and a `maxLos` of 7. If the length of stay is between 3 and 7 days, the applicable restriction is Closed to departure.
If the length of stay is outside the range 3 to 7 days, the state is assumed to be Closed. We do not in this case provide further information on the Closed state.

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
