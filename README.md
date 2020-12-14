# IGBlade Frontend Challenge

## Overview

The goal of this challenge is to see how experienced you are in Vue.js and how much you like to deliver clean code.

## Objective

Create a modular filter component that can be used everywhere regardless of the type of items that are being filtered. For example, currently we use a component like that to filter a user's assigned social media profiles and posts.

## API interface

We are in the process of transitioning to GraphQL from our REST API, so currently we have the filtering API interface implemented in both protocols. For the sake of simplicity (and because GraphQL is not yet fully implemented), you should use the REST API for reference.

### Endpoints

- User accounts: `POST https://igblade.com/api/v3/users/{userId}/accounts/searches`
  - `userId` is the user's numeric ID
- Social account posts: `POST https://igblade.com/api/v3/{platform}/accounts/{username}/posts/searches`
  - `platform` is one of `instagram` or `tiktok` - please only use Instagram accounts for this challenge.
  - `username` is the social profile's unique username

## Types

The filter component should support the basic types (numbers, strings, booleans)

## Comparisons

The filter component should support different types of comparisons for each type (see above).
- **Numbers**
  - greater than (`gt`)
  - less than (`lt`)
  - is (`is`)
  - is not (`is_not`)
  - is unknown (`unknown`) — _field is null_
  - has any value (`any`)  — _field is **NOT** null_

- **Strings**
  - is (`is`)
  - is not (`is_not`)
  - starts with (`starts_with`)
  - ends with (`ends_with`)
  - contains (`contains`)
  - does not contain (`not_contain`)
  - is unknown (`unknown`) — _field is null_
  - has any value (`any`)  — _field is **NOT** null_

- **Booleans**
  - true (`true`)
  - false (`false`)

## Filterable fields

### Accounts

<details>
  <summary>View all fields</summary>

  <p>

    [
     {
        "key":"follower_count",
        "label":"Followers",
        "type":"number"
     },
     {
        "key":"following_count",
        "label":"Followings",
        "type":"number"
     },
     {
        "key":"media_count",
        "label":"Posts",
        "type":"number"
     },
     {
        "key":"engagement_rate",
        "label":"Engagement rate",
        "type":"number"
     },
     {
        "key":"is_private",
        "label":"Is private",
        "type":"boolean"
     },
     {
        "key":"external_url",
        "label":"Profile link",
        "type":"string"
     },
     {
        "key":"is_verified",
        "label":"Is verified",
        "type":"boolean"
     },
     {
        "key":"is_business",
        "label":"Is business",
        "type":"boolean"
     },
     {
        "key":"biography",
        "label":"Biography",
        "type":"string"
     },
     {
        "key":"platform",
        "label":"Platform",
        "type":"platform"
     },
     {
        "key":"categories",
        "label":"Category",
        "type":"categories"
     },
     {
        "key":"team_member",
        "label":"Assignee",
        "type":"team_member"
     }
    ]

  </p>

</details>

### Posts

## Building the filter request

The filter request data should look the same for all endpoints.

```http request
POST /api/v3/instagram/accounts/5.min.crafts/posts/searches HTTP/1.1
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json;charset=UTF-8
Host: igblade.com
Connection: close
Content-Length: 159

{
   "filters":[
      ...
      {
         "key":"posted_at",
         "type":"date",
         "comparison":"gt_relative",
         "value":"5",
      }
      ...
   ],
   "page":1,
   "sort_by":"posted_at",
   "sort_desc":true
}
```

The filters should be provided as an array of objects with the above structure.
The required fields are:
- `key` the key of the field that is filtered [see keys](#Comparisons)
- `type` the field type that is filtered
- `comparison`
- `value`

### Query params
