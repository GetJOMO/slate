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

search: true
---

# Introduction

Welcome to the Wildfire API!

# Authentication

## Authentication Headers

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
This header (in addition to the `Client-key` header) is only required on the `login` & `token` endpoint requests.

### HTTP Token Auth Header
This header (in addition to the `Client-key` header) is required on all requests that
are not the `login` & `token` endpoint requests. Use the `token` endpoint to get an existing
user's `auth_token` for making further requests.

<aside class="notice">
You must use the LayerKit or Layer Android SDK in order to obtain the `nonce` used in the `token` endpoint request.
</aside>

# Users

Attribute | Type | Required/Optional
--------- | ------- | -----------
`id` | :integer | Required
`email` | :string | Required
`password` | :string | Required
`first_name` | :string | Required
`last_name` | :string | Required
`dob` | :date | Required
`gender` | :integer | Optional
`about` | :integer | Optional
`tags` | :array | Optional
`avatar_url` | :string | Optional
`auth_token` | :string | Required (Set by Server & used on all requests)
`auth_token_expires_at` | :datetime | Required (Set by Server)
<!-- `attribute` | :type | Required/Optional -->


## Register new User

> Send a POST request like so:

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

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/auth/email/sign-up`

## Authenticate a User via Email/Password

> Send a POST request like so:

```json

```

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/auth/email/sign-in`

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
