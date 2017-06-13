# API Models

Below are the resource models that the Wildfire API has.

## User

> Example User object:

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
  "start_date_range": 1,
  "social_ids": {
    "twitter": null,
    "facebook": null,
    "instagram": null,
    "pinterest": null
  },
  "followers_count": 16,
  "following_users_count": 65,
  "following_venues_count": 15,
  "attended_count": 15,
  "hosted_count": 10,
  "block_status": false,
  "follow_status": false,
  "reverse_block_status": false,
  "reverse_follow_status": false,
  "profile_tags": [
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
  ],
  "feed_tags": [
    {
      "id": 334,
      "name": "DART Big Green Basketball",
      "parent_id": 328
    },
    {
      "id": 148,
      "name": "CHS Cougars Basketball",
      "parent_id": 146
    },
    {
      "id": 541,
      "name": "Miami Heat",
      "parent_id": 537
    }
  ]
}
```

Attribute | Type | Default | Required/Optional
--------- | ---- | ------- | -----------
`id` | :integer | `n/a` | **Required** *(Set by Server)*
`email` | :string | `n/a` | **Required**
`first_name` | :string | `n/a` | **Required**
`last_name` | :string | `n/a` | **Required**
`dob` | :datetime | `n/a` | **Required**
`about` | :text | '' | Optional
`city` | :string | `n/a` | **Required**
`state` | :string | `n/a` | **Required**
`avatar_url` | :string | `n/a` | **Required**
`coords` | :object | `n/a` | **Required**
`push_notifs` | :object | `{ general: true, messages: true, event_updates: true, mentions: true, event_comments: true, user_follows: true, join_requests: true }` | Optional
`social_ids` | :object | `{ facebook: -1, twitter: -1, instagram: -1, pinterest: -1 }` | **Required**
`start_date_range` | :integer | `1` | **Required**
`badge_count` | :integer | `0` | Optional
`auth_token` | :string | `n/a` | **Required** *(Set by Server & used on all requests)*
`auth_token_expires_at` | :datetime | `n/a` | **Required** *(Set by Server)*
`last_login_time` | :datetime | `n/a` | **Required** *(Set by Server)*
`active` | :boolean | `false` | **Required** *(Set by Server)*
`digits_id` | :string | `n/a` | **Required**
`profile_tags` | [:objects] | {} | Optional
`feed_tags` | [:objects] | {} | Optional

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
  "event_privacy": 0,
  "attendee_status": 0,
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
`description` | :string | **Required**
`image_url` | :string | **Required**
`tags` | :object | **Required**
`starts_at` | :datetime | **Required**
`duration` | :float | **Required**
`ends_at` | :datetime | **Required** *Set by server (based on starts_at & duration)*
`coords` | :point | **Required** *(lat/lng values formatted by the server)*
`host_id` | :integer | **Required** *Set by server (based on current_user)*
`created_at` | :datetime | Optional
`updated_at` | :datetime | Optional
