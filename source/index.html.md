---
title: Wildfire API

language_tabs:
  - HTTPS
  - ruby

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - models

search: true
---

# Introduction

Welcome to the Wildfire API!

# Authentication

## Authentication Header Types

> HTTP Request Headers:

```shell
'Client-key': 'UNIQUE_CLIENT_KEY_GOES_HERE'
```

```shell
'Authorization': 'Basic my.email@gmail.com:s3cur3_p455w0rd'
```

```shell
'Authorization': 'Token o1yR62rL5yvvsmxLEVEXPjC3'
```

Wildfire uses multiple HTTP headers to authenticate API requests. The three different HTTP headers & their use cases for
each are described below.

### _Client-key_ Header (All requests)
The first is a `Client-key` header that is an unique API key which is checked for in *ALL* requests to the Wildfire API.
This API key can be obtained from an API administrator.

### HTTP Basic Auth Header
This header (in addition to the `Client-key` header) is only required on the `auth` endpoint requests. This header
should include the users's email & password as seen in the example to the right.

### HTTP Token Auth Header
This header (in addition to the `Client-key` header) is required on all requests that
are not the `auth` endpoint requests. Use the `token` endpoint to get an existing
user's `auth_token` for making further requests.

<aside class="notice">
You must use the LayerKit or Layer Android SDK in order to obtain the `nonce` used in the `token` endpoint request.
</aside>

## Verify Email Address

> Send a GET request with the email to be verified:

```shell
https://wildfire-staging.herokuapp.com/api/v1/auth/verify_email?email=woozykk@gmail.com
```

> If the email is available, the following JSON response will be returned:

```shell
status: 200
```
```json
{
  "email_exists": false
}
```

This endpoint is used during the on-boarding process to verify that the user's email address has not
already been registered with a previous account.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/auth/verify_email`

### Query Parameters

Parameter | Description
--------- | -----------
`email` | Specify email address to verify


## Register new User

> Send a POST request with the following parameters:

```json
{
  "email": "lauren@gmail.com",
  "first_name": "Lauren",
  "last_name": "Godwin",
  "dob": "1988-05-12",
  "city": "wilmington",
  "state": "NC",
  "avatar_url": "https://fake.urlto.img/Frieda_user_avatar",
  "digits_id": "74389204389432034948",
  "coords": {
    "lat": 34.4332,
    "lng": -77.8485
  },
  "password": "s3cur3_p455w0rd",
  "nonce": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxfQ.po9twTrX99V7XgAk5mVskkiq8aa0lpYOue62ehubRY4"
}
```

> The above request returns the following JSON:

```shell
status: 201
```
```json
{
  "id": 12,
  "email": "lk3317@gmail.com",
  "first_name": "Lauren",
  "last_name": "Godwin",
  "dob": "1988-05-12",
  "about": null,
  "city": "Wilmington",
  "state": "NC",
  "coords": {
    "lat": 34.2257,
    "lng": 77.9447
  },
  "push_notifs": {
    "general": true,
    "messages": true,
    "events": true,
    "mentions": true,
    "user_follows": true
  },
  "social_ids": {
    "facebook": null,
    "twitter": null,
    "instagram": null,
    "pinterest": null
  },
  "profile_tags":[
    { "tag_id": "20", "tag_name": "Sports" },
    { "tag_id": "22", "tag_name": "College" },
    { "tag_id": "29", "tag_name": "Football" }
  ],
  "avatar_url": "https:\/\/fake.urlto.img\/user_avatar",
  "auth_token": "WnwkLV4R9YDuD1bDcQvH7Pe6",
  "auth_token_expiry": "2016-11-09T15:38:59.493Z",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoibGszMzE3QGdtYWlsLmNvbSIsImlhdCI6MTQ3ODYxOTUzOSwiZXhwIjoxNDc5ODI5MTM5LCJuY2UiOiJfMTU3NnVQdFo5QzV1NXhlV0pVcXdHZzhTOVF5dnRNVmFhdGlTQ3c4dDlmWEE3NnFEX1NmSHBVeTMxYy1MRmVVNnlza3VuSXkyN3dzc0E1bHdYS0V3ZyJ9.PMtpDlAmR1U1DZt2ep_fWndEvn_Z16nFCoFw9CLE0DbAMLs_G8bHgUaytccjrDUi4iXSEwA4qVKrhSCWs_sR16qICKfNUgNevu4ioL3OYmIoZjExsm6hHVKO8B3s43gHlHIO7B8UWIWs5CuuRj2VK3piNnOkO63daChNPDZHdVVgq47-ldB2k9GuctRPHe8Zu3dZK2Wa5N24BOYah0K79V5Dntsbmre0UP9lVy4HQBv0VmnVcJBR4UNzrt4lFD2xJsX-SdZYrFPWjVuYN4GqyvFOO2N8A1NPd7hYcBqVY8cdY0ZQ2KA5qboMaA5-2XxAQuTfOx36FwUSWoatUq80yA",
  "active": true,
  "last_login_time": "2016-11-08T15:38:59.493Z",
  "digits_id": "36345202355432034911"
}
```

Description: Used to register a new client user and obtain a Layer `identity_token`

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/users/register`

<aside class="notice">
The `nonce` parameter is a JWT obtained from Layer using the Layer iOS or Android SDK. The server uses this
to create an `identity_token` for the client to use on future Layer requests.
</aside>

## Login/Authenticate a User via Email/Password

> Along with the Basic Auth Header, send a POST request with the 'nonce' from Layer

```shell
'Authorization': 'Basic my.email@gmail.com:s3cur3_p455w0rd'
```

```json
{
  "nonce": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxfQ.po9twTrX99V7XgAk5mVskkiq8aa0lpYOue62ehubRY4"
}
```

> The above request returns JSON structured like this:

```shell
status: 200
```
```json
{
  "id": 12,
  "email": "lk3317@gmail.com",
  "first_name": "Lauren",
  "last_name": "Godwin",
  "dob": "1988-05-12",
  "about": null,
  "city": "Wilmington",
  "state": "NC",
  "coords": {
    "lat": 34.2257,
    "lng": 77.9447
  },
  "push_notifs": {
    "general": true,
    "messages": true,
    "events": true,
    "mentions": true,
    "user_follows": true
  },
  "social_ids": {
    "facebook": null,
    "twitter": null,
    "instagram": null,
    "pinterest": null
  },
  "profile_tags": [
    { "tag_id": "20", "tag_name": "Sports" },
    { "tag_id": "22", "tag_name": "College" },
    { "tag_id": "29", "tag_name": "Football" }
  ],
  "avatar_url": "https:\/\/fake.urlto.img\/user_avatar",
  "auth_token": "WnwkLV4R9YDuD1bDcQvH7Pe6",
  "auth_token_expiry": "2016-11-09T15:38:59.493Z",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoibGszMzE3QGdtYWlsLmNvbSIsImlhdCI6MTQ3ODYxOTUzOSwiZXhwIjoxNDc5ODI5MTM5LCJuY2UiOiJfMTU3NnVQdFo5QzV1NXhlV0pVcXdHZzhTOVF5dnRNVmFhdGlTQ3c4dDlmWEE3NnFEX1NmSHBVeTMxYy1MRmVVNnlza3VuSXkyN3dzc0E1bHdYS0V3ZyJ9.PMtpDlAmR1U1DZt2ep_fWndEvn_Z16nFCoFw9CLE0DbAMLs_G8bHgUaytccjrDUi4iXSEwA4qVKrhSCWs_sR16qICKfNUgNevu4ioL3OYmIoZjExsm6hHVKO8B3s43gHlHIO7B8UWIWs5CuuRj2VK3piNnOkO63daChNPDZHdVVgq47-ldB2k9GuctRPHe8Zu3dZK2Wa5N24BOYah0K79V5Dntsbmre0UP9lVy4HQBv0VmnVcJBR4UNzrt4lFD2xJsX-SdZYrFPWjVuYN4GqyvFOO2N8A1NPd7hYcBqVY8cdY0ZQ2KA5qboMaA5-2XxAQuTfOx36FwUSWoatUq80yA",
  "active": true,
  "last_login_time": "2016-11-08T15:38:59.493Z",
  "digits_id": "36345202355432034911"
}
```

Description: Used to authenticate an existing user and obtain a Layer `identity_token`. A request
to this endpoint should include the users's email & password in HTTP Basc Auth header, as well as the `nonce` in
body of the request.

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/auth`

<aside class="notice">
Along with the Basic Auth Header, send a POST request with the ‘nonce’ from Layer
</aside>

## Logout the current User

> Along with the Token Auth Header, send a PATCH request to the specified URI:

```shell
'Authorization': 'Token Ac7qVZsS5Az7SVdgcrnEgXbf'
```

> The server will respond with the following JSON:

```shell
status: 200
```
```json
{
  "message": "Successfully logged-out user."
}
```

Description: Used to logout the current user. This will set their `active` attribute to `false`,
and delete their `auth_token`, forcing them to re-authenticate on the following request.

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/me/logout`

## Refresh Layer Identity Token for Current User

> Send a POST request with required parameters:

```json
{
  "nonce": "c61s5_f-_uOSJztlBr0QPVWhuUsgTKBZ91sSAy-oR0Ulmvkm6f4NOauFZesjDvIiyouFg57sI35LYu7ysMHOMQ"
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoiNDY2MzE3Mzk0IiwiaWF0IjoxNDc4NjMxOTE2LCJleHAiOjE0Nzk4NDE1MTYsIm5jZSI6bnVsbH0.EV-yLIsPdBthQCK2bboi307kOj4v3bKlc_0pB35v0NVLC5QExDukFwZlg5kFk0NlgyLDYLpRlbpefX_inEjE18wAM--JkYl1EDqFxKfehK-eiL7Hjyt8Nis-uuKF5S4fSrJ0vU_uG10LLVxHw3LJtM9shw8V2tZRGOfEroxa4Zp24LO0NKQP7QlxnTFxU1YOzmR3L88hpMvYrXT27oWV_vAsjjVHhpltlTVk-J1vmHNWG28IuJDHtEmznJzJHchWk3J34xGs9EOXflAyxwPxbGIYZfxbm0RW-XNNjPwAK9-Xgv9MTVOK7ymo_oGK_n2IDcUaa9gx0eFFLlGE9GJvs"
}
```

Description: If the `current_user`'s Layer identity token has expired, use this endpoint to obtain a fresh
identity token for making user authenticated requests to Layer's API

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/auth/refresh_identity_token`

# Users

## Update User

> Send a PATCH request with any of the user object's parameters:

```json
{
  "about": "An updated bio from the client app!"
}
```

> For Updating password, send the following parameters:

```json
{
  "password": "new_password",
  "password_confirmation": "new_password"
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 23,
  "first_name": "Jordan",
  "last_name": "Godwin",
  "about": "An updated bio from the client app!",
  "auth_token": "cRLSSiGNKfykY9DdsJQonkTp",
  "auth_token_expiry": "2016-10-05T16:16:54.827Z",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoiamVnMzIyNEBnbWFpbC5jb20iLCJpYXQiOjE0NzU2MDE1MzIsImV4cCI6MTQ3NjgxMTEzMiwibmNlIjpudWxsfQ.NN-AW7gpnklCoMKzZ_4MoXi0b6XJJQR1nZRXATd3M1nLe1VZk9NlIr-1hYbVRspeZOm4oZuN5HJslLMiYbEot5bm48I1OS0vqqwo64azZSVGTqmJl2GAm_lRizbb10Ic90YUdUPsrMeBfaq4B1yyGX03o2xSPQvDwBetGpqAJ_6oJkH_nDi0ZABLFtml1UHtUjKxOMU06-42r8L8FDTNOHWpw7bvB-hUa1T42OFO9cuDnPytu8yZeqEeS8MoXBY6-KUoI7qaJuaEI9G2IXYlodDZkAwW38Q_hWEp01umPSwB9551E5hApU2sJS2aih2A06tvP23yRt0r1JCI4w5pbQ",
  "email": "jeg3224@gmail.com",
  "dob": "1987-02-09",
  "active": true,
  "avatar_url": "https:\/\/fake.urlto.img\/user_avatar",
  "last_login_time": "2016-10-04T16:16:54.827Z",
  "push_notifs": {
    "event": false,
    "follows": false,
    "general": false,
    "mentions": false,
    "messages": false
  },
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "profile_tags": [
    { "tag_id": "20", "tag_name": "Sports" },
    { "tag_id": "22", "tag_name": "College" },
    { "tag_id": "29", "tag_name": "Football" }
  ]
}
```

Description: Updates the currently logged in user's profile

### HTTP Request

`PATCH https://wildfire-dev.herokuapp.com/api/v1/me`

## Deactivate the current User

> Along with the Token Auth Header, send a PATCH request to the specified URI:

```shell
'Authorization': 'Token Ac7qVZsS5Az7SVdgcrnEgXbf'
```

> The server will respond with the following JSON:

s
```json
{
  "message": "User logged-out & account deactivated"
}
```

Description: Used to deactivate the current user. Upon authentication of the current user,
the following data on the auth'ed user's model will be updated:

`active: false`
`push_notifs: { general: false, messages: false, event: false, mentions: false, follows: false }`
`auth_token: nil`
`auth_token_expiry: Time.zone.now`
`password_reset_token: nil`


### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/me/deactivate`

## Find Users by Digits ID

> Along with the Token Auth Header, send a POST request to the specified URI:

```shell
'Authorization': 'Token Ac7qVZsS5Az7SVdgcrnEgXbf'
```

```json
{
  "digits_ids": ["21659940719036163543", "19008283224256407389", "22530798334117390808", "68701303679444498592", "64307672012076350746", "28459077552041890622"]
}
```

> The server will respond with ONE of the following JSON reponses:

```shell
status: 200
```
```json
[
  {
    "id": 13,
    "first_name": "Jordy",
    "last_name": "D'Amore",
    "avatar_url": "https://fake.urlto.img/Jordy_user_avatar"
  },
  {
    "id": 14,
    "first_name": "Jaquan",
    "last_name": "Breitenberg",
    "avatar_url": "https://fake.urlto.img/Jaquan_user_avatar"
  },
  {
    "id": 15,
    "first_name": "Frieda",
    "last_name": "Stamm",
    "avatar_url": "https://fake.urlto.img/Frieda_user_avatar"
  }
  {
    "id": 16,
    "first_name": "Brice",
    "last_name": "Marvin",
    "avatar_url": "https://fake.urlto.img/Brice_user_avatar"
  },
  {
    "id": 17,
    "first_name": "Titus",
    "last_name": "Gleichner",
    "avatar_url": "https://fake.urlto.img/Titus_user_avatar"
  },
  {
    "id": 18,
    "first_name": "Elwyn",
    "last_name": "Ledner",
    "avatar_url": "https://fake.urlto.img/Elwyn_user_avatar"
  }
]
```

Description: Use this endpoint to pass an array of unique `:id`'s obtained from Digits,
in which will then be matched on users in the database on each user's `digit_id` attribute.


### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/users/find_digits`

## Get User Profile

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/5/profile
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 2,
  "first_name": "Kerry",
  "last_name": "Knight",
  "dob": "1980-11-17",
  "about": "about kerry knight",
  "city": "Wilmington",
  "state": "NC",
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/5",
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "followers_count": 16,
  "following_count": 9,
  "attended_count": 0,
  "block_status": false,
  "follow_status": false,
  "profile_tags": [
    {
      "id": 14,
      "name": "Lakers",
      "parent_id": 3
    }
  ],
  "events": [
    {
      "id": 4,
      "description": "Sed aut omnis quaerat quisquam hic est et possimus.",
      "media_url": "http://lorempixel.com/600/600/sports/7",
      "media_thumb": "http://lorempixel.com/128/128/sports/5",
      "media_type": 0,
      "comments_count": 0,
      "attendee_count": 1,
      "privacy": 0,
      "starts_at": "2017-01-03T09:53:32.000Z",
      "ends_at": "2017-01-03T09:53:35.810Z",
      "duration": 3.81
    },
    {
      "id": 3,
      "description": "Vel dolores earum sint saepe ea iste.",
      "media_url": "http://lorempixel.com/600/600/sports/9",
      "media_thumb": "http://lorempixel.com/128/128/sports/9",
      "media_type": 0,
      "comments_count": 0,
      "attendee_count": 2,
      "privacy": 0,
      "starts_at": "2017-01-04T14:20:45.000Z",
      "ends_at": "2017-01-04T14:20:45.149Z",
      "duration": 0.15
    },
    {
      "id": 2,
      "description": "Sit enim atque autem repellendus odit iusto tempora in.",
      "media_url": "http://lorempixel.com/600/600/sports/4",
      "media_thumb": "http://lorempixel.com/128/128/sports/2",
      "media_type": 0,
      "comments_count": 0,
      "attendee_count": 0,
      "privacy": 0,
      "starts_at": "2017-01-04T21:10:23.000Z",
      "ends_at": "2017-01-04T21:10:28.490Z",
      "duration": 5.49
    },
    {
      "id": 1,
      "description": "Non rerum repellendus ipsa et sunt quas vel.",
      "media_url": "http://lorempixel.com/600/600/sports/9",
      "media_thumb": "http://lorempixel.com/128/128/sports/2",
      "media_type": 0,
      "comments_count": 0,
      "attendee_count": 0,
      "privacy": 0,
      "starts_at": "2017-01-04T11:18:59.000Z",
      "ends_at": "2017-01-04T11:19:04.419Z",
      "duration": 5.42
    }
  ]
}
```

This endpoint will return the profile attributes for a specified user.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/profile`

## Retrieve User Followers

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/followers
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "first_name": "Lauren",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_3",
    "follow_status": true
  },
  {
    "id": 5,
    "name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_5",
    "follow_status": true
  },
  {
    "id": 12,
    "name": "Gavin",
    "last_name": "Anthony",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_12",
    "follow_status": true
  }
  ,{
    "id": 23,
    "name": "Kerry",
    "last_name": "Knight",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_23",
    "follow_status": true
  }
]
```

Description: This endpoint will return an array of users that are
followers of the specified user.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/followers`

## Retrieve User Followings

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/following
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "first_name": "Lauren",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_3",
    "follow_status": true
  },
  {
    "id": 5,
    "name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_5",
    "follow_status": true
  },
  {
    "id": 12,
    "name": "Gavin",
    "last_name": "Anthony",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_12",
    "follow_status": true
  }
  ,{
    "id": 23,
    "name": "Kerry",
    "last_name": "Knight",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_23",
    "follow_status": true
  }
]
```

Description: This endpoint will return an array of users that the
requesting user is currently following.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/following`

## Follow a User

> Send a POST request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/follow
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 3,
  "first_name": "Lauren",
  "last_name": "Godwin",
  "avatar_url": "https://fake.urlto.img/user_avatar_3",
  "follow_status": true
}
```

Description: Use this endpoint to follow a target user (create a follow) for the
current_user with the target of another user. This is the Twitter-style 'one-way' Follow.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/users/:id/follow`

## Unfollow a User

> Send a DELETE request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/55/unfollow
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 55,
  "first_name": "Jordan",
  "last_name": "Godwin",
  "avatar_url": "https://fake.urlto.img/user_avatar_55",
  "follow_status": false
}
```

Description: Use this endpoint to unfollow a target user (delete a follow)
for the current_user with the target of another user.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/users/:id/unfollow`

## Retrieve Blocked Users

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/me/blocked
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "first_name": "Lauren",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_3",
    "block_status": true
  },
  {
    "id": 5,
    "name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_5",
    "block_status": true
  },
  {
    "id": 12,
    "name": "Gavin",
    "last_name": "Anthony",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_12",
    "block_status": true
  }
  ,{
    "id": 23,
    "name": "Kerry",
    "last_name": "Knight",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_23",
    "block_status": true
  }
]
```

Description: This endpoint will return an array of users that the
requesting user has blocked.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/me/blocked`

## Block a User

> Send a POST request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/block
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 3,
  "first_name": "Lauren",
  "last_name": "Godwin",
  "avatar_url": "https://fake.urlto.img/user_avatar_3",
  "block_status": true
}
```

Description: Use this endpoint to block a target user (create a block) for the
current_user with the target of another user.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/users/:id/block`

## Unblock a User

> Send a DELETE request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/55/unblock
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 55,
  "first_name": "Jordan",
  "last_name": "Godwin",
  "avatar_url": "https://fake.urlto.img/user_avatar_55",
  "block_status": true
}
```

Description: Use this endpoint to unblock a target user (remove a block)
for the current_user with the target of another user.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/users/:id/unblock`

## User Profile Tags

> Send a PATCH request with required parameters:

```json
{
  "profile_tag_ids": [1, 3, 5]
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 1,
  "email": "jordan@likeli.co",
  "first_name": "Jordan",
  "last_name": "Godwin",
  "dob": "1987-02-03",
  "about": "This is all about Jordan.",
  "city": "Wilmington",
  "state": "NC",
  "coords": {
    "lat": 34.2257,
    "lng": -77.9447
  },
  "push_notifs": {
    "general": true,
    "mentions": true,
    "messages": true,
    "user_follows": true,
    "event_updates": true,
    "join_requests": true,
    "event_comments": true
  },
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/6",
  "auth_token": "vQWzcVfUcbhfwFQ1nk421Z6F",
  "auth_token_expiry": "2016-11-29T23:40:41.146Z",
  "active": true,
  "last_login_time": "2016-11-22T23:40:41.146Z",
  "digits_id": "771065260681887744",
  "profile_tags": [
    {
      "id": 5,
      "name": "Duke",
      "parent_id": 4
    },
    {
      "id": 3,
      "name": "Professional",
      "parent_id": 2
    },
    {
      "id": 1,
      "name": "Sports",
      "parent_id": ""
    },
    {
      "id": 2,
      "name": "Basketball",
      "parent_id": 1
    }
  ],
  "feed_tags": [
    {
      "id": 5,
      "name": "Duke",
      "parent_id": 4
    },
    {
      "id": 1,
      "name": "Sports",
      "parent_id": ""
    },
    {
      "id": 2,
      "name": "Basketball",
      "parent_id": 1
    }
  ]
}
```

This endpoint creates, updates, and deletes Profile tags for the currently logged in user.

### HTTP Request

`PATCH https://wildfire-dev.herokuapp.com/api/v1/me/profile_tags`

## User Feed Tags

> Send a PATCH request with required parameters:

```json
{
  "feed_tag_ids": [1, 2, 5]
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 1,
  "email": "jordan@likeli.co",
  "first_name": "Jordan",
  "last_name": "Godwin",
  "dob": "1987-02-03",
  "about": "This is all about Jordan.",
  "city": "Wilmington",
  "state": "NC",
  "coords": {
    "lat": 34.2257,
    "lng": -77.9447
  },
  "push_notifs": {
    "general": true,
    "mentions": true,
    "messages": true,
    "user_follows": true,
    "event_updates": true,
    "join_requests": true,
    "event_comments": true
  },
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/6",
  "auth_token": "vQWzcVfUcbhfwFQ1nk421Z6F",
  "auth_token_expiry": "2016-11-29T23:40:41.146Z",
  "active": true,
  "last_login_time": "2016-11-22T23:40:41.146Z",
  "digits_id": "771065260681887744",
  "profile_tags": [
    {
      "id": 5,
      "name": "Duke",
      "parent_id": 4
    },
    {
      "id": 3,
      "name": "Professional",
      "parent_id": 2
    },
    {
      "id": 1,
      "name": "Sports",
      "parent_id": ""
    },
    {
      "id": 2,
      "name": "Basketball",
      "parent_id": 1
    }
  ],
  "feed_tags": [
    {
      "id": 5,
      "name": "Duke",
      "parent_id": 4
    },
    {
      "id": 1,
      "name": "Sports",
      "parent_id": ""
    },
    {
      "id": 2,
      "name": "Basketball",
      "parent_id": 1
    }
  ]
}
```

This endpoint creates, updates, and deletes filter Feed tags for the currently logged in user.

### HTTP Request

`PATCH https://wildfire-dev.herokuapp.com/api/v1/me/feed_tags`

## Hosted Events for User

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/1/events/hosted
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 66,
    "description": "Minima aspernatur quia rerum ipsum veniam.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/9",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/5",
    "media_type": null,
    "comments_count": 0,
    "attendee_count": 1,
    "privacy": 0,
    "starts_at": "2017-01-06T01:03:14.000Z",
    "ends_at": "2017-01-06T01:03:18.709Z",
    "duration": 4.71,
    "host": {
      "id": 1,
      "first_name": "Jordan",
      "last_name": "Godwin",
      "dob": "1987-02-03"
    },
    "venue": null,
    "tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null,
      },
      {
        "id": 3,
        "name": "Football",
        "parent_id": 1,
      }
    ],
    "attendees": [
      {
        "id": 30,
        "first_name": "Charlotte",
        "last_name": "Jenkins",
        "avatar_url": "http://lorempixel.com/300/300/cats/2",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  },
  {
    "id": 109,
    "description": "My First Event!",
    "media_url": "http://www.aws.com/media_url",
    "media_thumb": "http://www.aws.com/media_thumb",
    "media_type": 0,
    "comments_count": 0,
    "attendee_count": 0,
    "privacy": 0,
    "starts_at": "2017-01-10T00:00:00.000Z",
    "ends_at": "2017-01-10T06:00:00.000Z",
    "duration": 6,
    "host": {
      "id": 1,
      "first_name": "Jordan",
      "last_name": "Godwin",
      "dob": "1987-02-03"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null,
      },
      {
        "id": 3,
        "name": "Football",
        "parent_id": 1,
      },
      {
        "id": 11,
        "name": "NASCAR",
        "parent_id": 7,
      }
    ],
    "attendees": [],
    "comments": []
  }
]
```

This endpoint will return an array of Events that the specified
user has hosted.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/events/hosted`

## Joined Events for User

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/276/events/joined
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 50,
    "description": "Veniam omnis mollitia est similique deleniti accusantium cumque.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/8",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/9",
    "media_type": 0,
    "comments_count": 0,
    "attendee_count": 1,
    "privacy": 0,
    "starts_at": "2017-01-03T10:40:17.000Z",
    "ends_at": "2017-01-03T10:40:17.660Z",
    "duration": 0.66,
    "host": {
      "id": 21,
      "first_name": "Reagan",
      "last_name": "Smitham",
      "dob": "1984-07-10"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 21,
        "name": "Social Entertainment",
        "parent_id": 38
      }
    ],
    "attendees": [
      {
        "id": 276,
        "first_name": "Adeline",
        "last_name": "Ziemann",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  },
  {
    "id": 77,
    "description": "Expedita aliquam veritatis autem quo itaque.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/8",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/4",
    "media_type": null,
    "comments_count": 0,
    "attendee_count": 4,
    "privacy": 0,
    "starts_at": "2017-01-05T07:22:26.000Z",
    "ends_at": "2017-01-05T07:22:35.429Z",
    "duration": 9.43,
    "host": {
      "id": 16,
      "first_name": "Lia",
      "last_name": "Koch",
      "dob": "1996-03-21"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 3,
        "name": "Motorsports",
        "parent_id": 4
      },
      {
        "id": 3,
        "name": "Extreme Sports",
        "parent_id": 20
      }
    ],
    "attendees": [
      {
        "id": 241,
        "first_name": "Christopher",
        "last_name": "Goyette",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/3",
        "block_status": false,
        "follow_status": false
      },
      {
        "id": 201,
        "first_name": "Lance",
        "last_name": "Hudson",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
        "block_status": false,
        "follow_status": false
      },
      {
        "id": 276,
        "first_name": "Adeline",
        "last_name": "Ziemann",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  },
  {
    "id": 95,
    "description": "Eos quaerat expedita nobis perferendis.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/3",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/7",
    "media_type": null,
    "comments_count": 0,
    "attendee_count": 1,
    "privacy": 0,
    "starts_at": "2017-01-05T01:21:10.000Z",
    "ends_at": "2017-01-05T01:21:12.020Z",
    "duration": 2.02,
    "host": {
      "id": 48,
      "first_name": "Aric",
      "last_name": "Ratke",
      "dob": "1997-12-16"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null
      },
      {
        "id": 3,
        "name": "Basketball",
        "parent_id": 1
      }
    ],
    "attendees": [
      {
        "id": 276,
        "first_name": "Adeline",
        "last_name": "Ziemann",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  }
]
```

This endpoint will return an array of Events that the specified
user has joined.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/events/joined`

## Event Join Requests for User

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/57/events/requested
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 33,
    "description": "Exercitationem doloremque eaque ea dolor qui.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/2",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/9",
    "media_type": null,
    "comments_count": 0,
    "attendee_count": 4,
    "privacy": 0,
    "starts_at": "2017-01-03T08:54:01.000Z",
    "ends_at": "2017-01-03T08:54:03.270Z",
    "duration": 2.27,
    "host": {
      "id": 26,
      "first_name": "Marion",
      "last_name": "Powlowski",
      "dob": "1984-08-09"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 3,
        "name": "Motorsports",
        "parent_id": 4
      },
      {
        "id": 3,
        "name": "Extreme Sports",
        "parent_id": 20
      }
    ],
    "attendees": [
      {
        "id": 354,
        "first_name": "Charlene",
        "last_name": "Reilly",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/4",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  },
  {
    "id": 81,
    "description": "Molestias sunt cupiditate consequatur.",
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/6",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/4",
    "media_type": null,
    "comments_count": 0,
    "attendee_count": 2,
    "privacy": 0,
    "starts_at": "2017-01-05T22:22:18.000Z",
    "ends_at": "2017-01-05T22:22:24.509Z",
    "duration": 6.51,
    "host": {
      "id": 58,
      "first_name": "Constance",
      "last_name": "Torphy",
      "dob": "1987-05-26"
    },
    "venue": {
      "name": "New Venue",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "category": "Beach Bar",
      "venue_id": 37483747
    },
    "tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null
      },
      {
        "id": 3,
        "name": "Basketball",
        "parent_id": 1
      }
    ],
    "attendees": [
      {
        "id": 350,
        "first_name": "Yadira",
        "last_name": "Gerhold",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
        "block_status": false,
        "follow_status": false
      }
    ],
    "comments": []
  }
]
```

This endpoint will return an array of Events that the specified
user has joined.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/events/requested`

# Events

## Create Event

> Send a POST request with required parameters:

```json
{
  "description": "My First Event!",
  "media_url": "http://www.aws.com/media_url",
  "media_thumb": "http://www.aws.com/media_thumb",
  "media_type": "0",
  "tag_ids": [3, 1, 11],
  "starts_at": "2017-01-10",
  "duration": 6,
  "venue": {
    "venue_id": 37483747,
    "name": "New Venue",
    "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
    "category": "Beach Bar",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
  }
}
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 109,
  "description": "My First Event!",
  "media_url": "http://www.aws.com/media_url",
  "media_thumb": "http://www.aws.com/media_thumb",
  "media_type": 0,
  "comments_count": 0,
  "attendee_count": 0,
  "privacy": 0,
  "starts_at": "2017-01-10T00:00:00.000Z",
  "ends_at": "2017-01-10T06:00:00.000Z",
  "duration": 6,
  "host": {
    "id": 1,
    "first_name": "Jordan",
    "last_name": "Godwin",
    "dob": "1987-02-03"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 1,
      "name": "Sports",
      "parent_id": null
    },
    {
      "id": 3,
      "name": "Football",
      "parent_id": 1
    },
    {
      "id": 11,
      "name": "NASCAR",
      "parent_id": 7
    }
  ],
  "attendees": [],
  "comments": []
}
```

This endpoint creates an Event for the currently logged in user.

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/events`

## Show Event

> Send a GET request to the following URL:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 22,
  "description": "Sed aut omnis quaerat quisquam hic est et possimus.",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/7",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/5",
  "media_type": 0,
  "comments_count": 0,
  "attendee_count": 1,
  "privacy": 0,
  "starts_at": "2017-01-03T09:53:32.000Z",
  "ends_at": "2017-01-03T09:53:35.810Z",
  "duration": 3.81,
  "host": {
    "id": 2,
    "first_name": "Kerry",
    "last_name": "Knight",
    "dob": "1980-11-17"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 1,
      "name": "Sports",
      "parent_id": null,
      "created_at": "2017-01-04T15:57:02.323Z",
      "updated_at": "2017-01-04T15:57:02.323Z"
    },
    {
      "id": 2,
      "name": "Basketball",
      "parent_id": 1,
      "created_at": "2017-01-04T15:57:02.372Z",
      "updated_at": "2017-01-04T15:57:02.372Z"
    }
  ],
  "attendees": [
    {
      "id": 489,
      "first_name": "Stacey",
      "last_name": "Armstrong",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/3",
      "block_status": false,
      "follow_status": false
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "See you there! I'll bring the beer!!",
      "user_id": 1
    }
  ]
}
```

Description: Use this endpoint to display an Event using it's
`:id` attribute in the URI request.

### HTTP Request

`GET https://wildfire-dev.herokuapp.com/api/v1/events/:id`

## Update Event

> Send a PATCH request with required parameters:

```json
{
  "description": "Updated test event desc",
  "tag_ids": [26, 160]
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 4,
  "description": "An updated event description!",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/7",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/5",
  "media_type": 0,
  "comments_count": 1,
  "attendee_count": 1,
  "privacy": 0,
  "starts_at": "2017-01-10T00:00:00.000Z",
  "ends_at": "2017-01-10T06:00:00.000Z",
  "duration": 6,
  "host": {
    "id": 2,
    "first_name": "Kerry",
    "last_name": "Knight",
    "dob": "1980-11-17"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 26,
      "name": "OVC Basketball",
      "parent_id": 12,
    },
    {
      "id": 160,
      "name": "IW Cardinals Basketball",
      "parent_id": 155,
    }
  ],
  "attendees": [
    {
      "id": 489,
      "first_name": "Stacey",
      "last_name": "Armstrong",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/3",
      "block_status": false,
      "follow_status": false
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "See you there! I'll bring the beer!!",
      "user_id": 1
    }
  ]
}
```

Description: This endpoint updates an Event for the event host by passing
ALL of the Event's attributes, including the ones that aren't being updated

### HTTP Request

`PATCH https://wildfire-dev.herokuapp.com/api/v1/events/:id`

## Delete Event

> Send a DELETE request with Event ID in the URI:

```
DELETE http://wildfire-dev.herokuapp.com/api/v1/events/9
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{ "message": "Event was successfully deleted." }
```

Description: This endpoint deletes an Event for the event host.

### HTTP Request

`DELETE https://wildfire-dev.herokuapp.com/api/v1/events/:id`

## Join an Event

> Send a POST request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/15/join
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 15,
  "description": "Et rerum vero eveniet excepturi.",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/6",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/5",
  "media_type": null,
  "comments_count": 0,
  "attendee_count": 1,
  "privacy": 0,
  "starts_at": "2017-01-04T12:22:53.000Z",
  "ends_at": "2017-01-04T12:23:02.630Z",
  "duration": 9.63,
  "host": {
    "id": 7,
    "first_name": "Carlotta",
    "last_name": "Huels",
    "dob": "1998-08-05"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 160,
      "name": "IW Cardinals Basketball",
      "parent_id": 155,
    }
  ],
  "attendees": [
    {
      "id": 1,
      "first_name": "Jordan",
      "last_name": "Godwin",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/1"
    },
    {
      "id": 2,
      "first_name": "Kerry",
      "last_name": "Knight",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/2"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "See you there! I'll bring the beer!!",
      "user_id": 1
    }
  ]
}
```

This endpoint will create a 'join' (Attendant record w/ an `:attending` status)
for the specified event.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/join`

## Unjoin an Event

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/15/unjoin
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 15,
  "description": "Et rerum vero eveniet excepturi.",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/6",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/5",
  "media_type": null,
  "comments_count": 0,
  "attendee_count": 1,
  "privacy": 0,
  "starts_at": "2017-01-04T12:22:53.000Z",
  "ends_at": "2017-01-04T12:23:02.630Z",
  "duration": 9.63,
  "host": {
    "id": 7,
    "first_name": "Carlotta",
    "last_name": "Huels",
    "dob": "1998-08-05"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 160,
      "name": "IW Cardinals Basketball",
      "parent_id": 155,
    }
  ],
  "attendees": [
    {
      "id": 2,
      "first_name": "Kerry",
      "last_name": "Knight",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/2"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "See you there! I'll bring the beer!!",
      "user_id": 1
    }
  ]
}
```

This endpoint will _destroy_ a previously created 'join' (Attendant record w/ an `:attending` status)
from the specified event.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/unjoin`

## Create an Event Join Request

> Send a POST request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/50/requests
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 50,
  "description": "Veniam omnis mollitia est similique deleniti accusantium cumque.",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/8",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/9",
  "media_type": null,
  "comments_count": 0,
  "attendee_count": 1,
  "privacy": 1,
  "starts_at": "2017-01-03T10:40:17.000Z",
  "ends_at": "2017-01-03T10:40:17.660Z",
  "duration": 0.66,
  "host": {
    "id": 21,
    "first_name": "Reagan",
    "last_name": "Smitham",
    "dob": "1984-07-10"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 160,
      "name": "IW Cardinals Basketball",
      "parent_id": 155,
    }
  ],
  "attendees": [
    {
      "id": 276,
      "first_name": "Adeline",
      "last_name": "Ziemann",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
      "block_status": false,
      "follow_status": false
    }
  ],
  "requests": [
    {
      "id": 1,
      "first_name": "Jordan",
      "last_name": "Godwin",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/2"
    },
    {
      "id": 2,
      "first_name": "Kerry",
      "last_name": "Knight",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/2"
    }
  ],
  "comments": []
}
```

This endpoint will create a 'request' (Attendant record w/ a `:requested` status)
for the specified private event.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/requests`

## Remove/Ignore a Join Request

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/32/requests/:user_id
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 50,
  "description": "Veniam omnis mollitia est similique deleniti accusantium cumque.",
  "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/8",
  "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/9",
  "media_type": null,
  "comments_count": 0,
  "attendee_count": 1,
  "privacy": 1,
  "starts_at": "2017-01-03T10:40:17.000Z",
  "ends_at": "2017-01-03T10:40:17.660Z",
  "duration": 0.66,
  "host": {
    "id": 21,
    "first_name": "Reagan",
    "last_name": "Smitham",
    "dob": "1984-07-10"
  },
  "venue": {
    "name": "New Venue",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Beach Bar",
    "venue_id": 37483747
  },
  "tags": [
    {
      "id": 160,
      "name": "IW Cardinals Basketball",
      "parent_id": 155,
    }
  ],
  "attendees": [
    {
      "id": 276,
      "first_name": "Adeline",
      "last_name": "Ziemann",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
      "block_status": false,
      "follow_status": false
    }
  ],
  "requests": [
    {
      "id": 2,
      "first_name": "Kerry",
      "last_name": "Knight",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/2"
    }
  ],
  "comments": []
}
```

This endpoint will _destroy_ a previously created 'request' (Attendant record w/ a `:requested` status)
from the specified event.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/unrequest`

## Show Event Attendees

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22/attendees
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 45,
    "first_name": "Leonora",
    "last_name": "Bergstrom",
    "avatar_url": "http://lorempixel.com/300/300/cats/6",
    "follow_status": false
  },
  {
    "id": 324,
    "first_name": "Arvid",
    "last_name": "Mertz",
    "avatar_url": "http://lorempixel.com/300/300/cats/7",
    "follow_status": true
  },
  {
    "id": 363,
    "first_name": "Eugene",
    "last_name": "Hand",
    "avatar_url": "http://lorempixel.com/300/300/cats/9",
    "follow_status": false
  },
  {
    "id": 473,
    "first_name": "Jett",
    "last_name": "Shanahan",
    "avatar_url": "http://lorempixel.com/300/300/cats/3",
    "follow_status": false
  }
]
```

This endpoint will return an array of users who are attending
the event.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/attendees`

## Show Event Requests

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22/requests
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 45,
    "first_name": "Leonora",
    "last_name": "Bergstrom",
    "avatar_url": "http://lorempixel.com/300/300/cats/6",
    "follow_status": false
  },
  {
    "id": 324,
    "first_name": "Arvid",
    "last_name": "Mertz",
    "avatar_url": "http://lorempixel.com/300/300/cats/7",
    "follow_status": true
  },
  {
    "id": 363,
    "first_name": "Eugene",
    "last_name": "Hand",
    "avatar_url": "http://lorempixel.com/300/300/cats/9",
    "follow_status": false
  },
  {
    "id": 473,
    "first_name": "Jett",
    "last_name": "Shanahan",
    "avatar_url": "http://lorempixel.com/300/300/cats/3",
    "follow_status": false
  }
]
```

This endpoint will return an array of users who are have requested
to join the event.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/requests`

# Tags

## Get all Tags

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/tags
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 1,
    "name": "Sports"
  },
  {
    "id": 2,
    "name": "Basketball",
    "parent_id_0": 1
  },
  {
    "id": 3,
    "name": "Professional",
    "parent_id_0": 1,
    "parent_id_1": 2
  },
  {
    "id": 4,
    "name": "College",
    "parent_id_0": 1,
    "parent_id_1": 2
  },
  {
    ...
  }
  {
    "id": 5,
    "name": "Duke",
    "parent_id_0": 1,
    "parent_id_1": 2,
    "parent_id_2": 4
  },
  {
    "id": 6,
    "name": "UNC",
    "parent_id_0": 1,
    "parent_id_1": 2,
    "parent_id_2": 4
  },
  {
    "id": 7,
    "name": "NCSU",
    "parent_id_0": 1,
    "parent_id_1": 2,
    "parent_id_2": 4
  }
]
```

This endpoint will return a flat array of Tag objects

### HTTP Request

`{HTTP_VERB} https://wildfire-staging.herokuapp.com/api/v1/tags`

# Venues

## Foursquare Venues (Venue Name Query)

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/search_venues?lat=34.2347&lng=-77.9481&query=duck+dive
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "name": "Duck & Dive Pub",
    "lat": 34.234175252892,
    "lng": -77.947961431061,
    "distance": 59,
    "venue_id": "4ba29614f964a520ba0638e3",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Pub"
  },
  {
    "name": "Dig & Dive",
    "lat": 34.238095496413,
    "lng": -77.899629099616,
    "distance": 4476,
    "venue_id": "54f47e79498e08a96c858200",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/default_64.png",
    "category": "Comfort Food Restaurant"
  },
  {
    "name": "Ducks Sporting Goods",
    "lat": 34.213194444444,
    "lng": -77.887222222222,
    "distance": 6093,
    "venue_id": "4cf9375a951537042b626189",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/sports_outdoors_64.png",
    "category": "Sporting Goods Shop"
  },
  {
    ...
  },
  {
    "name": "The Dive",
    "lat": 34.262494015181,
    "lng": -77.884393520048,
    "distance": 6628,
    "venue_id": "4e7e7684b803c1522317898e",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  {
    "name": "At a pond feeding,ducks",
    "lat": 34.197162628174,
    "lng": -77.906967163086,
    "distance": 5638,
    "venue_id": "500c4074e4b07352bcdb260e",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/outdoors_64.png",
    "category": "Other Great Outdoors"
  }
]
```

Description: This endpoint will return up to 10 Foursquare results based
on the `:lat`, `:lng`, and `:query` parameters provided by the requesting client.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/search_venues?lat=00.00000&lng=00.00000&query=search+query+string+here`

### Query Parameters (*All are required*)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | nil | The requesting client's latitude coordinate
`lng` | nil | The requesting client's longitude coordinate
`query` | nil | A string containing the search query of the name of a Foursquare venue

## Foursquare Venues (Radius Search)

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/nearby_venues?lat=34.2347&lng=-77.9481&radius=3000
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "name": "Buzz's Roost",
    "lat": 34.23477,
    "lng": -77.948472,
    "distance": 35,
    "venue_id": "51394473e4b0668118640b27",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  {
    "name": "The Husk Seed Store And Bar",
    "lat": 34.234334508993,
    "lng": -77.94827937235,
    "distance": 43,
    "venue_id": "4e860502e5fab0fb114a3a48",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  {
    "name": "Untappd HQ - ILM",
    "lat": 34.235789629482,
    "lng": -77.947483062744,
    "distance": 133,
    "venue_id": "5759a613cd1040089032b492",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
    "category": "Tech Startup"
  },
  ...
  {
    "name": "Downtown Wilmington",
    "lat": 34.235575987706,
    "lng": -77.948658669509,
    "distance": 110,
    "venue_id": "5192e5dc498e754446937af3",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/neighborhood_64.png",
    "category": "Neighborhood"
  },
  {
    "name": "Next Glass",
    "lat": 34.2346553,
    "lng": -77.9482581,
    "distance": 15,
    "venue_id": "57dc3ca4498e856bf7b6f4a4",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  {
    "name": "Village Market",
    "lat": 34.234469895747,
    "lng": -77.947632332954,
    "distance": 50,
    "venue_id": "4b5c9c40f964a520ba3929e3",
    "icon": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/conveniencestore_64.png",
    "category": "Convenience Store"
  }
]
```

Description: This endpoint will return up to 10 Foursquare results based on the `:radius`
distance of the `:lat` & `:lng` parameters provided by the requesting client.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/nearby_venues?lat=00.00000&lng=00.00000&radius=0000`

### Query Parameters (*All are required*)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | nil | The requesting client's latitude coordinate
`lng` | nil | The requesting client's longitude coordinate
`radius` | nil | An integer value representing the radius distance (in meters) from the specified `:lat` & `:lng` values

# {TEMPLATE}

## {TEMPLATE: Title of the Endpoint}

> Send a POST request with required parameters:

```json
{
  "key": "value",
  "key": "value",
  "key": "value",
  "key": [
    {
      "key": "value"
    },
    {
      "key": "value"
    }
  ]
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "key": "value",
  "key": "value",
  "key": [
    {
      "key": "value"
    },
    {
      "key": "value"
    }
  ]
}
```

Description: {TEMPLATE}

### HTTP Request

`{HTTP_VERB} https://wildfire-staging.herokuapp.com/api/v1/{TEMPLATE}`

### Query Parameters (or additional details)

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
