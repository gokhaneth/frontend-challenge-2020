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

The filter component should support the following types:
- `number`
  - value can be any integer or float number
- `string`
  - value can be any string
- `boolean`
  - value is not required
- `date`
  - in case a relative comparison is selected value can be an integer representing number of days
  - in case an absolute comparison is selected value can be a date in `YYYY-MM-DD` format
- `time`
  - in case an absolute comparison is selected value should be a 24h time in `HH:mm` format

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

- **Booleans** (`value` field not required, `true` or `false` should be sent in the `comparison` field)
  - true (`true`)
  - false (`false`)

- **Dates**
  - less than (`gt_relative`)
  - more than (`lt_relative`)
  - exactly (`is_relative`)
  - after (`gt`)
  - before (`lt`)
  - is (`on`)
  - is unknown (`unknown`) — _field is null_
  - has any value (`any`)  — _field is **NOT** null_

- **Time**
  - after (`gt`)
  - before (`lt`)
  - is (`on`)
  - is unknown (`unknown`) — _field is null_
  - has any value (`any`)  — _field is **NOT** null_

## Filterable fields

We omitted a few custom fields for simplicity, but the point of this component is modularity, so bear in mind that it should support all kinds of custom fields and comparisons.

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

The filters should be provided as an array of filter objects.

The required fields for each filter are:
- `key` the key of the field that is filtered [see keys](#filterable-fields)
- `type` the field type that is filtered [see types](#types)
- `comparison` the comparison method [see comparisons](#comparisons)
- `value` the user supplied value used for filtering (not required in some cases)

## Component interface

The root of the filter component should have a `value` prop that accepts an array of filters and updates it with `$emit('input', filters)` to be used with `v-model`.

The component should have another prop used to pass in the fields that can be filtered.

It should be simple to implement other types of comparisons and custom fields as seen on IGBlade.com, but for this challenge the [listed fields](#filterable-fields) are enough.

## Bonus points

We'll give bonus points if you implement a wrapper component that parses and writes the filters in the query string. Hint: we are using a base64 encoded solution for this.

## Questions?

If you have any questions don't hesitate to contact us.
