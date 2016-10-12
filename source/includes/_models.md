# API Models

Below are the resource models that the Wildfire API has.

## User

> Example User object:

```json
{
  "last_name": "Knight",
  "email": "woozykk@gmail.com",
  "dob": "1980-10-24",
  "gender": "1",
  "avatar_url": "https://fake.urlto.img/user_avatar",
  "uid": "2f9dea45-23ea-47cb-9541-c1218bf59e20",
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
`gender` | :integer | Optional
`about` | :integer | Optional
`tags` | :array | Optional
`social_ids` | :object | Optional
`avatar_url` | :string | Optional
`auth_token` | :string | **Required** *(Set by Server & used on all requests)*
`auth_token_expires_at` | :datetime | **Required** *(Set by Server)*

## Event
