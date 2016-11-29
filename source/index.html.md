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
  "tags": [
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
  "tags": [
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
  "tags": [
    "Programming",
    "Duke Basketball",
    "Fishing",
    "Boating"
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

```shell
status: 200
```
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
  "avatar_url": "http://lorempixel.com/300/300/sports/#{Faker::Number.between(1, 9)}",
  "follow_status": false,
  "tags": [
    {
      "id": 14,
      "name": "Lakers",
      "parent_id": 3,
      "created_at": "2016-11-28T15:56:46.605Z",
      "updated_at": "2016-11-28T15:56:46.605Z"
    },
    {
      "id": 13,
      "name": "Jazz",
      "parent_id": 3,
      "created_at": "2016-11-28T15:56:46.591Z",
      "updated_at": "2016-11-28T15:56:46.591Z"
    },
    {
      "id": 1,
      "name": "Sports",
      "parent_id": null,
      "created_at": "2016-11-28T15:56:46.392Z",
      "updated_at": "2016-11-28T15:56:46.392Z"
    }
  ]
}
```

This endpoint will return the profile attributes for a specified user.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/profile`

## Retrieve User Followings

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/me/following
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

`GET https://wildfire-staging.herokuapp.com/api/v1/me/following`

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

## Add/Update User Profile Tags

> Send a PATCH request with required parameters:

```json
{
  "tag_ids": [1, 3, 5]
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
  "tags": [
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
  ]
}
```

This endpoint creates and/or updates a Tag(s) for the currently logged in user.

### HTTP Request

`PATCH https://wildfire-dev.herokuapp.com/api/v1/me/tags`

# Events

## Create Event

> Send a POST request with required parameters:

```json
{
  "description": "test event desc",
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "tags": [
    {
      "id": 6
    },
    {
      "id": 26
    },
    {
      "id": 160
    }
  ],
  "starts_at": "1988-05-12",
  "duration": 1.5,
  "venue_id": 37483747,
  "password": "12345678",
  "coords": {
    "lat": 34.4332,
    "lng": -77.8485
  }
}
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 9,
  "description": "test event desc",
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "tags": [
    {
      "id": "6",
      "title": "NFL"
    },
    {
      "id": "26",
      "title": "Carolina Panthers"
    },
    {
      "id": "160",
      "title": "Beer"
    }
  ],
  "starts_at": "1988-05-12T00:00:00.000Z",
  "duration": 1.5,
  "venue_id": 37483747,
  "coords": {
    "lat": 34.4332,
    "lng": -77.8485
  },
  "host": {
    "id": 1,
    "name": "Jordan Godwin",
    "dob": "1987-02-09",
    "tags": [
      "Programming",
      "Duke Basketball",
      "Fishing",
      "Boating"
    ]
  }
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
  "description": "test event desc",
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "starts_at": "1988-05-12T00:00:00.000Z",
  "duration": 1.5,
  "venue_id": 37483747,
  "host": {
    "id": 1,
    "name": "Jordan Godwin",
    "dob": "1987-02-09",
    "tags": [
      "Programming",
      "Duke Basketball",
      "Fishing",
      "Boating"
    ]
  },
  "tags": [
    {
      "id": "6",
      "title": "NFL"
    },
    {
      "id": "26",
      "title": "Carolina Panthers"
    },
    {
      "id": "160",
      "title": "Beer"
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
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "tags": [
    {
      "id": "6",
      "title": "NFL"
    },
    {
      "id": "26",
      "title": "Carolina Panthers"
    },
    {
      "id": "160",
      "title": "Beer"
    }
  ],
  "starts_at": "1988-05-12",
  "duration": 1.5,
  "venue_id": 37483747,
  "password": "12345678",
  "coords": {
    "lat": 34.4332,
    "lng": -77.8485
  }
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 9,
  "description": "Updated test event desc",
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "tags": [
    {
      "id": "6",
      "title": "NFL"
    },
    {
      "id": "26",
      "title": "Carolina Panthers"
    },
    {
      "id": "160",
      "title": "Beer"
    }
  ],
  "starts_at": "1988-05-12T00:00:00.000Z",
  "duration": 1.5,
  "venue_id": 37483747,
  "host": {
    "id": 1,
    "name": "Jordan Godwin",
    "dob": "1987-02-09",
    "tags": [
      "Programming",
      "Duke Basketball",
      "Fishing",
      "Boating"
    ]
  }
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

This endpoint will return an array of users who are are attending
the event.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/attendees`

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
