---
title: API Reference

toc_footers:
  - <a href='https://justklikkit.com'>Contact Klikkit For A Client ID</a>

search: true
---

# Introduction

> **Postman collection environment**

> The examples given below can also be executed from the example
[postman collection](/collection.json). The collection
assumes that the following environment variables have been set up:

```
{{url}}: https://api.justklikkit.com/v1

{{clientId}}: MY_CLIENT_ID

{{clientSecret}}: MY_CLIENT_SECRET
```

Welcome to the Klikkit service. This service is used for all interactions with
the Klikkit system.

All requests are made via https. You can view example requests in the
dark area to the right.

All requests except for `/oauth/token` must have the `Authorization`
header set to `Bearer MY_TOKEN` where `MY_TOKEN` is the `access_token`
returned by the `/oauth/token` request.

# Activity

## Create Activity

> **To create an activity, use this code:**

```
POST https://api.justklikkit.com/v1/activities
```

> Request body:

```json
{
  "name": "Drink water",
  "productId": "59fb3e3aed922d4718a4f90c",
  "color": "blue",
  "buttons": [
    {
      "hardwareId": "00:25:96:FF:FE:12:34:56",
      "firmwareVersion": "V0024",
      "virtual": false
    },
    {
      "virtual": true
    }
  ],
  "schedules": [
    {
      "startDate": "2017-05-08T10:24:31.142Z",
      "endDate": "2017-08-08T10:24:31.142Z",
      "days": ["monday", "wednesday", "friday"],
      "timesOfDay": [
        {
          "time": "2017-05-08T08:00:00.000Z",
          "remind": true
        },
        {
          "time": "2017-05-08T17:00:00.000Z",
          "remind": false
        }
      ]
    }
  ]  
}
```

> **Success Response:**

> The newly created activity contains the same fields as the request body
fields with the added object ids for all objects.

> **HTTP Status Code:** 200

```json
{
  "id": "59fc2f7438bf4c48ef493588",
  "name": "Drink water",
  "productId": "59fb3e3aed922d4718a4f90c",
  "color": "blue",
  "buttons": [
    {
      "id": "59fc2f6d38bf4c48ef493587",
      "hardwareId": "00:25:96:FF:FE:12:34:56",
      "firmwareVersion": "V0024",
      "virtual": false
    },
    {
      "id": "59fc2f6d38bf4c48ef493586",
      "virtual": true
    }
  ],
  "schedules": [
    {
      "id": "59fb444eb0a73a4790f34a80",
      "startDate": "2017-05-08T10:24:31.142Z",
      "endDate": "2017-08-08T10:24:31.142Z",
      "days": ["monday", "wednesday", "friday"],
      "timesOfDay": [
        {
          "id": "59fb4022e51c3647444c4cd8",
          "time": "2017-05-08T08:00:00.000Z",
          "remind": true
        },
        {
          "id": "59fb3ffb0b0633473fe1ae42",
          "time": "2017-05-08T17:00:00.000Z",
          "remind": false
        }
      ]
    }
  ]  
}
```

An activity is associated with one or more buttons with the same color.
It may contain a product id, and should contain one or more schedules.

### URI

`/activities`

### Method

`POST`

### Parameters

When creating an activity, manufacturer and EAN in product is optional.
All other fields are mandatory. The buttons array should not be empty.
The schedules array may be empty.

 - **name**

The activity `name` should be unique for the current user and should be non-empty.

 - **productId**

The `productId` is optional. If set it must be set to the id of an existing [Product](#product).

 - **color**

The color should be one of the following values:

**Color** |
----------|
**blue** |
**pink** |
**red** |
**orange** |
**green** |

 - **buttons**

The `buttons` parameter consists of an array of buttons. The array should not
be empty. Each button has the following properties:

**Property** | **Description** |
-------------|-----------------|
**virtual** | Must be `false` for physical buttons and `true` for virtual buttons. |
**hardwareId** | This field is mandatory when **virtual** is set to `false`. It must be the MAC address of the button. |
**firmwareVersion** | This field is mandatory when **virtual** is set to `false`. It should be the version of the firmware on the button. |

 - **schedules**

The `schedules` parameter is an array of schedules. Each schedule has the following properties, all of which are mandatory:

**Property** | **Description** |
-------------|-----------------|
**startDate** | This must be set to a [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) date and specify the start time and date of the schedule. |
**endDate** | This must be set to a [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) date and specify the end time and date of the schedule. |
**days** | This must be an array of zero or more lowercase weekdays. Possible values are: `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`. |
**timesOfDay** | If **days** is non-empty length, then this must be non-empty. It must be an array of **TimeOfDay** as described below. |

A **TimeOfDay** instance must have two properties, both of which are mandatory:

**Property** | **Description** |
-------------|-----------------|
**time** | This must be set to a [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) date and specify the time of day when the activity is scheduled. Hour, day and minute (offset by timezone) is applied with the offset to the day given by the **days** array specified above. |
**remind** | If this is `true` then a push notification is sent to all registered [Device]($device)s at the specified time unless a click is registered. |

## Read Activity

> **To get an activity, use this code:**

```
GET https://api.justklikkit.com/v1/activities/:id
```

> Where `:id` is the activity id, e.g. "59fb3e3aed922d4718a4f90c"

> **Success Response:**

> **HTTP Status Code:** 200

```json
{
  "id": "59fc2f7438bf4c48ef493588",
  "name": "Drink water",
  "productId": "59fb3e3aed922d4718a4f90c",
  "color": "blue",
  "buttons": [
    {
      "id": "59fc2f6d38bf4c48ef493587",
      "hardwareId": "00:25:96:FF:FE:12:34:56",
      "firmwareVersion": "V0024",
      "virtual": false
    },
    {
      "id": "59fc2f6d38bf4c48ef493586",
      "virtual": true
    }
  ],
  "schedules": [
    {
      "id": "59fb444eb0a73a4790f34a80",
      "startDate": "2017-05-05T10:24:31.142Z",
      "endDate": "2017-08-08T10:24:31.142Z",
      "days": ["monday", "wednesday", "friday"],
      "timesOfDay": [
        {
          "id": "59fb4022e51c3647444c4cd8",
          "time": "2017-05-08T08:00:00.000Z",
          "remind": true
        },
        {
          "id": "59fb3ffb0b0633473fe1ae42",
          "time": "2017-05-08T17:00:00.000Z",
          "remind": false
        }
      ]
    }
  ]  
}
```

Read the activity with the given id.

`/activities/:id`

Where `:id` is the activity id, e.g. "59fb3e3aed922d4718a4f90c"

### Method

`GET`

### Parameters

 - **:id**

The id of the activity to get.

## Update Activity

> **To update an existing activity, use this code:**

```
PUT https://api.justklikkit.com/v1/activities/:id
```

> Where `:id` is the activity id, e.g. "59fb3e3aed922d4718a4f90c"

> To set just the name and the start date of the schedule, use code like this:

```json
{
  "id": "59fc2f7438bf4c48ef493588",
  "name": "Drink cold water",
  "schedules": [
    {
      "id": "59fb444eb0a73a4790f34a80",
      "startDate": "2017-05-05T10:24:31.142Z",
    }
  ]  
}
```

> The code above would also remove any schedule items not specified in the list of
schedules. To retain a an unchanged schedule in the list of schedules, specify an
item with just an id.

> **Success Response:**

> The updated activity contains all fields of the activity.

> **HTTP Status Code:** 200

```json
{
  "id": "59fc2f7438bf4c48ef493588",
  "name": "Drink water",
  "productId": "59fb3e3aed922d4718a4f90c",
  "color": "blue",
  "buttons": [
    {
      "id": "59fc2f6d38bf4c48ef493587",
      "hardwareId": "00:25:96:FF:FE:12:34:56",
      "firmwareVersion": "V0024",
      "virtual": false
    },
    {
      "id": "59fc2f6d38bf4c48ef493586",
      "virtual": true
    }
  ],
  "schedules": [
    {
      "id": "59fb444eb0a73a4790f34a80",
      "startDate": "2017-05-08T10:24:31.142Z",
      "endDate": "2017-08-08T10:24:31.142Z",
      "days": ["monday", "wednesday", "friday"],
      "timesOfDay": [
        {
          "id": "59fb4022e51c3647444c4cd8",
          "time": "2017-05-08T08:00:00.000Z",
          "remind": true
        },
        {
          "id": "59fb3ffb0b0633473fe1ae42",
          "time": "2017-05-08T17:00:00.000Z",
          "remind": false
        }
      ]
    }
  ]  
}
```

When updating an activity, id is mandatory. All other fields are optional.
Any field which is not included in the request body will remain unchanged.

### URI

`/activities/:id`

Where `:id` is the activity id, e.g. "59fb3e3aed922d4718a4f90c"

### Method

`PUT`

### Parameters

All parameters are the same as for [Create Activity](#create-activity).

All parameters except id are optional. Any omitted parameter will remain
intact in the persisted object.

## Delete Activity

> **To delete an activity, use this code:**

```
DELETE https://api.justklikkit.com/v1/activities/:id
```

> Where `:id` is the activity id, e.g. "59fb3e3aed922d4718a4f90c"

> **Success Response:**

> **HTTP Status Code:** 204

> The response has no body

Delete a given activity.

### URI

`/activities`

### Method

`DELETE`

### Parameters

- **:id**

The id of the activity to delete.

# Authentication

Getting an access token has 3 main use cases:

 - Get client access token
 - Get user access token
 - Exchange a Facebook access token

A client access token is mainly used for authorizing a client capable
of creating user accounts. This case is identified by setting `grant`
to `client_credentials`.

Required parameters for this case are:

Parameter |
---------- |
**client_id** |
**client_secret** |
**grant** |
**scope** |

A user access token is used for authorizing a user which has been created
using the [create user](#create-user) endpoint. This case is identified
by setting `grant` to `resource_owner_credentials`.

Required parameters for this case are:

Parameter |
---------- |
**client_id** |
**client_secret** |
**username** |
**password** |
**grant** |
**scope** |

An exchanged facebook access token is used to authorize a user who has
logged in using [Facebook login](https://developers.facebook.com/docs/facebook-login/).
This case is identified by setting `grant` to `fb_exchange_token`.

Required parameters for this case are:

Parameter |
---------- |
**client_id** |
**client_secret** |
**fb_exchange_token** |
**grant** |
**scope** |

## Get Access Token

> **To authorize a client, use this code:**

```
POST https://api.justklikkit.com/v1/oauth/token
```

> Post body:

```json
{
  "client_id": "59f36188f3e75be0e6a7e831",
  "client_secret": "asei5thaigh0eey8phoh8Xookuchoh0o",
  "grant": "client_credentials",
  "scope": [ "create_user" ]
}
```

> **To authorize a previously registered user:**

```
POST https://api.justklikkit.com/v1/oauth/token
```

> Post body:

```json
{
  "client_id": "59f36188f3e75be0e6a7e831",
  "client_secret": "asei5thaigh0eey8phoh8Xookuchoh0o",
  "username": "joeschmoe",
  "password": "secret",
  "grant": "resource_owner_credentials",
  "scope": [ "read_user", "write_user", "read_press", "write_press",
    "read_schedule", "write_schedule" ]
}
```

> **Success Response:**

> **HTTP Status Code:** 200

```json
{
  "token_type": "Bearer",
  "expires_in": 2595600,
  "access_token": "d6be0db9297547536e122ceabc23ac4a670b078a78bc09aa22092516c77dfcc48730035d946c00a8a22286d9c207e3d5b455b9975b5442b5ff34ef75e5877a85"
}
```

> **To authorize a Facebook user:**

```
POST https://api.justklikkit.com/v1/oauth/token
```

> Post body, where `MY_TOKEN` is set to the `currentAccessToken`
returned by the AccessToken class from the Facebook SDK.

```json
{
  "client_id": "59f36188f3e75be0e6a7e831",
  "client_secret": "asei5thaigh0eey8phoh8Xookuchoh0o",
  "fb_exchange_token": "EAAEZBS9qSCfQBAM4pBHoKIdNDZBgcmXYidacWrtKnZCDmO6kxAaegW8PgFZBksmOCVX3hF7h9g6i4FszW38xiLwRws67u96Y83oKEFzfZCybfi0r0ipwgU6kZAUjVF2NouvFMsTIrPf8PFE1FZBUYEOg0LjkXfQZA9MngkGOvArFAaKa8MOVnpMp",
  "grant": "fb_exchange_token",
  "scope": [ "read_user", "write_user", "read_press", "write_press",
    "read_schedule", "write_schedule" ]
}
```

> **Success Response:**

> Requests with grant of type `fb_exchange_token` will return both the
access_token which is must used with following requests to the klikkit
API and a long lived access token which should be used with following
requests to Facebook APIs.

> **HTTP Status Code:** 200

```json
{
  "klikkit_access_token": {
    "token_type": "Bearer",
    "expires_in": 2595600,
    "access_token": "d6be0db9297547536e122ceabc23ac4a670b078a78bc09aa22092516c77dfcc48730035d946c00a8a22286d9c207e3d5b455b9975b5442b5ff34ef75e5877a85"
  },
  "fb_access_token": {
    "access_token": "EAAEZBS8qSCfQBAM4pBHoKIdNDZBgcmXYidacWrtKnZCDmO6kxAaegW8PgFZBksmOCVX3hF7h9g6i4FszW38xiLwRws67u96Y83oKEFzfZCybfi0r0ipwgU6kZAUjVF2NouvFMsTIrPf8PFE1FZBUYEOg0LjkXfQZA9MngkGOvArFAaKa8MOVnpYp",
    "token_type": "bearer",
    "expires_in": 5180261
  }
}
```

> **Error Response:**

> **HTTP Status Code:** 400 Bad request

> This is returned if one or more of the required parameters are missing.
It will be one of the following responses:

```json
{ "error": "Missing grant" }

{ "error": "Missing scope" }
```

> **HTTP Status Code:** 401 Unauthorized

>  This is returned when the client id or secret does not match a
registered client, or when the requested scope isn't allowed for the
given client. For Facebook exchange tokens it may include the error
returned from Facebook authentication servers. It will be one of the
following responses:

```json
{ "error": "Unauthorized client" }

{ "error": "Invalid scope" }

{
  "error": "Error validating access token",
  "caused_by": {
    "message": "Error validating access token: Session has expired at unix
                time SOME_TIME. The current unix time is SOME_TIME.",
    "type": "OAuthException",
    "code": 190
  }
}

{
  "error": "Error validating access token",
  "caused_by": {
    "message": "Error validating access token: The session is invalid
                because the user logged out.",
    "type": "OAuthException",
    "code": 190
  }
}

{
  "error": "Error validating access token",
  "caused_by": {
    "message": "Error validating access token: User USER_ID has
                not authorized application APP_ID.",
    "type": "OAuthException",
    "code": 190
  }
}
```

Klikkit uses client ids and secrets to allow access to the API. You can
register a new client by contacting [Klikkit](https://justklikkit.com).

All other requests made to Klikkit services require an <code>Authorization</code>
header with the format <code>Bearer ACCESS_TOKEN</code> where the access
token is the token returned by the <code>oauth/token</code> endpoint.

See [Facebook docs](https://developers.facebook.com/docs/facebook-login/)
for information on how to get a Facebook access token when using `fb_exchange_token`.


### URI

`/oauth/token`

### Method

`POST`

### Parameters

Parameters can be posted as either form-data or application/json in the
request body.

 - **grant**

The `grant` parameter can be one of:

Grant | Description
----- | -----------
**client_credentials** | Grant a registered client access
**resource_owner_credentials** | Grant a previously created user access
**fb_exchange_token** | Grant a Facebook user access

 - **client_id**

The `client_id` parameter is the client id provided by Klikkit.

 - **client_secret**

The `client_secret` parameter is the client secret provided by Klikkit.

- **username**

The `username` parameter is solely used with grants of type `resource_owner_credentials`.
It must be set to the username for the user which is going to be authorized.

- **password**

The `password` parameter is solely used with grants of type `resource_owner_credentials`.
It must be set to the password for the user which is going to be authorized.

- **fb_exchange_token**

The `fb_exchange_token` parameter is solely used with grants of type `fb_exchange_token`.
It must be set to the access token given by the Facebook SDK.

- **scope**

The `scope` parameter is an array which must be one or more of:

Scope | Description
----- | -----------
**create_user** | grants access to create users
**read_user** | grants access to read the user information for the authorized user
**write_user** | grants access to update the user information for the authorized user
**read_press** | grants access to read button presses for all registered buttons for the authorized user
**write_press** | grants access to write button presses for all registered buttons for the authorized user
**read_schedule** | grants access to read button pres schedules for all registered buttons for the authorized user
**write_schedule** | grants access to write button pres schedules for all registered buttons for the authorized user

It must not include any scopes which are not granted to the client
identified by the `client_id` parameter.

## Invalidate Access Token

> **To de-authorize a client or a user:**

```
DELETE https://api.justklikkit.com/v1/oauth/token
```

> **Success Response:**

> **HTTP Status Code:** 204

> The response has no body

When succesful, the server will invalidate the access token given in the
`Authorization` request header and not return any response content.

### URI

`/oauth/token`

### Method

`DELETE`

### Parameters

None. This method does not take any parameters. It uses the `Authorization`
request header to identify which access token to invalidate.

<aside class="warning">After making this request, a <a href="#get-access-token">Get Access Token</a>
  request must be made before making any further requests.</aside>

# Product

## Create Product

> **To create a product, use this code:**

```
POST https://api.justklikkit.com/v1/products
```

> Post body:

```json
{
  "name": "Mypre",
  "manufacturer": "Myprotein",
  "ean": "5055534352212"
}
```

> **Success Response:**

> **HTTP Status Code:** 200 - Product was created

```json
{
  "id": "59fb3da06feac047021bf462",
  "name": "Mypre",
  "manufacturer": "Myprotein",
  "ean": "5055534352212"
}
```

> **HTTP Status Code:** 300 - Product was created, but another matching product exists.

> If another product with the same name exists, then a list with the newly created product followed
by any other products which match the created product is returned.

```json
[
  {
    "id": "59fb3da06feac047021bf462",
    "name": "Mypre",
    "manufacturer": "Myprotein",
    "ean": "5055534352212"
  },
  {
    "id": "59fb3a6534f9e646dcae06cc",
    "name": "Mypre",
    "manufacturer": "MyProtein",
    "ean": "5055534352212"
  }
]
```

A product must contain a name which should be unique. It may contain a manufacturer, and an EAN.

### URI

`/products`

### Method

`POST`

### Parameters

 - **name**

The `name` must be non-empty and should be unique.

 - **manufacturer**

The `manufacturer` should be the name of the product manufacturer if available. This parameter is optional.

 - **ean**

If `ean` is set then it should be set to a valid [International Article Number](https://en.wikipedia.org/wiki/International_Article_Number). This parameter is optional.

## Read Product

> **To get a product, use this code:**

```
GET https://api.justklikkit.com/v1/products/:id
```

> Where `:id` is the product id, e.g. "59fb3e3aed922d4718a4f90c"

> **Success Response:**

> **HTTP Status Code:** 200

```json
{
  "id": "59fb3da06feac047021bf462",
  "name": "Mypre",
  "manufacturer": "Myprotein",
  "ean": "5055534352212"
}
```

Read the product with the given id.

`/products/:id`

Where `:id` is the product id, e.g. "59fb3e3aed922d4718a4f90c"

### Method

`GET`

### Parameters

 - **:id**

The id of the product to get.

## Update Product

> **To update an existing product, use this code:**

```
PUT https://api.justklikkit.com/v1/products/:id
```

> Where `:id` is the product id, e.g. "59fb3e3aed922d4718a4f90c"

> To set just the name of the product, use code like this:

```json
{
  "id": "59fc2f7438bf4c48ef493588",
  "name": "Whey Protein",
}
```

> **HTTP Status Code:** 200 - Product was created

```json
{
  "id": "59fb3da06feac047021bf462",
  "name": "Whey Protein",
  "manufacturer": "Myprotein",
  "ean": "5055534352212"
}
```

> **HTTP Status Code:** 300 - Product was created, but another matching product exists.

> If another product with the same name exists, then a list with the newly created product followed
by any other products which match the created product is returned.

```json
[
  {
    "id": "59fb3da06feac047021bf462",
    "name": "Whey Protein",
    "manufacturer": "Myprotein",
    "ean": "5055534352212"
  },
  {
    "id": "59fb3a6534f9e646dcae06cc",
    "name": "Whey Protein",
    "manufacturer": "MyProtein",
    "ean": "5055534352212"
  }
]
```

When updating a product, id is mandatory. All other fields are optional.
Any field which is not included in the request body will remain unchanged.

### URI

`/products/:id`

Where `:id` is the product id, e.g. "59fb3e3aed922d4718a4f90c"

### Method

`PUT`

### Parameters

All parameters are the same as for [Create Product](#create-product).

All parameters except id are optional. Any omitted parameter will remain
intact in the persisted object.

## Delete Product

> **To delete a product, use this code:**

```
DELETE https://api.justklikkit.com/v1/products/:id
```

> Where `:id` is the product id, e.g. "59fb3e3aed922d4718a4f90c"

> **Success Response:**

> **HTTP Status Code:** 204

> The response has no body

Delete a given product.

### URI

`/products`

### Method

`DELETE`

### Parameters

- **:id**

The id of the product to delete.

# User

## Create User

> **To create a user, use this code:**

```
POST https://api.justklikkit.com/v1/user/create
```

> Post body:

```json
{
  "password": "secret",
  "firstname": "Joe",
  "lastname": "Schmoe",
  "email": "joe@example.com"
}
```

> **Success Response:**

> A newly created user is implicitly authenticated and the access_token
which should be used for accessing any resources belonging to this user
is returned when the user is succesfully created.

> **HTTP Status Code:** 200

```json
{
  "token_type": "Bearer",
  "expires_in": 2595600,
  "access_token": "d6be0db9297547536e122ceabc23ac4a670b078a78bc09aa22092516c77dfcc48730035d946c00a8a22286d9c207e3d5b455b9975b5442b5ff34ef75e5877a85"
}
```

A user must be created using this endpoint will automatically be authorized
and the access_token is returned when user creation succeeds. This
access_token must be used when accessing any of the other [User](#user),
[Schedule](#schedule), or [Press](#press) services.

### URI

`/user/create`

### Method

`POST`

### Parameters

Parameters can be posted as either form-data or application/json in
the request body. Username, password, and email are required parameters.

 - **email**

The `email` paramter must be unique and be a valid email address
conforming to RFCs [5321](http://tools.ietf.org/html/rfc5321),
[5322](http://tools.ietf.org/html/rfc5322),
[6530](http://tools.ietf.org/html/rfc6530) and others.

 - **password**

The `password` parameter must be non-empty.

 - **firstname**

The `firstname` parameter is optional.

 - **lastname**

The `lastname` parameter is optional.

## Read User

> **To read a user, use this code:**

```
GET https://api.justklikkit.com/v1/user/me
```

> **HTTP Status Code:** 200

```json
{
  "firstname": "Joe",
  "lastname": "Schmoe",
  "email": "joeschmoe@example.com",
  "shortId": 1071308335,
  "createdAt": "2017-10-31T15:06:07.073Z",
  "updatedAt": "2017-10-31T15:06:54.887Z",
  "id": "59f8915ff9f6a1295d08776c"
}
```

A user must be created and authorized using the [Get Access Token](#get-access-token)
endpoint before accessing any of the other [User](#user) services.

### URI

`/user/me`

### Method

`GET`

### Parameters

None. This method does not take any parameters.

## Update User

> **To update a user, use this code:**

```
POST https://api.justklikkit.com/v1/user/me
```

> Post body:

```json
{
  "password": "secret",
  "firstname": "Joe",
  "lastname": "Schmoe",
  "email": "joe@example.com"
}
```

> **Success Response:**

> An updated created user is implicitly authenticated and the access_token
which should be used for accessing any resources belonging to this user
is returned when the user is succesfully updated. The access token may be
different and previous access tokens should be considered invalid.

> **HTTP Status Code:** 200

```json
{
    "user": {
        "firstname": "Joe",
        "lastname": "Schmoe",
        "email": "joeschmoe@example.com",
        "shortId": 1071308335,
        "createdAt": "2017-10-31T15:06:07.073Z",
        "updatedAt": "2017-10-31T15:06:54.887Z",
        "id": "59f8915ff9f6a1295d08776c"
    },
    "authorization": {
        "type": "Bearer",
        "expires_in": 2592000,
        "access_token": "c26ccfecd4ae11ebdaca9fc4c2fda3cedd52e5c629d8a1897e1fec8539b2ab52e502ef2b4d1d47df65c8563c5466538a0349e760b71ff7fe27de8c4ffc29ea4f"
    }
}
```

Update the currently authenticated user with the given information.

### URI

`/user/me`

### Method

`PUT`

### Parameters

Parameters can be posted as either form-data or application/json in
the request body. Username, password, and email are required parameters.

 - **email**

If the `email` parameter is set then it must be unique and be a valid email address
conforming to RFCs [5321](http://tools.ietf.org/html/rfc5321),
[5322](http://tools.ietf.org/html/rfc5322),
[6530](http://tools.ietf.org/html/rfc6530) and others.

 - **password**

If the `password` parameter is set then it must be non-empty. For users
using facebook login, this parameter is ignored.

 - **firstname**

The `firstname` parameter is optional.

 - **lastname**

The `lastname` parameter is optional.

# Coffee

## Get Coffee

> **To get a cup of coffee:**

```
GET https://api.justklikkit.com/v1/coffee
```

> **Error Response:**

> **HTTP Status Code:** 418 I'm a teapot

```json
{
  "errors": [
    "                        (",
    "            _           ) )",
    "         _,(_)._        ((",
    "    ___,(_______).        )",
    "  ,'__.   /       \     /_",
    "/,' /  |""|.       \   /  /",
    "| | |   |__|        |,'  /",
    "\`.|                   /",
    "  `. :           :    /",
    "    `.            :.,'",
    "      `-.________,-'"
  ]
}
```

You may at some point want to get a cup of coffee and call the `/coffee`
endpoint. You would be disappointed if you do, because this isn't a coffee
service.

### URI

`/coffee`

### Method

`GET`

### Parameters

None. This method does not take any parameters.

See [RFC 2324](https://tools.ietf.org/html/rfc2324) for a description
of the "I'm a teapot" HTTP response code.




# Common Error Responses

> **Error Response**

> **HTTP Status Code:** 401

```json
{ "error": "Invalid access token" }
```

> **HTTP Status Code:** 404

```json
{ "error": "Object not found" }
```

All requests except [Get Access Token](#get-access-token) can respond
with **401 Unauthorized** if the `Authorization` header is not set or
is set to an invalid access token.

All `GET`, `PUT`, and `DELETE` requests to read, update, or delete a resource,
respectively, may return **404 Not Found** if the given resource id does
not match an existing object.
