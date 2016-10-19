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
  "id": 9,
  "description": "test event desc",
  "image_url": "http://www.fakeurl.com/fake_img_id",
  "tags": [
    {
      "title": "NFL"
    },
    {
      "title": "Carolina Panthers"
    },
    {
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

Attribute | Type | Required/Optional
--------- | ------- | -----------
`description` | :string | **Required**
`image_url` | :string | **Required**
`tags` | :object | **Required**
`starts_at` | :datetime | **Required**
`duration` | :float | **Required**
`venue_id` | :integer | Optional
`coords` | :point | **Required** *(lat/lng values formatted by the server)*
`host_id` | :integer | **Required**
`created_at` | :datetime | Optional
`updated_at` | :datetime | Optional
