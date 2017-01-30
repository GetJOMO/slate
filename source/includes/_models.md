# API Models

Below are the resource models that the Wildfire API has.

## User

> Example User object:

```json
{
  "first_name": "Kerry",
  "last_name": "Knight",
  "email": "woozykk@gmail.com",
  "dob": "1982-10-24",
  "avatar_url": "https://fake.urlto.img/user_avatar",
  "auth_token": "'a2zS95rM5yrrsmxOPVZXPjC5",
  "auth_token_expiry": "2016-09-28 17:33:52.449806",
  "last_login_time": "2016-09-27 17:33:52.449806",
  "about": "This is all about Kerry Knight.",
  "tags": "['Swift', 'UNC Sports', 'Beer', 'Startups']",
  "social_ids": {
    "facebook": "729779407355",
    "twitter": null,
    "instagram": null,
    "pinterest": null
  },
  "push_notifs": {
    "general": true,
    "messages": true,
    "events": false,
    "mentions": true,
    "follows": true
  }
}
```

Attribute | Type | Required/Optional
--------- | ------- | -----------
`id` | :integer | **Required**
`email` | :string | **Required**
`password` | :string | **Required**
`first_name` | :string | **Required**
`last_name` | :string | **Required**
`dob` | :date | **Required**
`about` | :integer | Optional
`tags` | :array | Optional
`social_ids` | :object | Optional
`avatar_url` | :string | Optional
`auth_token` | :string | **Required** *(Set by Server & used on all requests)*
`auth_token_expires_at` | :datetime | **Required** *(Set by Server)*

## Event

> Example Event object:

```json
{
  "id": 3,
  "description": "Jordan's fishing trip for <a class=\"user\" id=\"2\">Kerry Knight<\/a> and <a class=\"user\" id=\"3\">Gavin Baradic<\/a> goes here",
  "media": {
    "media_url": "http:\/\/lorempixel.com\/600\/600\/sports\/3",
    "media_thumb": "http:\/\/lorempixel.com\/128\/128\/sports\/6",
    "media_type": null
  },
  "starts_at": "2017-01-19T11:17:07.000Z",
  "ends_at": "2017-01-19T11:17:16.439Z",
  "status": 0,
  "status": 0,
  "privacy": 0,
  "attendee_status": 0,
  "comments_count": 0,
  "attendee_count": 0,
  "host": {
    "id": 1,
    "first_name": "Jordan",
    "last_name": "Godwin",
    "avatar_url": "http:\/\/lorempixel.com\/300\/300\/sports\/3",
    "profile_tags": [
      {
        "id": 94,
        "name": "Fishing",
        "parent_id": 32,
      }
    ]
  },
  "venue": {
    "id": 34,
    "name": "Atlantic Ocean",
    "icon_url": "https:\/\/ss0.4sqi.net\/img\/categories_v2\/nightlife\/default_bg_64.png",
    "coords": {
      "lat": 34.4332,
      "lng": -77.8485
    },
    "category": "Outdoors",
    "foursquare_id": "4e41606ca80996808534bb7f"
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
}
```

Attribute | Type | Required/Optional
--------- | ------- | -----------
`foursquare_id` | :string | **Required**
`description` | :string | **Required**
`image_url` | :string | **Required**
`tags` | :object | **Required**
`starts_at` | :datetime | **Required**
`duration` | :float | **Required**
`coords` | :point | **Required** *(lat/lng values formatted by the server)*
`host_id` | :integer | **Required**
`created_at` | :datetime | Optional
`updated_at` | :datetime | Optional
