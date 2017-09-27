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

## Legend

> Sample Pagination Request

```shell
https://wildfire-staging.herokuapp.com/api/v1/events/55/comments?page=2&per_page=12
```

> Sample Pagination Response (headers)

```shell
HTTP/1.1 200 OK
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Link: <https://wildfire-staging.herokuapp.com/api/v1/events/55/comments?page=2&per_page=12>;
Per-Page: 12
Total: 50
Current-Page: 2
Content-Type: application/json; charset=utf-8
ETag: W/"44633c96f6521810dd8d72c80650e337"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: 64e2a61d-5cf2-40f1-b5c3-b5f3e9f14ff5
X-Runtime: 1.020325
Connection: close
Transfer-Encoding: chunked
```


### Pagination<a name="pagination"></a>
Endpoints that are paginated are denoted with `[Pg'd]` in their title. These Endpoints
accept the following additional parameters:

Parameter | Default | Description
--------- | ------- | -----------
`:page` | 1 | The page number to be returned
`:per_page` | 30 | The number of items to return on each _paged_ request

`NOTE: If neither of the above parameters are specified, the defaults will be used.`

Upon receiving a response from a paginated endpoint, each response will contain
the following headers:

Header | Description
--------- | -----------
`Total` | The total count of items for the specified endpoint
`Per-Page` | The number of items returned in the current request
`Current-Page` | The page number of the current request

# Authentication

## Authentication Header Types

> HTTP Request Headers:

```shell
'Client-key': 'UNIQUE_CLIENT_KEY_GOES_HERE'
```

```shell
'Authorization': 'Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImUyOGU2MDQwOTI4OTA4ZTE2YzNjMThlNTc2ZDg1Y2QxNmEyNjRlZWEifQ.eyJpc3MiOiJodHRwczovL3NlY3VyZXRva2VuLmdvb2dsZS5jb20vam9tby1iMTU5OCIsIm5hbWUiOiJOaWNob2xhcyBOZyIsInBpY3R1cmUiOiJodHRwczovL3Njb250ZW50Lnh4LmZiY2RuLm5ldC92L3QxLjAtMS9zMTAweDEwMC8xNDI5MjI5MF8xMDE1NTE3NjUzNTk0MzA5OF85NDE2NTkwMzUzNzU5MzY1ODJfbi5qcGc_b2g9ZDQ2NTA3NTRmODc4OGEwOWYwMDVhYzk3MGJiZGI3Yjgmb2U9NUEwOUU1NEQiLCJhdWQiOiJqb21vLWIxNTk4IiwiYXV0aF90aW1lIjoxNTAxNzgwNzk4LCJ1c2VyX2lkIjoiNU5tbFIxVGZId2dwNTBUV1loWXFJckF3cnlaMiIsInN1YiI6IjVObWxSMVRmSHdncDUwVFdZaFlxSXJBd3J5WjIiLCJpYXQiOjE1MDE3OTI5NjgsImV4cCI6MTUwMTc5NjU2OCwiZW1haWwiOiJuZ2hldW5neXVAZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJmaXJlYmFzZSI6eyJpZGVudGl0aWVzIjp7ImZhY2Vib29rLmNvbSI6WyIxMDE1NjM0MzY1MzI1ODA5OCJdLCJlbWFpbCI6WyJuZ2hldW5neXVAZ21haWwuY29tIl19LCJzaWduX2luX3Byb3ZpZGVyIjoiZmFjZWJvb2suY29tIn19.N7DSP3KqYvCvJ21JzjAd0p9b41b0Skp6pKWVzJ3iioKd5_Q9rGoqkp_X01sRJfmjnSNW817Sedba3DJROxAJwXmHqvUF_o59tPx_ns7TvBfqFE70Tq3pF0EpJi0oZyT1a5ICnEVnjuKCZYooNkpE0rUI4gHZUJolXxxB0xFDVNHHbLeT9hNaq5MdSHQKKJ3rHuAqmmOYJAr9IpMKDCMlCyUxcCrzzEDs-vRQPbthoyg9j3arKfAgO0XSB-00agcStB54SYg7yYoN9Ly4QKZQT5Lrz8hjlLJGV8_M_MB4C2sO4BVphpUdUlH8GBw4OC44LaPc5qDcRZwDHV51MvvXyA'
```

Wildfire uses multiple HTTP headers to authenticate API requests. The three different HTTP headers & their use cases for
each are described below.

### _Client-key_ Header (All requests)
The first is a `Client-key` header that is an unique API key which is checked for in *ALL* requests to the Wildfire API.
This API key can be obtained from an API administrator.

### Firebase Auth Token
This header (in addition to the `Client-key` header) is required on all requests. The token is
a JSON Web Token that is provided by Firebase and is then verified by the Wildfire Server API.

<aside class="notice">
OPTIONAL: You must use the LayerKit or Layer Android SDK in order to obtain the `nonce` used in the `token` endpoint request.
</aside>

## Verify User

> Send a GET request with the email to be verified:

```shell
https://wildfire-staging.herokuapp.com/api/v1/auth/verify_user
```

> If the user exists, the following JSON response will be returned:

```shell
status: 200
```

This endpoint is used during the on-boarding process to verify that if a user exists.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/auth/verify_user`

## Register User via Facebook

> Send a POST request with the following parameters:

```json
{
  "first_name": "Aaron",
  "last_name": "Rodgers",
  "gender": 1,
  "city": "Green Bay",
  "state": "WI",
  "avatar_url": "https://nflprofile.com/aaron_rodgers",
  "coords": {
    "lat": "44.5192",
    "lng": "88.0198"
  },
  "avatar_url": "https://fake.urlto.img/user_avatar",
  "firebase_id": "937849239748230",
  "facebook_id": "324849239748230"
}
```

> The above request returns the following JSON:

```shell
status: 201
```
```json
{
  "id": 612,
  "email": "greenbay4lyfe@gmail.com",
  "first_name": "Aaron",
  "last_name": "Rodgers",
  "dob": "",
  "about": "",
  "city": "Green Bay",
  "state": "WI",
  "coords": {
    "lat": 44.5192,
    "lng": 88.0198
  },
  "push_notifs": {
    "general": true,
    "messages": true,
    "event_updates": true,
    "mentions": true,
    "event_comments": true,
    "user_follows": true,
    "join_requests": true
  },
  "avatar_url": "https:\/\/nflprofile.com\/aaron_rodgers",
  "followers_count": 0,
  "attended_count": 0,
  "checkins_count": 0,
  "hosted_count": 0,
  "following_users_count": 0,
  "following_venues_count": 0,
  "last_login_time": "2017-06-23T14:24:30.905Z",
  "firebase_id": "937849239748230",
  "facebook_id": "324849239748230"
}
```

Description: Used to register a new client user from Facebook Ouath via Firebase.

### HTTP Request

`POST https://wildfire-dev.herokuapp.com/api/v1/users/register`

<aside class="notice">
NOTE: The `nonce` parameter is a JWT obtained from Layer using the Layer iOS or Android SDK. The server uses this
to create an `identity_token` for the client to use on future Layer requests.
</aside>

## Logout the current User

> Along with the Token Auth Header, send a PATCH request to the specified URI:

```shell
https://wildfire-dev.herokuapp.com/api/v1/me/logout
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

```shell
https://wildfire-dev.herokuapp.com/api/v1/auth/refresh_identity_token
```

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

## Notifications [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/me/notifications?per_page=20
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 4893,
    "actor": {
      "id": 374,
      "first_name": "Kieran",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8"
    },
    "action": 0,
    "text": "Kieran joined your event",
    "data": {
      "event": {
        "id": 1,
        "attendee_count": 2
      }
    },
    "created_at": "2017-05-02T16:45:50.962Z"
  },
  {
    "id": 1205,
    "actor": {
      "id": 6,
      "first_name": "Nicholas",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/3"
    },
    "action": 3,
    "text": "Nicholas commented on an event you're attending",
    "data": {
      "event": {
        "id": 89,
        "description": "Event 30: Nicholas' fun event for <a class=\"user\" id=\"1\">Jordan Godwin<\/a>, <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>."
      },
      "comment": {
        "id": 847,
        "body": "This sounds really fun!!"
      },
    },
    "created_at": "2017-05-02T15:52:10.758Z"
  }
  {
    "id": 4892,
    "actor": {
      "id": 354,
      "first_name": "Lemuel",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/4"
    },
    "action": 2,
    "text": "Lemuel is now following you",
    "data": "{}",
    "created_at": "2017-05-02T16:43:08.987Z"
  },
  {
    "id": 1727,
    "actor": {
      "id": 7,
      "first_name": "Mike",
      "avatar_url": "https:\/\/s3.amazonaws.com\/jomo-dev-us-east-1\/uploads\/60aa27c2-8547-4bdc-aba2-247c3426e7a4.png"
    },
    "action": 4,
    "text": "Mike mentioned you in an event",
    "data": {
      "event": {
        "id": 41,
        "description": "Event 41: Nicholas' fun event for <a class=\"user\" id=\"1\">Jordan Godwin<\/a>, <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>."
      }
    },
    "created_at": "2017-05-02T16:27:34.757Z"
  },
  {
    "id": 1205,
    "actor": {
      "id": 6,
      "first_name": "Nicholas",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/3"
    },
    "action": 5,
    "text": "Nicholas mentioned you in a comment",
    "data": {
      "event": {
        "id": 30,
        "description": "Event 30: Nicholas' fun event for <a class=\"user\" id=\"1\">Jordan Godwin<\/a>, <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>."
      },
      "comment": {
        "id": 283,
        "body": "I'll be there <a class=\"user\" id=\"1\">Jordan Godwin<\/a>!"
      },
    },
    "created_at": "2017-05-02T15:52:10.758Z"
  }
]  
```

Description: This endpoint will return a paginated array of notification objects,
ordered by the most recently received notification.

**Action Types:**
```
{
  joined: 0,
  followed: 1,
  commented: 2,
  event_mention: 3,
  comment_mention: 4,
  event_updated: 5,
  new_badge: 6,
  new_checkin: 7,
  join_request: 8,
  accepted_request: 9,
  general: 10
}
```

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/me/notifications`

## Update Device Badge Count

> Send a PATCH request with any of the user object's attributes:

```json
{
  "device_badge_count": 1
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "device_badge_count": 1
}
```

Description: Updates the current user's `device_badge_count` attribute

### HTTP Request

`PATCH https://wildfire-staging.herokuapp.com/api/v1/me/device_badge_count`

## Update User

> Send a PATCH request with any of the user object's attributes:

```json
{
  "about": "An updated bio from the client app!",
  "start_date_range": 2,
  "coords": {
    "lat": "34.4343",
    "lng": "-77.5353"
  }
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
  "id": 2,
  "first_name": "Kerry",
  "last_name": "Knight",
  "dob": "1980-11-17",
  "about": "about kerry knight",
  "city": "Wilmington",
  "state": "NC",
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/5",
  "coords": {
    "lat": 34.2253,
    "lng": -77.93786
  },
  "start_date_range": 2,
  "followers_count": 16,
  "following_users_count": 65,
  "following_venues_count": 15,
  "attended_count": 15,
  "hosted_count": 10,
  "checkins_count": 3,
  "block_status": false,
  "follow_status": false,
  "reverse_block_status": false,
  "reverse_follow_status": false,
  "firebase_id": "937849239748230",
  "facebook_id": "324849239748230",
  "tags": [
    {
      "id": 516,
      "name": "AUB Tigers Football",
      "parent_id": 510,
    },
    {
      "id": 123,
      "name": "UCI Anteaters Basketball",
      "parent_id": 121
    }
    {
      "id": 585,
      "name": "Dallas Cowboys",
      "parent_id": 568
    }
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


### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/me/deactivate`

## Find Facebook Friends Using JOMO

> Along with the Token Auth Header, send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/me/find_friends
```

```json
{
  "token": "Ac7qVZsS5Az7SVdgcrnEgXbf23sdfajidafasdikvmj823"
}
```

> The server will respond with ONE of the following JSON reponses:

```shell
status: 200
```

```json
[
  {
    "id": 1,
    "first_name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/9",
    "facebook_id": "794663274565",
    "block_status": false,
    "follow_status": false,
    "reverse_block_status": false,
    "reverse_follow_status": false
  },
  {
    "id": 1,
    "first_name": "Lauren",
    "last_name": "Godwin",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/clothing\/9",
    "facebook_id": "4663272379245237",
    "block_status": false,
    "follow_status": false,
    "reverse_block_status": false,
    "reverse_follow_status": false
  }
]
```

Description: Use this endpoint to pass the facebook token for the current_user to have an
array of users returned that are friends on facebook and also using the JOMO app.


### HTTP Request

`GET https://wildfire-dev.herokuapp.com/api/v1/me/find_friends?token=EAAZAvtcsxhLMBADwMYG1RnD4rGFdZA2XcS9B0JK4E9ZAlXsuHZBJSLyvF433mGUiJtBvjw7WJ4sh7JO7uvqeww5nPCUWZAgn1z6QfvqZCl5xWIlmUtqL3Y530WXnB4rmvZC1vuHUZCAsRnc74AWvqRaYeHgKOH74uOKsOUrwSu6TL0YadOfpjNidctcZCWgQa5rgZD`

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
  "coords": {
    "lat": 34.2257,
    "lng": 77.9447
  },
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/7",
  "start_date_range": 0,
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "followers_count": 16,
  "following_users_count": 102,
  "following_venues_count": 3,
  "attended_count": 5,
  "hosted_count": 10,
  "block_status": false,
  "follow_status": false,
  "profile_tags": [],
  "events": [
    {
      "id": 29,
      "description": "Event 29: Kerry's Super Bowl party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
      "media": {
        "media_url": "http://lorempixel.com/600/338/sports/3",
        "media_thumb": "http://lorempixel.com/300/169/sports/9",
        "media_type": 0,
        "width": 600,
        "height": 338,
        "thumb_width": 300,
        "thumb_height": 169
      },
      "starts_at": "2017-02-13T21:06:00.000Z",
      "ends_at": "2017-02-13T21:06:03.930Z",
      "status": 0,
      "event_privacy": 0,
      "comments_count": 3,
      "attendee_count": 0,
      "attendee_status": 0,
      "notifications_status": false,
      "host": {
        "id": 2,
        "first_name": "Kerry",
        "last_name": "Knight",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/7",
        "block_status": false,
        "follow_status": false,
        "profile_tags": []
      },
      "venue": {
        "id": 2,
        "name": "Duck & Dive Pub",
        "city": "Wilmington",
        "state": "NC",
        "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
        "coords": {
          "lat": 34.234175252892,
          "lng": -77.947961431061
        },
        "follow_status": true,
        "category": "Pub",
        "foursquare_id": "4bf58dd8d48988d11b941735"
      },
      "tags": []
    },
    {
      ...
    }
  ]
}
```

This endpoint will return the profile attributes for a specified user.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/profile`

## Retrieve User Followers [[Pg'd](#pagination)]

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

## Retrieve User Followings [[Pg'd](#pagination)]

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/following/users
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
specified user is currently following.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/following/users`

## Retrieve Venue Followings [[Pg'd](#pagination)]

> Send a GET request to the specified URI:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/3/following/venues
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 1,
    "name": "Untappd HQ - ILM",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
    "coords": {
      "lat": 34.235789629482,
      "lng": -77.947483062744
    },
    "category": "Tech Startup",
    "foursquare_id": "5759a613cd1040089032b492"
  },
  {
    "id": 2,
    "name": "Duck & Dive Pub",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "coords": {
      "lat": 34.234175252892,
      "lng": -77.947961431061
    },
    "category": "Pub",
    "foursquare_id": "4bf58dd8d48988d11b941735"
  },
  {
    "id": 3,
    "name": "24 South Coffee House",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/coffeeshop_64.png",
    "coords": {
      "lat": 34.234456599111,
      "lng": -77.948524800075
    },
    "category": "Coffee Shop",
    "foursquare_id": "53fa55f3498ed31bb942100a"
  },
  {
    "id": 4,
    "name": "Masonboro Inlet",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/beach_64.png",
    "coords": {
      "lat": 34.182547362367,
      "lng": -77.81902944261
    },
    "category": "Beach",
    "foursquare_id": "4c30a001a0ced13a3c61126e"
  }
]
```

Description: This endpoint will return an array of venues that the
specified user is currently following.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/following/venues`

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

## Retrieve Blocked Users [[Pg'd](#pagination)]

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
    "block_status": true,
    "report_status": -1
  },
  {
    "id": 5,
    "name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_5",
    "block_status": true,
    "report_status": -1
  },
  {
    "id": 12,
    "name": "Gavin",
    "last_name": "Anthony",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_12",
    "block_status": true,
    "report_status": -1
  }
  ,{
    "id": 23,
    "name": "Kerry",
    "last_name": "Knight",
    "avatar_url": "https:\/\/fake.urlto.img\/user_avatar_23",
    "block_status": true,
    "report_status": -1
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
  "block_status": true,
  "report_status": -1
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
  "block_status": true,
  "report_status": -1
}
```

Description: Use this endpoint to unblock a target user (remove a block)
for the current_user with the target of another user.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/users/:id/unblock`

## Report a User

> Send a POST request to the specified URI with the required params:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/1/report
```
```json
{
  "report_status": 2
}
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 1,
  "first_name": "Jordan",
  "last_name": "Godwin",
  "avatar_url": "https://fake.urlto.img/user_avatar_3",
  "block_status": true,
  "report_status": 2
}
```

Description: Use this endpoint to report a target user (create a block w/ report_status)
for the current_user with the target of another user.

Report Status Enum values: `{ spam: 0, messages: 1, media: 2, bullying: 3, other: 4 }`

*Note:* By reporting a user, you are essentially blocking the user and
including a report status. Therefore, use the _Unblock User_ endpoint
to remove the block with report status.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/users/:id/report`

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

## Hosted Events for User [[Pg'd](#pagination)]

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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/9",
      "media_thumb": "http://lorempixel.com/300/169/sports/5",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_data": {
      "me": {
        "user_id": 1,
        "attendee_status": 2,
        "mute_notifications": false
      },
      "total_attendee_count": 42,
      "follows_attendee_count": 7,
      "users_data": [
        {
          "user_id": 17,
          "last_name": "Larson",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
          "first_name": "Clement",
          "follow_status": true
        },
        {
          "user_id": 89,
          "last_name": "Effertz",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/5",
          "first_name": "Newell",
          "follow_status": true
        },
        {
          "user_id": 389,
          "last_name": "White",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
          "first_name": "Albina",
          "follow_status": true
        },
        {
          "user_id": 375,
          "last_name": "Pollich",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
          "first_name": "Cindy",
          "follow_status": true
        }
      ]
    },
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_data": {
      "me": {
        "user_id": 2,
        "attendee_status": 2,
        "mute_notifications": false
      },
      "total_attendee_count": 42,
      "follows_attendee_count": 7,
      "users_data": [
        {
          "user_id": 17,
          "last_name": "Larson",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
          "first_name": "Clement",
          "follow_status": true
        },
        {
          "user_id": 89,
          "last_name": "Effertz",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/5",
          "first_name": "Newell",
          "follow_status": true
        },
        {
          "user_id": 389,
          "last_name": "White",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
          "first_name": "Albina",
          "follow_status": true
        },
        {
          "user_id": 375,
          "last_name": "Pollich",
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
          "first_name": "Cindy",
          "follow_status": true
        }
      ]
    },
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": true,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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

## Joined Events for User [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/276/attendants/joins
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 1,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": false,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 4,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": false,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 1,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": true,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/attendants/joins`

## Event Join Requests for User [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/57/attendants/requests
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 4,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": true,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 2,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
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
      "id": 32,
      "name": "New Venue",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": false,
      "category": "Beach Bar",
      "foursquare_id": "fsdf37fds483df747af"
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

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:id/attendants/requests`

## All Join Requests for Hosted Events [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/users/23/events/requests
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 7,
    "first_name": "Roy",
    "last_name": "Bradtke",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
    "event_id": 38,
    "description": "Event 38: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:39.258Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 11,
    "first_name": "Abel",
    "last_name": "Berge",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/9",
    "event_id": 26,
    "description": "Event 26: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:31.896Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 13,
    "first_name": "Juvenal",
    "last_name": "Graham",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 32,
    "description": "Event 32: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:35.553Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 18,
    "first_name": "Hassie",
    "last_name": "Medhurst",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.297Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 19,
    "first_name": "Nicholaus",
    "last_name": "Weimann",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 38,
    "description": "Event 38: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:39.037Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 29,
    "first_name": "Cara",
    "last_name": "Hermann",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/5",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.287Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 44,
    "first_name": "Gunnar",
    "last_name": "West",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/1",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.347Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  }
]
```

This endpoint will return an array of users who have requested
to join *any* of the specified user's active or future events. These are ordered
by the `requested_at` attribute beginning with the most recent.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:user_id/events/requests`

## Toggle Event Notifications

> Send a PATCH request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/events/33/attendees/notifications
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 33,
  "description": "Exercitationem doloremque eaque ea dolor qui.",
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 0,
  "attendee_count": 4,
  "status": 0,
  "event_privacy": 0,
  "attendee_status": 0,
  "notifications_status": false,
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
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": true,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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
}
```

This endpoint will toggle the `notifications_status` on this event for current user,
thus disabling or enabling notifications for the specified event.

### HTTP Request

`PATCH https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/attendees/notifications`

## Tag/Mention User<a name="mention_user"></a> [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/users/search?q=Joh
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 67,
    "first_name": "Kyra",
    "last_name": "Johnston",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/9",
    "city": "Marielaburgh",
    "state": "TN",
    "follow_status": false
  },
  {
    "id": 276,
    "first_name": "Lorenz",
    "last_name": "Johnson",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
    "city": "Dickensport",
    "state": "NE",
    "follow_status": false
  },
  {
    "id": 460,
    "first_name": "Heather",
    "last_name": "Johns",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/1",
    "city": "North Florineburgh",
    "state": "MS",
    "follow_status": false
  },
  {
    "id": 471,
    "first_name": "Johann",
    "last_name": "Ziemann",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/4",
    "city": "Binston",
    "state": "KY",
    "follow_status": false
  }
]
```

This endpoint will return an array of users where their first or
last name match part of the search query.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/search`

### Query Parameters (*required*)

Parameter | Default | Description
--------- | ------- | -----------
`q` | nil | Used to search for users by their name (first or last name)
`tag_id` | nil | Used to search for users by the specified Tag

## Get User Checkins

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/users/11/checkins
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "body": "Made it to the event!",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 32,
    "venue_id": 232,
    "created_at": "2017-05-31T18:20:00.651Z",
    "user": {
      "id": 11,
      "first_name": "Gay",
      "last_name": "Watsica",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null,
          "created_at": "2017-05-24T18:52:48.104Z",
          "updated_at": "2017-05-24T18:52:48.104Z"
        },
        {
          "id": 245,
          "name": "WRST Raiders Basketball",
          "parent_id": 241,
          "created_at": "2017-05-24T18:52:43.576Z",
          "updated_at": "2017-05-24T18:52:43.576Z"
        }
      ]
    }
  },
  {
    ...
  },
  {
    "id": 4,
    "body": "",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 32,
    "venue_id": 232,
    "created_at": "2017-05-31T18:53:40.773Z",
    "user": {
      "id": 11,
      "first_name": "Gay",
      "last_name": "Watsica",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null,
          "created_at": "2017-05-24T18:52:48.104Z",
          "updated_at": "2017-05-24T18:52:48.104Z"
        },
        {
          "id": 245,
          "name": "WRST Raiders Basketball",
          "parent_id": 241,
          "created_at": "2017-05-24T18:52:43.576Z",
          "updated_at": "2017-05-24T18:52:43.576Z"
        }
      ]
    }
  }
]
```

This endpoint will retrieve all checkin's for the specified user.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/users/:user_id/checkins`

# Events

## Create Event

> Send a POST request with required parameters:

```json
{
  "description": "My First Event with <a class=\"user\" id="6">Nicholas Ng</a>!",
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "tag_ids": [3, 1, 11],
  "starts_at": "2017-01-10",
  "duration": 3,
  "event_privacy": 2,
  "mention_ids": [6],
  "venue": {
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https://ss0.4sqi.net/img/categories_v2/nightlife/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": false,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 0,
  "attendee_count": 0,
  "status": 0,
  "event_privacy": 0,
  "attendee_status": 0,
  "notifications_status": false,
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
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": false,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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

Description: This endpoint creates an Event for the currently logged in user.

*NOTE:* include the `mention_ids` with an array of integers within the JSON object when tagging users.

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
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 0,
  "attendee_count": 1,
  "status": 0,
  "event_privacy": 0,
  "attendee_status": 0,
  "notifications_status": false,
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
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": true,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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
      "user_id": 1,
      "created_at": "2017-02-06T22:40:59.502Z"
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
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 1,
  "attendee_count": 1,
  "status": 0,
  "event_privacy": 0,
  "attendee_status": 0,
  "notifications_status": false,
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
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": true,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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
      "user_id": 1,
      "created_at": "2017-02-06T22:40:59.502Z"
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

## Join/Request to Join an Event

> Send a POST request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/15/joins
```

> A JSON response like the following would be returned:

```shell
status: 201
```
```json
{
  "id": 2339,
  "user_id": 17,
  "first_name": "Jamal",
  "last_name": "Schinner",
  "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
  "event_id": 39,
  "description": "Event 39: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
  "ends_at": "2017-05-19T00:00:00.000Z",
  "requested_at": "2017-05-15T14:56:55.114Z",
  "attendee_status": 1
}
```

This endpoint, depending on the state of the event's `:privacy` attribute,
will create an Attendant record with either an `:attending` or `:requested`
status; `attending` status when event is public and `requested` when event is private.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/joins`

## Remove a Join an Event

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/15/joins/321
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 15,
  "description": "Et rerum vero eveniet excepturi.",
  "media": {
    "media_url": "http://lorempixel.com/600/338/sports/3",
    "media_thumb": "http://lorempixel.com/300/169/sports/9",
    "media_type": 0,
    "width": 600,
    "height": 338,
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 0,
  "attendee_count": 1,
  "status": 0,
  "event_privacy": 0,
  "attendee_status": 0,
  "notifications_status": false,
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
    "id": 32,
    "name": "New Venue",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "follow_status": false,
    "category": "Beach Bar",
    "foursquare_id": "fsdf37fds483df747af"
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
      "user_id": 1,
      "created_at": "2017-02-06T22:40:59.502Z"
    }
  ]
}
```

This endpoint will _destroy_ a previously created 'join' (Attendant record w/ an `:attending` status)
from the specified event.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/joins/:id`

## Retrieve Joins (Attendees) for Event [[Pg'd](#pagination)]

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

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/joins`

## Retrieve Join Requests for Event [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/327/requests
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 7,
    "first_name": "Roy",
    "last_name": "Bradtke",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2",
    "event_id": 38,
    "description": "Event 38: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:39.258Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 11,
    "first_name": "Abel",
    "last_name": "Berge",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/9",
    "event_id": 26,
    "description": "Event 26: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:31.896Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 13,
    "first_name": "Juvenal",
    "last_name": "Graham",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 32,
    "description": "Event 32: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:35.553Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 18,
    "first_name": "Hassie",
    "last_name": "Medhurst",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.297Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 19,
    "first_name": "Nicholaus",
    "last_name": "Weimann",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
    "event_id": 38,
    "description": "Event 38: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:39.037Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 29,
    "first_name": "Cara",
    "last_name": "Hermann",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/5",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.287Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  },
  {
    "id": 44,
    "first_name": "Gunnar",
    "last_name": "West",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/1",
    "event_id": 14,
    "description": "Event 14: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "requested_at": "2017-03-02T21:56:24.347Z",
    "attendee_status": 1,
    "block_status": false,
    "follow_status": false
  }
]
```

This endpoint will return an array of users who have _requested_
to join the specified event.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/requests`

## Approve an Event Join Request

> Send a PATCH request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/38/requests/324
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 7,
  "user_id": 324,
  "event_id": 38,
  "attendee_status": 2,
}
```

This endpoint will update a given user's join request from
`requested` to `joined` (`1` => `2`). The returned response
is a single JSON object that includes the updated attendee_status.

### HTTP Request

`PATCH https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/requests/:user_id`

## Ignore a Join Request

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/32/requests/324
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "head": "no_content"
}
```

This endpoint will _destroy_ a previously created 'request' (Attendant record w/ a `:requested` status)
from the specified event.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/requests/:user_id`

## Checkin to an Event

> Send a POST request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/96/checkin
```

```json
{
  "body": "Made it to the event!",
  "media": {
    "media_url": "test.com/img",
    "media_thumb": "test.com/img_thumb",
    "media_type": 0,
    "height": 300,
    "width": 300,
    "thumb_height": 100,
    "thumb_width": 100
  }
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 74,
  "body": "Yeah, I made it too! Awesome scene so far!!",
  "media": {
    "media_url": "test.com\/img",
    "media_thumb": "test.com\/img_thumb",
    "media_type": 0,
    "height": 300,
    "width": 300,
    "thumb_height": 100,
    "thumb_width": 100
  },
  "event_id": 96,
  "venue_id": 25,
  "created_at": "2017-06-13T14:06:54.003Z",
  "user": {
    "id": 2,
    "first_name": "Kerry",
    "last_name": "Knight",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/8",
    "profile_tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null,
        "created_at": "2017-05-24T18:52:41.614Z",
        "updated_at": "2017-05-24T18:52:41.614Z"
      },
      {
        "id": 29,
        "name": "SEMO Redhawks Basketball",
        "parent_id": 26,
        "created_at": "2017-05-24T18:52:40.994Z",
        "updated_at": "2017-05-24T18:52:40.994Z"
      },
      {
        "id": 549,
        "name": "Cleveland Cavaliers",
        "parent_id": 537,
        "created_at": "2017-05-24T18:52:47.505Z",
        "updated_at": "2017-05-24T18:52:47.505Z"
      }
    ]
  }
}
```

This endpoint will allow an Attendee with a `:joined` status to create a photo
check-in to an event. Since the Checkin is a 'Photo Checkin', the `:media` attribute is required.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/checkin`

### Request Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
`media` | nil | A media object just like Events or Comments **(Required)**
`body` | nil | A string of text content **(Optional)**

## Delete an Event Checkin

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/96/checkins/2
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 96,
  "description": "Sunset cruise on the sailboat. Request to join please!",
  "media": {
    "width": 600,
    "height": 338,
    "media_url":
    "http://lorempixel.com/600/338/sports/\#{Faker::Number.between(1, 9)}",
    "media_type": 0,
    "media_thumb":
    "http://lorempixel.com/300/169/sports/\#{Faker::Number.between(1, 9)}",
    "thumb_width": 300,
    "thumb_height": 169
  },
  "comments_count": 0,
  "event_privacy": 1,
  "starts_at": "2017-06-02T20:53:46.000Z",
  "ends_at": "2017-06-02T22:53:46.000Z",
  "duration": 2.0,
  "event_status": 0,
  "attendee_data": {
    "me": {
      "user_id": 343,
      "attendee_status": 2,
      "mute_notifications": false
    },
    "total_attendee_count": 2,
    "follows_attendee_count": 1,
    "users_data": [
      {
        "user_id": 2,
        "last_name": "Anthony",
        "avatar_url": "https://fake.urlto.img/user_avatar_gavin",
        "first_name": "Gavin",
        "follow_status": true
      },
      {
        "user_id": 6,
        "last_name": "Richman",
        "avatar_url": "https://fake.urlto.img/user_avatar",
        "first_name": "Nick",
        "follow_status": false
      }
    ]
  },
  "host": {
    "id": 343,
    "first_name": "Annamae",
    "last_name": "Williamson",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
    "profile_tags": [
      {
        "id": 605,
        "name": "Calgary Flames",
        "parent_id": 601,
        "created_at": "2017-06-08T15:58:37.404Z",
        "updated_at": "2017-06-08T15:58:37.404Z"
      }
    ]
  },
  "venue": {
    "id": 491,
    "name": "Wrightsville Beach",
    "city": "Wrightsville Beach",
    "state": "NC",
    "icon_url": "https://ss3.4sqi.net/img/categories_v2/parks_outdoors/beach_64.png",
    "coords": { "lat": 34.2085, "lng": 77.7964 },
    "category": "Beach",
    "follow_status": false,
    "foursquare_id": "4b9c5278f964a5205a5f36e3"
  }
}
```

This endpoint will allow an Attendee with a `:checked_in` status to delete their event checkin record.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/checkins/:user_id`

## Get Event Checkins

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/32/checkins
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "body": "Made it to the event!",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 32,
    "venue_id": 232,
    "created_at": "2017-05-31T18:20:00.651Z",
    "user": {
      "id": 11,
      "first_name": "Gay",
      "last_name": "Watsica",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null,
          "created_at": "2017-05-24T18:52:48.104Z",
          "updated_at": "2017-05-24T18:52:48.104Z"
        },
        {
          "id": 245,
          "name": "WRST Raiders Basketball",
          "parent_id": 241,
          "created_at": "2017-05-24T18:52:43.576Z",
          "updated_at": "2017-05-24T18:52:43.576Z"
        }
      ]
    }
  },
  {
    "id": 4,
    "body": "",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 32,
    "venue_id": 232,
    "created_at": "2017-05-31T18:53:40.773Z",
    "user": {
      "id": 13,
      "first_name": "Aubree",
      "last_name": "Pagac",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
      "block_status": false,
      "follow_status": false,
      "reverse_block_status": false,
      "reverse_follow_status": false,
      "profile_tags": [
        {
          "id": 87,
          "name": "IDHO Vandals Basketball",
          "parent_id": 75,
          "created_at": "2017-05-24T18:52:41.614Z",
          "updated_at": "2017-05-24T18:52:41.614Z"
        },
        {
          "id": 29,
          "name": "SEMO Redhawks Basketball",
          "parent_id": 26,
          "created_at": "2017-05-24T18:52:40.994Z",
          "updated_at": "2017-05-24T18:52:40.994Z"
        }
      ]
    }
  }
]
```

This endpoint will retrieve all checkin's for the specified event.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/checkins`

## Create Event Comment

> Send a POST request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22/comments
```
```json
{
  "body": "See you there <a class=\"user\" id="6">Nicholas Ng</a> and <a class=\"user\" id="4">George Taylor</a>! I'll bring the beer!!",
  "media": {
    "media_url": "test.com/img",
    "media_thumb": "test.com/img_thumb",
    "media_type": 0,
    "height": 300,
    "width": 300,
    "thumb_height": 100,
    "thumb_width": 100
  }
  "mention_ids": [6, 4]
}
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 2447,
  "body": "See you there <a class=\"user\" id="6">Nicholas Ng</a> and <a class=\"user\" id="4">George Taylor</a>! I'll bring the beer!!",
  "event_id": 12,
  "created_at": "2017-02-15T16:17:29.565Z",
  "media": {
    "media_url": "test.com/img",
    "media_thumb": "test.com/img_thumb",
    "media_type": 0,
    "height": 300,
    "width": 300,
    "thumb_height": 100,
    "thumb_width": 100
  },
  "user": {
    "id": 1,
    "first_name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "http://lorempixel.com/300/300/sports/1",
    "profile_tags": []
  }
}
```

Description: This endpoint will create a comment for the specified event.

*NOTE:* include the `mention_ids` with an array of integers within the JSON object when tagging users.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/comments`

## Delete Event Comment

> Send a DELETE request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22/comments/141
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "head": "no_content"
}
```

Description: This endpoint will delete a specified comment for the specified event.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/comments`

## Show Event Comments [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/events/22/comments
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 5,
    "body": "Eveniet id officiis libero architecto quia.",
    "event_id": 12,
    "created_at": "2017-02-06T22:30:59.502Z",
    "user": {
      "id": 74,
      "first_name": "Eulalia",
      "last_name": "Ritchie",
      "avatar_url": "http://lorempixel.com/300/300/cats/6",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 135,
          "name": "MRSH Thundering Herd Basketball",
          "parent_id": 131
        },
        {
          "id": 284,
          "name": "TOL Rockets Basketball",
          "parent_id": 279
        }
      ]
    }
  },
  {
    "id": 6,
    "body": "Ducimus sed sunt voluptatem laborum dolores.",
    "event_id": 12,
    "created_at": "2017-02-06T22:33:59.502Z",
    "user": {
      "id": 63,
      "first_name": "Alena",
      "last_name": "Batz",
      "avatar_url": "http://lorempixel.com/300/300/cats/2",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 141,
          "name": "UTSA Roadrunners Basketball",
          "parent_id": 131
        },
        {
          "id": 329,
          "name": "BRWN Bears Basketball",
          "parent_id": 328
        },
        {
          "id": 379,
          "name": "LAS Explorers Basketball",
          "parent_id": 370
        }
      ]
    }
  },
  {
    "id": 7,
    "body": "Quisquam ipsam quia corporis occaecati ex.",
    "event_id": 12,
    "created_at": "2017-02-06T22:40:59.502Z",
    "user": {
      "id": 29,
      "first_name": "Everardo",
      "last_name": "Leffler",
      "avatar_url": "http://lorempixel.com/300/300/cats/7",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 169,
          "name": "SWAC Basketball",
          "parent_id": 12
        },
        {
          "id": 217,
          "name": "LIP Bisons Basketball",
          "parent_id": 210
        },
        {
          "id": 327,
          "name": "SFNY Terriers Basketball",
          "parent_id": 317
        }
      ]
    }
  }
]
```

Description: This endpoint will return an array of comments for the specified event in ascending order by `:created_at`.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/:event_id/comments`

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

`GET https://wildfire-staging.herokuapp.com/api/v1/tags`

# Venues

## Show Venue

> Send a GET request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/5759a613cd1040089032b492`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 1,
  "name": "Untappd HQ - ILM",
  "city": "Wilmington",
  "state": "NC",
  "coords": {
    "lat": 34.235789629482,
    "lng": -77.947483062744
  },
  "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
  "foursquare_id": "5759a613cd1040089032b492",
  "category": "Tech Startup",
  "data": {
    "phone": "9102924336",
    "hours": {
      "status": "Open til 5pm",
      "is_open": true
    },
    "address": "21 S Front St",
    "zip_code": "28401",
    "web_url": "http:\/\/untappd.com"
  },
  "events": [
    {
      "id": 2,
      "description": "Gavin's Coffee event description for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"1\">Jordan Godwin<\/a>.",
      "media": {
        "media_url": "http://lorempixel.com/600/338/sports/3",
        "media_thumb": "http://lorempixel.com/300/169/sports/9",
        "media_type": 0,
        "width": 600,
        "height": 338,
        "thumb_width": 300,
        "thumb_height": 169
      },
      "starts_at": "2017-01-21T03:22:12.000Z",
      "ends_at": "2017-01-21T03:22:17.719Z",
      "status": 0,
      "status": 0,
      "event_privacy": 0,
      "attendee_status": 0,
      "notifications_status": false,
      "comments_count": 0,
      "attendee_count": 0,
      "host": {
        "id": 3,
        "first_name": "Gavin",
        "last_name": "Baradic",
        "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/4",
        "block_status": false,
        "follow_status": false,
        "profile_tags": [
          {
            "id": 1,
            "name": "Sports",
            "parent_id": null
          },
          {
            "id": 3,
            "name": "Football",
            "parent_id": 1
          }
        ]
      },
      "venue": {
        "id": 32,
        "city": "Wilmington",
        "state": "NC",
        "name": "Untappd HQ - ILM",
        "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
        "coords": {
          "lat": 34.4332,
          "lng": -77.8485
        },
        "follow_status": false,
        "category": "Tech Startup",
        "foursquare_id": "5759a613cd1040089032b492"
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
          "id": 5,
          "name": "Hockey",
          "parent_id": 1
        },
        {
          "id": 7,
          "name": "Racing",
          "parent_id": 1
        }
      ]
    }
  ]
}
```

Description: This endpoint will return an up-to-date response of
data for the specified venue, including contact info, location and
the current open/closed status.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/`

## Foursquare Venues<a name="venues_query"></a> (Venue Name Query)

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/venues/search?q=duck+dive
```
```shell
https://wildfire-staging.herokuapp.com/api/v1/venues/search?tag_id=23
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "name": "Duck & Dive Pub",
    "city": "Wilmington",
    "state": "NC",
    "address": "11 S. Front St",
    "coords": {
      "lat": 34.234175252892,
      "lng": -77.947961431061
    },
    "distance": 59,
    "foursquare_id": "4ba29614f964a520ba0638e3",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Pub"
  },
  {
    "name": "Dig & Dive",
    "city": "Wilmington",
    "state": "NC",
    "address": "129 Covil Rd.",
    "coords": {
      "lat": 34.238095496413,
      "lng": -77.899629099616
    },
    "distance": 4476,
    "foursquare_id": "54f47e79498e08a96c858200",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/default_64.png",
    "category": "Comfort Food Restaurant"
  },
  {
    "name": "Ducks Sporting Goods",
    "city": "Wilmington",
    "state": "NC",
    "address": "1292 Military Cutoff Rd.",
    "coords": {
      "lat": 34.213194444444,
      "lng": -77.887222222222
    },
    "distance": 6093,
    "foursquare_id": "4cf9375a951537042b626189",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/sports_outdoors_64.png",
    "category": "Sporting Goods Shop"
  },
  {
    ...
  },
  {
    "name": "Two Duck Lake",
    "city": "Castle Hayne",
    "state": "NC",
    "address": "6 3/4 Holland Dr",
    "coords": {
      "lat": 34.300494887124,
      "lng": -77.919621866052
    },
    "distance": 7778,
    "foursquare_id": "4efba1e377c822653ea5e151",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/lake_64.png",
    "category": "Lake"
  },
  {
    "name": "Diversified Energy",
    "city": "Wilm",
    "state": "NC",
    "address": "Gordon Road",
    "coords": {
      "lat": 34.268261124196,
      "lng": -77.82300554898
    },
    "distance": 12101,
    "foursquare_id": "5012f671e4b0f8ec77ef3c1a",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/building\/default_64.png",
    "category": "Building"
  }
]
```

Description: This endpoint will return up to 40 Foursquare results based
on the `:lat`, `:lng`, and `:query` parameters provided by the requesting client.

*NOTE:* These objects are not stored in our database. Upon creating an event, the
venue object that is passed back will then be cached and stored for retrieval later.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/search?q=search+query+string+here`


### Query Parameters
*NOTE:* Only use one of the below query parameters.

Parameter | Default | Description
--------- | ------- | -----------
`q` | nil | A string containing the search query of the name of a Foursquare venue
`tag_id` | nil | The ID of the specified Tag used to search Foursquare venues based on the Tag's name

## Foursquare Venues (Radius Search)

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/venues/nearby?lat=34.2347&lng=-77.9481&radius=3000
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "name": "Buzz's Roost",
    "city": "Wilmington",
    "state": "NC",
    "address": "11 S. Front St",
    "coords": {
      "lat": 34.23477,
      "lng": -77.948472
    },
    "distance": 35,
    "foursquare_id": "51394473e4b0668118640b27",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  {
    "name": "Untappd HQ - ILM",
    "city": "Wilmington",
    "state": "NC",
    "address": "21 S. Front St",
    "coords": {
      "lat": 34.235789629482,
      "lng": -77.947483062744
    },
    "distance": 133,
    "foursquare_id": "5759a613cd1040089032b492",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
    "category": "Tech Startup"
  },
  {
    "name": "The Husk Seed Store And Bar",
    "city": "Wilmington",
    "state": "NC",
    "address": "26 S. Front St",
    "coords": {
      "lat": 34.234334508993,
      "lng": -77.94827937235
    },
    "distance": 43,
    "foursquare_id": "4e860502e5fab0fb114a3a48",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "category": "Bar"
  },
  ...
  {
    "name": "Wilmington Riverwalk",
    "city": "Wilmington",
    "state": "NC",
    "address": "Water Street",
    "coords": {
      "lat": 34.234473662713,
      "lng": -77.949545550328
    },
    "distance": 135,
    "foursquare_id": "4c9e7f43ca44236a38872b99",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/sceniclookout_64.png",
    "category": "Scenic Lookout"
  },
  {
    "name": "Dram + Morsel",
    "city": "Wilmington",
    "state": "NC",
    "address": "33 S Front St",
    "coords": {
      "lat": 34.234334962627,
      "lng": -77.948360145092
    },
    "distance": 47,
    "foursquare_id": "5855ef680bc55b58b7c6fc64",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/tapas_64.png",
    "category": "Tapas Restaurant"
  }
]
```

Description: This endpoint will return up to 10 Foursquare results based on the `:radius`
distance of the `:lat` & `:lng` parameters provided by the requesting client.

*NOTE:* These objects are not stored in our database. Upon creating an event, the
venue object that is passed back will then be cached and stored for retrieval later.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/nearby?lat=00.00000&lng=00.00000&radius=0000`

### Query Parameters (*All are required*)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | nil | The requesting client's latitude coordinate
`lng` | nil | The requesting client's longitude coordinate
`radius` | nil | An integer value representing the radius distance (in meters) from the specified `:lat` & `:lng` values

## Follow a Venue

> Send a POST request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/follow`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 1,
  "name": "Untappd HQ - ILM",
  "city": "Wilmington",
  "state": "NC",
  "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
  "coords": {
    "lat": 34.235789629482,
    "lng": -77.947483062744
  },
  "category": "Tech Startup",
  "foursquare_id": "5759a613cd1040089032b492"
}
```

Description: This endpoint will create a `follow` for the current user with the
followed target being the specified venue.

### HTTP Request

`POST https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/follow`

## Un-follow a Venue

> Send a DELETE request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/follow`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
{
  "id": 1,
  "name": "Untappd HQ - ILM",
  "city": "Wilmington",
  "state": "NC",
  "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
  "coords": {
    "lat": 34.235789629482,
    "lng": -77.947483062744
  },
  "category": "Tech Startup",
  "foursquare_id": "5759a613cd1040089032b492"
}
```

Description: This endpoint will *remove* a previously created `follow` for the current user with the
followed target being the specified venue.

### HTTP Request

`DELETE https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/follow`

## Unfinished Venue Events [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/57483748/events`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 2,
    "description": "Gavin's Coffee event description for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"1\">Jordan Godwin<\/a>.",
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "starts_at": "2017-01-21T03:22:12.000Z",
    "ends_at": "2017-01-21T03:22:17.719Z",
    "status": 0,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
    "comments_count": 0,
    "attendee_count": 0,
    "host": {
      "id": 3,
      "first_name": "Gavin",
      "last_name": "Baradic",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/4",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null
        },
        {
          "id": 3,
          "name": "Football",
          "parent_id": 1
        }
      ]
    },
    "venue": {
      "id": 84,
      "name": "21 South Coffee",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": false,
      "category": "Coffee Shop",
      "foursquare_id": "ag5gd742dsf83g7d48g",
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
        "id": 5,
        "name": "Hockey",
        "parent_id": 1
      },
      {
        "id": 7,
        "name": "Racing",
        "parent_id": 1
      }
    ]
  },
  ...
]
```

Description: This endpoint will return an array of events from the
specified Foursquare venue that _unfinished_; meaning they either have
not begun yet or are currently active but have not yet completed.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/events`

## Past Venue Events [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/57483748/events/past`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 2,
    "description": "Gavin's Coffee event description for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"1\">Jordan Godwin<\/a>.",
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "starts_at": "2017-01-21T03:22:12.000Z",
    "ends_at": "2017-01-21T03:22:17.719Z",
    "status": 0,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
    "comments_count": 0,
    "attendee_count": 0,
    "host": {
      "id": 3,
      "first_name": "Gavin",
      "last_name": "Baradic",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/4",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null
        },
        {
          "id": 3,
          "name": "Football",
          "parent_id": 1
        }
      ]
    },
    "venue": {
      "id": 84,
      "name": "21 South Coffee",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": false,
      "category": "Coffee Shop",
      "foursquare_id": "ag5gd742dsf83g7d48g",
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
        "id": 5,
        "name": "Hockey",
        "parent_id": 1
      },
      {
        "id": 7,
        "name": "Racing",
        "parent_id": 1
      }
    ]
  },
  ...
]
```

Description: This endpoint will return an array of events from the
specified Foursquare venue that are in the past and therefore no longer
active or available to join.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/events/past`

## Active Venue Events [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/57483748/events/active`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 2,
    "description": "Gavin's Coffee event description for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"1\">Jordan Godwin<\/a>.",
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "starts_at": "2017-01-21T03:22:12.000Z",
    "ends_at": "2017-01-21T03:22:17.719Z",
    "status": 0,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
    "comments_count": 0,
    "attendee_count": 0,
    "host": {
      "id": 3,
      "first_name": "Gavin",
      "last_name": "Baradic",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/4",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null
        },
        {
          "id": 3,
          "name": "Football",
          "parent_id": 1
        }
      ]
    },
    "venue": {
      "id": 84,
      "name": "21 South Coffee",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": true,
      "category": "Coffee Shop",
      "foursquare_id": "ag5gd742dsf83g7d48g",
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
        "id": 5,
        "name": "Hockey",
        "parent_id": 1
      },
      {
        "id": 7,
        "name": "Racing",
        "parent_id": 1
      }
    ]
  },
  ...
]
```
Description: This endpoint will return an array of events from the
specified Foursquare venue that are currently active/happening now.

## Future Venue Events [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
`https://wildfire-staging.herokuapp.com/api/v1/venues/57483748/events/future`
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 2,
    "description": "Gavin's Coffee event description for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"1\">Jordan Godwin<\/a>.",
    "media": {
      "media_url": "http://lorempixel.com/600/338/sports/3",
      "media_thumb": "http://lorempixel.com/300/169/sports/9",
      "media_type": 0,
      "width": 600,
      "height": 338,
      "thumb_width": 300,
      "thumb_height": 169
    },
    "starts_at": "2017-01-21T03:22:12.000Z",
    "ends_at": "2017-01-21T03:22:17.719Z",
    "status": 0,
    "status": 0,
    "event_privacy": 0,
    "attendee_status": 0,
    "notifications_status": false,
    "comments_count": 0,
    "attendee_count": 0,
    "host": {
      "id": 3,
      "first_name": "Gavin",
      "last_name": "Baradic",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/4",
      "block_status": false,
      "follow_status": false,
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null
        },
        {
          "id": 3,
          "name": "Football",
          "parent_id": 1
        }
      ]
    },
    "venue": {
      "id": 84,
      "name": "21 South Coffee",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
      "coords": {
        "lat": 34.4332,
        "lng": -77.8485
      },
      "follow_status": true,
      "category": "Coffee Shop",
      "foursquare_id": "ag5gd742dsf83g7d48g",
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
        "id": 5,
        "name": "Hockey",
        "parent_id": 1
      },
      {
        "id": 7,
        "name": "Racing",
        "parent_id": 1
      }
    ]
  },
  ...
]
```
Description: This endpoint will return an array of events from the
specified Foursquare venue that are set to happen at a time in the near future.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/events/future`

## Get Venues Checkins

> Send a GET request with required parameters:

```shell
https://wildfire-dev.herokuapp.com/api/v1/venues/232/checkins
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "body": "Made it to the event!",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 82,
    "venue_id": 232,
    "created_at": "2017-05-31T18:20:00.651Z",
    "user": {
      "id": 11,
      "first_name": "Gary",
      "last_name": "Watsica",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null,
          "created_at": "2017-05-24T18:52:48.104Z",
          "updated_at": "2017-05-24T18:52:48.104Z"
        },
        {
          "id": 245,
          "name": "WRST Raiders Basketball",
          "parent_id": 241,
          "created_at": "2017-05-24T18:52:43.576Z",
          "updated_at": "2017-05-24T18:52:43.576Z"
        }
      ]
    }
  },
  {
    ...
  },
  {
    "id": 4,
    "body": "",
    "media": {
      "width": 300,
      "height": 300,
      "media_url": "test.com\/img",
      "media_type": 0,
      "media_thumb": "test.com\/img_thumb",
      "thumb_width": 100,
      "thumb_height": 100
    },
    "event_id": 384,
    "venue_id": 232,
    "created_at": "2017-05-31T18:53:40.773Z",
    "user": {
      "id": 32,
      "first_name": "Daniel",
      "last_name": "Winston",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "profile_tags": [
        {
          "id": 1,
          "name": "Sports",
          "parent_id": null,
          "created_at": "2017-05-24T18:52:48.104Z",
          "updated_at": "2017-05-24T18:52:48.104Z"
        },
        {
          "id": 245,
          "name": "WRST Raiders Basketball",
          "parent_id": 241,
          "created_at": "2017-05-24T18:52:43.576Z",
          "updated_at": "2017-05-24T18:52:43.576Z"
        }
      ]
    }
  }
]
```

This endpoint will retrieve all checkin's for the specified venue.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/venues/:foursquare_id/checkins`

# Feeds

## User Feed [[Pg'd](#pagination)]

> Send a GET request with required parameters:

```shell
GET https://wildfire-staging.herokuapp.com/api/v1/feeds/user_feed?lat=34.225723432&lng=-77.944723238
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 77,
    "description": "Harum dolores omnis esse pariatur hic.",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338\/abstract\/3",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169\/abstract\/3",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 19,
    "privacy": 1,
    "starts_at": "2017-06-14T00:00:00.000Z",
    "ends_at": "2017-06-22T00:00:00.000Z",
    "duration": 8.25608,
    "event_status": 1,
    "attendee_data": {
      "me": {},
      "total_attendee_count": 0,
      "follows_attendee_count": 0,
      "users_data": []
    },
    "host": {
      "id": 496,
      "first_name": "Linda",
      "last_name": "Crona",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/6",
      "block_status": false,
      "follow_status": false,
      "reverse_block_status": true,
      "reverse_follow_status": false
    },
    "venue": {
      "id": 26,
      "name": "Silver Dollar",
      "city": "Carolina Beach",
      "state": "NC",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/divebar_64.png",
      "coords": {
        "lat": 34.033268094063,
        "lng": -77.89241194725
      },
      "category": "Dive Bar",
      "follow_status": false,
      "foursquare_id": "4c1a4d3eb306c9284e5b60b7"
    },
    "tags": [
      {
        "id": 350,
        "name": "PORT Pilots Basketball",
        "parent_id": 348,
        "created_at": "2017-05-15T18:07:12.118Z",
        "updated_at": "2017-05-15T18:07:12.118Z"
      },
      {
        "id": 273,
        "name": "MSST Bulldogs Basketball",
        "parent_id": 264,
        "created_at": "2017-05-15T18:07:10.947Z",
        "updated_at": "2017-05-15T18:07:10.947Z"
      },
      {
        "id": 493,
        "name": "UNLV Rebels Football",
        "parent_id": 484,
        "created_at": "2017-05-15T18:07:14.358Z",
        "updated_at": "2017-05-15T18:07:14.358Z"
      }
    ]
  },
  {
    "id": 123,
    "description": "Odit accusamus ab illo facere numquam quo corrupti temporibus.",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338\/animals\/6",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169\/animals\/6",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 17,
    "privacy": 1,
    "starts_at": "2017-06-17T00:00:00.000Z",
    "ends_at": "2017-06-22T00:00:00.000Z",
    "duration": 5.54884,
    "event_status": 0,
    "attendee_data": {
      "me": {},
      "total_attendee_count": 0,
      "follows_attendee_count": 0,
      "users_data": []
    },
    "host": {
      "id": 310,
      "first_name": "Davon",
      "last_name": "Beatty",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/8",
      "block_status": false,
      "follow_status": false,
      "reverse_block_status": false,
      "reverse_follow_status": false
    },
    "venue": {
      "id": 53,
      "name": "The Players' Retreat",
      "city": "Raleigh",
      "state": "NC",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/sportsbar_64.png",
      "coords": {
        "lat": -78.661452233791,
        "lng": -78.661452233791
      },
      "category": "Sports Bar",
      "follow_status": true,
      "foursquare_id": "4ad8bd4ef964a520351421e3"
    },
    "tags": [
      {
        "id": 273,
        "name": "MSST Bulldogs Basketball",
        "parent_id": 264,
        "created_at": "2017-05-15T18:07:10.947Z",
        "updated_at": "2017-05-15T18:07:10.947Z"
      },
      {
        "id": 43,
        "name": "L-IL Ramblers Basketball",
        "parent_id": 39,
        "created_at": "2017-05-15T18:07:07.147Z",
        "updated_at": "2017-05-15T18:07:07.147Z"
      },
      {
        "id": 336,
        "name": "PRIN Tigers Basketball",
        "parent_id": 328,
        "created_at": "2017-05-15T18:07:11.902Z",
        "updated_at": "2017-05-15T18:07:11.902Z"
      }
    ]
  },
  {
    ...
  }
]
```

Description: This endpoint will return a feed of filtered events based on the
requesting user's filter tags & users that he/she follows.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/feeds/user_feed`

### Query Parameters (required)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | n/a | latitude coordinate of the requesting client
`lng` | n/a | longitude coordinate of the requesting client

## Local Feed [[Pg'd](#pagination)]

> Send a POST request with required parameters:

```shell
GET https://wildfire-staging.herokuapp.com/api/v1/feeds/local?lat=34.225723432&lng=-77.944723238
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 3,
    "description": "Jordan's fishing trip for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a> goes here",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338\/people\/4",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169\/people\/4",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 40,
    "event_privacy": 0,
    "starts_at": "2017-05-22T00:00:00.000Z",
    "ends_at": "2017-05-28T00:00:00.000Z",
    "duration": 6.86,
    "event_status": 1,
    "attendee_data": {
      "me": {},
      "total_attendee_count": 41,
      "follows_attendee_count": 4,
      "follows_data": [
        {
          "user_id": 5,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7"
        },
        {
          "user_id": 232,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/5"
        },
        {
          "user_id": 381,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2"
        },
        {
          "user_id": 524,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2"
        }
      ]
    },
    "host": {
      "id": 5,
      "first_name": "Test",
      "last_name": "Testerson",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/7",
      "block_status": false,
      "follow_status": true,
      "reverse_block_status": false,
      "reverse_follow_status": true
    },
    "venue": {
      "id": 29,
      "name": "Barbary Coast",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/divebar_64.png",
      "coords": {
        "lat": 34.233611017457,
        "lng": -77.948636357948
      },
      "category": "Dive Bar",
      "follow_status": false,
      "foursquare_id": "4b88846bf964a520b7fd31e3"
    },
    "tags": [
      {
        "id": 32,
        "name": "Outdoors",
        "parent_id": 1,
      },
      {
        "id": 94,
        "name": "Fishing",
        "parent_id": 32,
      }
    ]
  },
  {
    ...
  },
  {
    "id": 15,
    "description": "Event 15: Kerry's sports party for <a class=\"user\" id=\"1\">Jordan Godwin<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a>.",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338\/sports\/9",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169\/sports\/9",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 33,
    "event_privacy": 0,
    "starts_at": "2017-05-18T00:00:00.000Z",
    "ends_at": "2017-05-25T00:00:00.000Z",
    "duration": 7.39,
    "event_status": 1,
    "attendee_data": {
      "me": {
        "user_id": 2,
        "attendee_status": 2,
        "mute_notifications": false
      },
      "total_attendee_count": 40,
      "follows_attendee_count": 2,
      "follows_data": [
        {
          "user_id": 75,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2"
        },
        {
          "user_id": 453,
          "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/2"
        }
      ]
    },
    "host": {
      "id": 2,
      "first_name": "Kerry",
      "last_name": "Knight",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/5"
    },
    "venue": {
      "id": 6,
      "name": "Ironclad Brewery",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/brewery_64.png",
      "coords": {
        "lat": 34.237000354377,
        "lng": -77.947729825973
      },
      "category": "Brewery",
      "follow_status": false,
      "foursquare_id": "53dbd858498e33be781da324"
    },
    "tags": [
      {
        "id": 312,
        "name": "Beer",
        "parent_id": 35,
      }
    ]
  }
]
```

Description: This endpoint will return a feed of filtered events based on the
location that the event was recorded starting with the closest first.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/feeds/local`

### Query Parameters (required)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | n/a | latitude coordinate of the requesting client
`lng` | n/a | longitude coordinate of the requesting client

## Local Map Feed

> Send a POST request with required parameters:

```shell
GET https://wildfire-staging.herokuapp.com/api/v1/feeds/local_map?lat=34.225723432&lng=-77.944723238&radius=8000
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 1,
    "name": "Untappd HQ - ILM",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
    "coords": {
      "lat": 34.235789629482,
      "lng": -77.947483062744
    },
    "category": "Tech Startup",
    "follow_status": true,
    "foursquare_id": "5759a613cd1040089032b492",
    "active_events_count": 14,
    "future_events_count": 3
  },
  {
    "id": 6,
    "name": "Ironclad Brewery",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/brewery_64.png",
    "coords": {
      "lat": 34.237000354377,
      "lng": -77.947729825973
    },
    "category": "Brewery",
    "follow_status": false,
    "foursquare_id": "53dbd858498e33be781da324",
    "active_events_count": 42,
    "future_events_count": 0
  },
  {
    ...
  },
  {
    "id": 2,
    "name": "Duck & Dive Pub",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "coords": {
      "lat": 34.234175252892,
      "lng": -77.947961431061
    },
    "category": "Pub",
    "follow_status": false,
    "foursquare_id": "4bf58dd8d48988d11b941735",
    "active_events_count": 11,
    "future_events_count": 1
  },
  {
    "id": 3,
    "name": "24 South Coffee House",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/coffeeshop_64.png",
    "coords": {
      "lat": 34.234456599111,
      "lng": -77.948524800075
    },
    "category": "Coffee Shop",
    "follow_status": false,
    "foursquare_id": "53fa55f3498ed31bb942100a",
    "active_events_count": 7,
    "future_events_count": 0
  },
  {
    "id": 4,
    "name": "Masonboro Inlet",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/beach_64.png",
    "coords": {
      "lat": 34.182547362367,
      "lng": -77.81902944261
    },
    "category": "Beach",
    "follow_status": false,
    "foursquare_id": "4c30a001a0ced13a3c61126e",
    "active_events_count": 3,
    "future_events_count": 1
  }
]
```

Description: This endpoint will return an array of Venues with the
count of _unfinished_ venues on each venue object. Use the
`/api/v1/venues/:foursquare_id/events` endpoint to retrieve the unfinished
events for that venue.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/feeds/local_map`

### Query Parameters (required)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | n/a | latitude coordinate of the requesting client
`lng` | n/a | longitude coordinate of the requesting client
`radius` | n/a | radius in meters from the users current location

## Filtered Map Feed

> Send a POST request with required parameters:

```shell
GET https://wildfire-staging.herokuapp.com/api/v1/feeds/filtered_map?lat=34.225723432&lng=-77.944723238&radius=8000
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 1,
    "name": "Untappd HQ - ILM",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/shops\/technology_64.png",
    "coords": {
      "lat": 34.235789629482,
      "lng": -77.947483062744
    },
    "category": "Tech Startup",
    "follow_status": true,
    "foursquare_id": "5759a613cd1040089032b492",
    "active_events_count": 14,
    "future_events_count": 1
  },
  {
    "id": 2,
    "name": "Duck & Dive Pub",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/nightlife\/pub_64.png",
    "coords": {
      "lat": 34.234175252892,
      "lng": -77.947961431061
    },
    "category": "Pub",
    "follow_status": false,
    "foursquare_id": "4bf58dd8d48988d11b941735",
    "active_events_count": 11,
    "future_events_count": 0
  },
  {
    ...
  },
  {
    "id": 3,
    "name": "24 South Coffee House",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/food\/coffeeshop_64.png",
    "coords": {
      "lat": 34.234456599111,
      "lng": -77.948524800075
    },
    "category": "Coffee Shop",
    "follow_status": false,
    "foursquare_id": "53fa55f3498ed31bb942100a",
    "active_events_count": 7,
    "future_events_count": 5
  },
  {
    "id": 4,
    "name": "Masonboro Inlet",
    "city": "Wilmington",
    "state": "NC",
    "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/beach_64.png",
    "coords": {
      "lat": 34.182547362367,
      "lng": -77.81902944261
    },
    "category": "Beach",
    "follow_status": false,
    "foursquare_id": "4c30a001a0ced13a3c61126e",
    "active_events_count": 3,
    "future_events_count": 0
  }
]
```

Description: This endpoint will return an array of Venues based on
the filter settings that the current_user has set. Each venue object
includes the count of _unfinished_ venues. Use the
`/api/v1/venues/:foursquare_id/events` endpoint to retrieve the unfinished
events for each venue.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/feeds/filtered_map`

### Query Parameters (required)

Parameter | Default | Description
--------- | ------- | -----------
`lat` | n/a | latitude coordinate of the requesting client
`lng` | n/a | longitude coordinate of the requesting client
`radius` | n/a | radius in meters from the users current location

# Discovery

## Search Users by Name or Tag [[Pg'd](#pagination)]

Description: Use the [Tag/Mention User](#mention_user) endpoint for User Discovery

## Search Venues by Name or Tag

Description: Use the [Foursquare Venues Search](#venues_query) endpoint for Venue Discovery

## Events Search by Tag [[Pg'd](#pagination)]

> Send a POST request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/events/search?tag_id=2
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "id": 189,
    "description": "Lomo squid asymmetrical wes anderson kitsch. Retro diy waistcoat pork belly. Vegan yr mustache. Cleanse actually goth shabby chic kale chips chillwave ugh kogi.",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 15,
    "attendee_count": 0,
    "event_privacy": 1,
    "starts_at": "2017-03-28T00:00:00.000Z",
    "ends_at": "2017-03-31T00:00:00.000Z",
    "duration": 3.71,
    "status": 1,
    "attendee_status": 0,
    "host": {
      "id": 445,
      "first_name": "Doris",
      "last_name": "Schroeder",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/cats\/4",
      "block_status": false,
      "follow_status": false,
      "reverse_block_status": false,
      "reverse_follow_status": false,
      "profile_tags": [
        {
          "id": 363,
          "name": "PROV Friars Basketball",
          "parent_id": 359,
          "created_at": "2017-03-28T18:25:10.894Z",
          "updated_at": "2017-03-28T18:25:10.894Z"
        },
        {
          "id": 504,
          "name": "CAL Bears Football",
          "parent_id": 497,
          "created_at": "2017-03-28T18:25:12.557Z",
          "updated_at": "2017-03-28T18:25:12.557Z"
        }
      ]
    },
    "venue": {
      "id": 104,
      "name": "Fat Cat",
      "city": "New York",
      "state": "NY",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/arts_entertainment\/musicvenue_jazzclub_64.png",
      "coords": {
        "lat": -74.002879437032,
        "lng": -74.002879437032
      },
      "category": "Jazz Club",
      "follow_status": false,
      "foursquare_id": "3fd66200f964a52000e71ee3"
    },
    "tags": [
      {
        "id": 2,
        "name": "Basketball",
        "parent_id": 1,
        "created_at": "2017-03-28T18:25:06.809Z",
        "updated_at": "2017-03-28T18:25:06.809Z"
      },
      {
        "id": 49,
        "name": "WICH Shockers Basketball",
        "parent_id": 39,
        "created_at": "2017-03-28T18:25:07.372Z",
        "updated_at": "2017-03-28T18:25:07.372Z"
      },
      {
        "id": 333,
        "name": "CLMB Lions Basketball",
        "parent_id": 328,
        "created_at": "2017-03-28T18:25:10.576Z",
        "updated_at": "2017-03-28T18:25:10.576Z"
      }
    ]
  },
  {
    "id": 1,
    "description": "Event 1: Jordan's fishing trip for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a> goes here",
    "media": {
      "width": 600,
      "height": 338,
      "media_url": "http:\/\/lorempixel.com\/600\/338\/sports\/4",
      "media_type": "image",
      "media_thumb": "http:\/\/lorempixel.com\/300\/169\/sports\/4",
      "thumb_width": 300,
      "thumb_height": 169
    },
    "comments_count": 0,
    "attendee_count": 0,
    "event_privacy": 0,
    "starts_at": "2017-04-09T00:00:00.000Z",
    "ends_at": "2017-04-14T00:00:00.000Z",
    "duration": 5.45,
    "status": 0,
    "attendee_status": 0,
    "host": {
      "id": 1,
      "first_name": "Jordan",
      "last_name": "Godwin",
      "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/7",
      "profile_tags": [
        {
          "id": 306,
          "name": "MINN Golden Gophers Basketball",
          "parent_id": 292,
          "created_at": "2017-03-28T18:25:10.292Z",
          "updated_at": "2017-03-28T18:25:10.292Z"
        },
        {
          "id": 407,
          "name": "UVA Cavaliers Football",
          "parent_id": 398,
          "created_at": "2017-03-28T18:25:11.391Z",
          "updated_at": "2017-03-28T18:25:11.391Z"
        },
        {
          "id": 506,
          "name": "STA Cardinal Football",
          "parent_id": 497,
          "created_at": "2017-03-28T18:25:12.579Z",
          "updated_at": "2017-03-28T18:25:12.579Z"
        },
        {
          "id": 508,
          "name": "ORE Ducks Football",
          "parent_id": 497,
          "created_at": "2017-03-28T18:25:12.602Z",
          "updated_at": "2017-03-28T18:25:12.602Z"
        }
      ]
    },
    "venue": {
      "id": 4,
      "name": "Masonboro Inlet",
      "city": "Wilmington",
      "state": "NC",
      "icon_url": "https:\/\/ss3.4sqi.net\/img\/categories_v2\/parks_outdoors\/beach_64.png",
      "coords": {
        "lat": 34.182547362367,
        "lng": -77.81902944261
      },
      "category": "Beach",
      "follow_status": false,
      "foursquare_id": "4c30a001a0ced13a3c61126e"
    },
    "tags": [
      {
        "id": 1,
        "name": "Sports",
        "parent_id": null,
        "created_at": "2017-03-28T18:25:06.753Z",
        "updated_at": "2017-03-28T18:25:06.753Z"
      },
      {
        "id": 2,
        "name": "Basketball",
        "parent_id": 1,
        "created_at": "2017-03-28T18:25:06.809Z",
        "updated_at": "2017-03-28T18:25:06.809Z"
      },
      {
        "id": 5,
        "name": "Hockey",
        "parent_id": 1,
        "created_at": "2017-03-28T18:25:06.849Z",
        "updated_at": "2017-03-28T18:25:06.849Z"
      }
    ]
  }
]
```

Description: Searches for events that have been tagged with the specified tag.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/events/search?tag_id=122`

### Query Parameters (or additional details)

Parameter | Default | Description
--------- | ------- | -----------
`tag_id` | nil | Used to search for users by the specified Tag

# Images

## Images Search via Bing API

> Send a GET request with required parameters:

```shell
https://wildfire-staging.herokuapp.com/api/v1/images/search?q=duke+basketball&page=1&per_page=50
```

> A JSON response like the following would be returned:

```shell
status: 200
```
```json
[
  {
    "url": "http:\/\/www.bing.com\/cr?IG=FD582B4EB2904C008DACC9F24EB62E47&CID=18C6E3EA71786AE02B31E8E0707E6BC5&rd=1&h=EYmyl6N3Kx6-2nvVCFerlnnnOhpDbWYHGwfZVjqptAY&v=1&r=http%3a%2f%2fnews.fiu.edu%2fwp-content%2fuploads%2fbasketball.jpg&p=DevEx,5009.1",
    "thumb_url": "https:\/\/tse2.mm.bing.net\/th?id=OIP.DYsvGZUaRnb5d8LVeJruzQD2Es&pid=Api"
  },
  {
    "url": "http:\/\/www.bing.com\/cr?IG=FD582B4EB2904C008DACC9F24EB62E47&CID=18C6E3EA71786AE02B31E8E0707E6BC5&rd=1&h=t_yfnZmTeeEhzcxuKaycrRIICNnWE-FkyVA4Pv0yU68&v=1&r=http%3a%2f%2fupload.wikimedia.org%2fwikipedia%2fcommons%2f7%2f7a%2fBasketball.png&p=DevEx,5015.1",
    "thumb_url": "https:\/\/tse1.mm.bing.net\/th?id=OIP.m3KV6ywO19IfCOjH5tBCXwEsEs&pid=Api"
  },
  {
    "url": "http:\/\/www.bing.com\/cr?IG=FD582B4EB2904C008DACC9F24EB62E47&CID=18C6E3EA71786AE02B31E8E0707E6BC5&rd=1&h=c3qoil5bs3IHRZmIOd_ufWu9tWRbkWRBz2gWaRgFJwI&v=1&r=http%3a%2f%2fs3.amazonaws.com%2fvnn-aws-sites%2f10188%2ffiles%2f2017%2f02%2f7dbcd0ef5aa0d212-BasketballStockImage.jpg&p=DevEx,5021.1",
    "thumb_url": "https:\/\/tse1.explicit.bing.net\/th?id=OIP.VnEy6s3M58Ga6GRvvhQv-QEkEs&pid=Api"
  },
  {
    "url": "http:\/\/www.bing.com\/cr?IG=FD582B4EB2904C008DACC9F24EB62E47&CID=18C6E3EA71786AE02B31E8E0707E6BC5&rd=1&h=7te_i-ZysYbms983gKZi5h8qOPA9oyIgiodIJJEV0gg&v=1&r=http%3a%2f%2fmurrayglobal.files.wordpress.com%2f2012%2f03%2fbasketball.jpg&p=DevEx,5027.1",
    "thumb_url": "https:\/\/tse2.mm.bing.net\/th?id=OIP.tqyRBFPb39ne2tGX__bRnAEsEs&pid=Api"
  },
  {
    "url": "http:\/\/www.bing.com\/cr?IG=FD582B4EB2904C008DACC9F24EB62E47&CID=18C6E3EA71786AE02B31E8E0707E6BC5&rd=1&h=Kvg6ZD-UuFxNmuOknCCk95dWpsXavX0RjFTNUYAOzzU&v=1&r=http%3a%2f%2fthumb101.shutterstock.com%2fdisplay_pic_with_logo%2f540784%2f124198177%2fstock-photo-basketball-isolated-on-a-white-background-as-a-sports-and-fitness-symbol-of-a-team-leisure-activity-124198177.jpg&p=DevEx,5033.1",
    "thumb_url": "https:\/\/tse2.mm.bing.net\/th?id=OIP.5ooh4fdTEtHRREzav6GHJwElEs&pid=Api"
  }
]
```

Description: This endpoint will a paginated response of images from the Bing search API.

### HTTP Request

`GET https://wildfire-staging.herokuapp.com/api/v1/images/search?q=search+query+string+here&page=1&per_page=50`

### Required Header on all subsequent requests
*NOTE:* After the initial request has been made to this endpoint a header similar to the
one below will be returned. This header should be persisted on the client device and
must be passed on all subsequent requests to this endpoint.

Key | Value
--------- | -------
'X-MSEdge-ClientID' | '18C6E3EA71786AE02B31E8E0707E6BC5'

### Query Parameters
*NOTE:* Only use one of the below query parameters.

Parameter | Default | Description
--------- | ------- | -----------
`q` | nil | A string containing the search query for images
`page` | 1 | the page being requested from the Bing Image Search API
`per_page` | 25 | the number of images to return per requested page

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
