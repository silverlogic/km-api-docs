---
title: Kidtrol API Reference

search: true
---

# Permissions

Every end point requires the parent to be logged in, except for the create parent endpoint.

# Parent

## The parent object

> An example parent object.

```json
{
    "id": 1,
    "email": "manchild@tsl.io",
    "first_name": "Roger",
    "last_name": "Rabbit"
}
```

A parent object.

Name | Type | Description
---- | ---- | -----------
id | integer
email | string | Used to login
first_name | string
last_name | string

## Create a parent

Create a new parent.  This is basically a registration page.

### HTTP Request

`POST /parents`

### Input

Name | Type | Description
---- | ---- | -----------
email | string | **Required.**
password | string | **Required.**
confirm_password | string | **Required.** Must match `password`.
first_name | string | **Required.**
last_name | string | **Required.**

### Back End Notes

+ A new family is created and the parent is assigned to it.

## Retrieve yourself

Get the currently logged in parent.

### HTTP Request

`GET /parent`

## Update yourself

Edit the currently logged in parent.

### HTTP Request

`PUT /parent`
`PATCH /parent`

### Input

Name | Type | Description
---- | ---- | -----------
first_name | string | **Required.**
last_name | string | **Required.**

## Delete yourself

Delete the currently logged in parent.

### HTTP Request

`DELETE /parent`

### Back End Notes

+ Should this deactivate everything?
+ Should this delete everything?

## Change your password

Change the password of the logged in parent.

### HTTP Request

`POST /parent/change-password`

### Input

> Example input.

```json
{
    "old_password": "old guy smithers",
    "new_password": "new guy tim",
    "confirm_new_password": "new guy tim"
}
```

Name | Type | Description
---- | ---- | -----------
old_password | string | The password before changing it.
new_password | string | The password to change to.
confirm_new_password | string | Must match `new_password`.

# Parent Mobile Device

A parent mobile device is used to send push notifications to the parent.

## The parent mobile device object

> An example parent mobile device object.

```json
{
    "id": 1,
    "name": "Ma Favorite Phone",
    "email": "",
    "phone": "",
    "os": 1,
    "type": "Galaxy S5",
    "push_token": "lja;sdfj8alksjdf"
}
```

Name | Type | Description
---- | ---- | -----------
id | integer
name | string | Human readable name
email | string
phone | string | The device's phone number
os | integer | See [Device OS Enum](#device-os-enum)
type | string | Human readable name for the type of device.
push_token | string | Used to send push notifications to the device.

## Create a parent mobile device

### HTTP Request

`POST /parent-mobile-devices`

### Input

Name | Type | Description
---- | ---- | -----------
name | string | **Required.**
email | string
phone | string
os | integer | **Required.**
type | string | **Required.**
push_token | string

## Retrieve a parent mobile device

### HTTP Request

`GET /parent-mobile-devices/:device_id`

## Update a parent mobile device

### HTTP Request

`PUT /parent-mobile-devices/:device_id`
`PATCH /parent-mobile-devices/:device_id`

## Delete a parent mobile device

### HTTP Request

`DELETE /parent-mobile-devices/:device_id`

## List all the parent mobile devices

All of the parent mobile devices that you own.

### HTTP Request

`GET /parent-mobile-devices`

# Kid

## The kid object

> An example kid object.

```json
{
    "id": 1,
    "email": "ladybug@gmail.com",
    "first_name": "Lady",
    "last_name": "Bug",
    "avatar_url": "https://s3.amazon.com/lady-bug.png",
    "blocked_mobile_feature_list": [2]
}
```

Name | Type | Description
---- | ---- | -----------
id | integer
email | string | Will be used to login later but kids can't login right now.  Unique site wide.
first_name | string
last_name | string
avatar_url | string
blocked_mobile_feature_list | array[integer] | The [Features](#feature-enum) that are blocked no matter what.


## Create a kid

Create a new kid.  Returns a Kid object.

### HTTP Request

`POST /kids`

### Input

> Example input.

```json
{
    "email": "ladybug@gmail.com",
    "first_name": "Lady",
    "last_name": "Bug",
    "avatar": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAACXBIWXMAAAsTAAALEwEAmpwYAAAA B3RJTUUH3wYXDjQBq4FwtwAAAB1pVFh0Q29tbWVudAAAAAAAQ3JlYXRlZCB3aXRoIEdJTVBkLmUH AAAADElEQVQI12P4//8/AAX+Av7czFnnAAAAAElFTkSuQmCC",
    "blocked_mobile_feature_list": [1]
}
```

Name | Type | Description
---- | ---- | -----------
email | string | **Required.**
first_name | string | **Required.**
last_name | string | **Required.**
avatar | string | base64 encoded image
blocked_mobile_feature_list | array[integer]

### Back End Notes

+ New kid is assigned to same family as the parent.

## Retrieve a kid

Retrieve a specific kid.

### HTTP Request

`GET /kids/:kid_id`

## Update a kid

### HTTP Request

`PUT /kids/:kid_id` `PATCH /kids/:kid_id`

## Delete a kid

Delete a kid.

### HTTP Request

`DELETE /kids/:kid_id`

## List all kids

You can see a list of all the kids in your family.

### HTTP Request

`GET /kids`

# Regulated Mobile Device

A regulated mobile device that belongs to a kid.

## The regulated mobile device object

> An example regulated mobile device object.

```json
{
    "id": 1,
    "name": "Mark's iPhone",
    "email": "",
    "phone": "",
    "schedule_list": [1, 2, 5],
    "kid": 8,
    "os": 1,
    "type": "iPhone 5s",
    "push_token": "",
    "is_profile_installed": false
}
```

Name | Type | Description
---- | ---- | -----------
id | integer
name | string | A descriptive name for the device
email | string
phone | string | The phone number
schedule_list | array[integer] | An array of [Schedule](#schedule) ids assigned to this device.
kid | integer | The id of the Kid that owns the device.
os | integer | See [Device OS Enum](#device-os-enum)
type | string | Human readable name for the type of device.
push_token | string | Used to send push notifications to the device.
is_profile_installed | boolean | Whether the profile has been installed on the device using MDM.

## Create a regulated mobile device

### HTTP Request

`POST /regulated-mobile-devices`

### Input

The input formats are the same as the object.

Name | Type | Description
---- | ---- | -----------
name | string | **Required.**
email | string
phone | string
schedule_list | array[integer]
kid | integer | **Required.**
os | integer | **Required.**
type | string | **Required.**
push_token | string

### Back End Notes

+ A device belongs to a family and a kid has a list of devices but it is not being represented that way in the API.
+ The device is joined to the same family as the parent.
+ The kid must be in the same family as the parent.
+ Each schedule must be in the same family as the parent.

## Retrieve a  regulated mobile device

### HTTP Request

`GET /regulated-mobile-devices/:device_id`

## Update a regulated mobile device

### HTTP Request

`PUT /regulated-mobile-devices/:device_id` `PATCH /regulated-mobile-devices/:device_id`

### Input

The input formats are the same as the parent.

Name | Type | Description
---- | ---- | -----------
name | string | **Required.**
email | string
phone | string
schedule_list | array[integer]
kid | integer | **Required.**
os | integer | **Required.**
type | string | **Required.**

## Delete a regulated mobile device

### HTTP Request

`DELETE /regulated-mobile-devices/:device_id`

## List all regulated mobile devices

You can get a list of all of the regulated mobile devices in your family.

### HTTP Request

`GET /regulated-mobile-devices`

## View the master schedule for a regulated mobile device

A flat representation of when and what this device is allowed to do.

### HTTP Request

`GET /devices/:device_id/master-schedule`

### Format

TODO

# Schedule

## The schedule object

> An example schedule object.

```json
{
    "id": 1,
    "name": "School",
    "start_time": "8:00",
    "end_time": "20:00",
    "day_list": [1, 2, 3],
    "feature_list": [1, 3],
    "is_active": true,
    "kid_list": [],
    "device_list": []
}
```

Name | Type | Description
---- | ---- | -----------
id | integer
name | string | Human readable name
start_time | string | The time of day that the schedule starts affecting devices in 24h format. e.g. `14:00`.
end_time | string | The time of day the schedule stops affecting devices in 24h format. e.g. `18:00`.
day_list | array[integer] | The [Days](#day-enum) that the schedule effects.
feature_list | array[integer] | The [Features](#feature-enum) that the schedule effects.
is_active | boolean | If a schedule is inactive it will have no effect on any kids or devices.
kid_list | array[Kid] | The kids that the schedule is applied to.
device_list | array[RegulatedMobileDevice] | The devices that the schedule is applied to.


## Create a schedule

### HTTP Request

`POST /schedules`

### Input

> Example input.

```json
{
    "name": "School Time",
    "start_time": "8:00",
    "end_time": "15:00",
    "day_list": [1, 2],
    "feature_list": [1]
}
```

Name | Type | Description
---- | ---- | -----------
name | string | **Required.**
start_time | string | **Required.**
end_time | string | **Required.**
day_list | array[integer] | **Required.**  What days the schedule is active.
feature_list | array[integer] | **Required.** What features the schedule blocks.
is_active | string | Defaults to true.
kid_list | array[integer] | The ids of the [Kids](#kid) the schedule will effect.
device_list | array[integer] | The ids of the [Devices](#regulated-mobile-device) the schedule will effect.

### Back End Notes

+ The schedule model does not have start and end times. That is on the schedule item.
  For now the API will transparantly create both a schedule and a  schedule item and
  treat the whole thing as a schedule.
+ Schedules can be blocking or unblocking but for now they are always blocking.
+ end time must be after start time

## Retrieve a schedule

### HTTP Request

`GET /schedules/:schedule_id`

## Update a schedule

### HTTP Request

`PUT /schedules/:schedule_id`
`PATCH /schedules/:schedule_id`

## Delete a schedule

### HTTP Request

`DELETE /schedules/:schedule_id`

## List all schedules

You can get a list of all of the schedules that belong to your family.

### HTTP Request

`GET /schedules`


# Override

An override is used to temporarily override all active schedules to either block or unblock kid(s) or device(s).

## The override object

> An example override object.

```json
{
    "id": 1,
    "name": "You've been a bad boy.",
    "start_datetime": "2015-04-05T14:30",
    "end_datetime": "2015-04-05-16:30",
    "block": true,
    "kid_list": [],
    "device_list": []
}
```

Name | Type | Description
---- | ---- | -----------
id | integer
name | string
start_datetime | string | When the override will start taking effect.
end_datetime | string | When the override will stop taking effect.
block | boolean | Whether the override blocks or unblocks features.
kid_list | array[Kid] | Which kids  the schedule override effects.
device_list | array[RegulatedMobileDevice] | Which devices the schedule override effects.

### Back End Notes

+ An override can affect specific features but for now an override affects all features.  The back end will have to set that value.

## Create an override

### HTTP Request

`POST /overrides`

## Retrieve an override

### HTTP Request

`POST /overrides/:override_id`

## Update an override

### HTTP Request

`PUT /overrides/:override_id`
`PATCH /overrides/:override_id`

## Delete an override

### HTTP Request

`DELETE /overrides/:override_id`

## List all overrides

### HTTP Request

`GET /overrides`

# Enums

## Day Enum

Id | Description
--- | -----------
0 | Monday
1 | Tuesday
2 | Wednesday
3 | Thursday
4 | Friday
5 | Saturday
6 | Sunday

## Feature Enum

Id | Description
--- | -----------
0 | Camera
1 | Third Party Apps
2 | Safari
3 | Itunes
4 | Install Apps
5 | In App Purchases

## Device OS Enum
Id | Description
--- | -----------
0 | IOS
1 | Android
