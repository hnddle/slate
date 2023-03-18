---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - jsx
  - graphql
  - json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/hnddle/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Voiij API
---

# Introduction

Welcome to the Voiij API! You can use our API to access Voiij API endpoints.

반갑습니다!

# Authentication

> To login, use this code:

```graphql
mutation Login($email: String!, $password: String!) {
  login(email: $email, password: $password) {
    accessToken {
			tokenString
			tokenType
		}
		refreshToken {
			tokenString
			tokenType
		}
  }
}
```

> 로그인 토큰

로그인 하려면

`Authorization: bearer exampleiOiJIUzUxMiJ9.eyJzmWIiOiIwOTMxZWNjNy00ZDhkLTRhNjktYjU0MS02ZGJiZTJmMjM0M2UiLCJpYXQiOjE2NzY0NTk5NjEsImV4cCI6MTY3NzA2NDc2MX0.G7yAq6excQ_Kn2i0Rr8mnfHIqscG4uyCEOnf2z91GLYWZ6DIhLt6QBYznceL8iK8TYMyZ-eQRA_6_YQKhMaMLw`

<aside class="notice">
로그인 토큰은 API 전송시 header에 <code>Authorization</code> 값으로 전달해야 합니다. (토큰 없이도 로그인 불필요한 API는 사용 가능)
</aside>

# Kittens

## Get All Kittens

```jsx
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

`query getSimpleRecommendedProductsByCursor`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```jsx
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

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

