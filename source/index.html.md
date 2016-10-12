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


## Register new User

> Send a POST request with the following parameters:

```json
{
  "email": "lauren@gmail.com",
  "first_name": "Lauren",
  "last_name": "Godwin",
  "dob": "1988-05-12",
  "gender": 2,
  "password": "s3cur3_p455w0rd",
  "password_confirmation": "s3cur3_p455w0rd",
  "nonce": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxfQ.po9twTrX99V7XgAk5mVskkiq8aa0lpYOue62ehubRY4"
}
```

> The above request returns JSON structured like this:

```json
{
  "auth_token": "Ac7qVZsS5Az7SVdgcrnEgXbf",
  "auth_token_expiry": "2016-10-05T16:16:54.827Z",
  "first_name": "Lauren",
  "uid": "26934fae-7b3a-4f72-b7cb-2469c94458cd",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoiMiIsImlhdCI6MTQ3NDkwMDMxNiwiZXhwIjoxNDc2MTA5OTE2LCJuY2UiOm51bGx9.pbHs3nk5IuIYCssA4XfcwKGFWM443MSXOeQhlAgXvMd3fQMO9OMpK6o9pBTju-LRfjXW-4mC7y6jhbSVfJ34KQ5HH7np8MQEO3HlmrpBSf4LBBDtox7GC2DzhYyo9uX-MgjJRKNwIH2Gv9qUE3oB9dYU2it_y4YR6Kw_Oe9Nd1TYuK6S-PFXnhsKEHdfVb0VlSBMYOvRYL6X8N-MaQyvbz__wVpJ55Y3QligFaV1of9DGgbTZLbbqbMAQFk8GnftTiIF2em3RFxKOMMItARGC-XEvXoEIgB1N6TvyJV-67cUtg1wvoCHvK2JsHFuSAA8or-oAHBlJ52Hm5nSNg8wmw"
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

```json
{
  "auth_token": "Ac7qVZsS5Az7SVdgcrnEgXbf",
  "auth_token_expiry": "2016-10-05T16:16:54.827Z",
  "first_name": "Lauren",
  "uid": "26934fae-7b3a-4f72-b7cb-2469c94458cd",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoiMiIsImlhdCI6MTQ3NDkwMDMxNiwiZXhwIjoxNDc2MTA5OTE2LCJuY2UiOm51bGx9.pbHs3nk5IuIYCssA4XfcwKGFWM443MSXOeQhlAgXvMd3fQMO9OMpK6o9pBTju-LRfjXW-4mC7y6jhbSVfJ34KQ5HH7np8MQEO3HlmrpBSf4LBBDtox7GC2DzhYyo9uX-MgjJRKNwIH2Gv9qUE3oB9dYU2it_y4YR6Kw_Oe9Nd1TYuK6S-PFXnhsKEHdfVb0VlSBMYOvRYL6X8N-MaQyvbz__wVpJ55Y3QligFaV1of9DGgbTZLbbqbMAQFk8GnftTiIF2em3RFxKOMMItARGC-XEvXoEIgB1N6TvyJV-67cUtg1wvoCHvK2JsHFuSAA8or-oAHBlJ52Hm5nSNg8wmw"
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

# Users

## Update User

> Using the user's auth Token within a Token Auth Header, send a PATCH request with the user params:

```json
{
  "about": "An updated bio from the client app!"
}
```

> A JSON response like the following would be returned:

```json
{
  "first_name": "Jordan",
  "auth_token": "cRLSSiGNKfykY9DdsJQonkTp",
  "auth_token_expiry": "2016-10-05T16:16:54.827Z",
  "identity_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6ImxheWVyLWVpdDt2PTEiLCJraWQiOiJsYXllcjovLy9rZXlzLzA5YTI1NDZhLTdmNDItMTFlNi1hMzk3LTAyZDM1NjAwMTJmMCJ9.eyJpc3MiOiJsYXllcjovLy9wcm92aWRlcnMvY2Y4ZDEzZTgtN2U5NS0xMWU2LTkyNGItYTE1MWU1MTI0NjI0IiwicHJuIjoiamVnMzIyNEBnbWFpbC5jb20iLCJpYXQiOjE0NzU2MDE1MzIsImV4cCI6MTQ3NjgxMTEzMiwibmNlIjpudWxsfQ.NN-AW7gpnklCoMKzZ_4MoXi0b6XJJQR1nZRXATd3M1nLe1VZk9NlIr-1hYbVRspeZOm4oZuN5HJslLMiYbEot5bm48I1OS0vqqwo64azZSVGTqmJl2GAm_lRizbb10Ic90YUdUPsrMeBfaq4B1yyGX03o2xSPQvDwBetGpqAJ_6oJkH_nDi0ZABLFtml1UHtUjKxOMU06-42r8L8FDTNOHWpw7bvB-hUa1T42OFO9cuDnPytu8yZeqEeS8MoXBY6-KUoI7qaJuaEI9G2IXYlodDZkAwW38Q_hWEp01umPSwB9551E5hApU2sJS2aih2A06tvP23yRt0r1JCI4w5pbQ",
  "email": "jeg3224@gmail.com",
  "dob": "1987-02-09",
  "gender": "male",
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

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

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
