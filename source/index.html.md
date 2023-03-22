---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - graphql

toc_footers:
  - Voiij API 문서입니다.
  - <a href='https://voiij.kr'>2023. Voiij Corp. All Rights Reserved.</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Voiij API
---

# 개요

반갑습니다! Voiij API 입니다.

## 인증 토큰

Voiij에 회원가입이 완료된 회원은 로그인 후 header에 Authorization 토큰을 넣어 회원 전용 기능을 이용할 수 있습니다.

토큰 예시

`Authorization: bearer exampleiOiJIUzUxMiJ9.eyJzmWIiOiIwOTMxZWNjNy00ZDhkLTRhNjktYjU0MS02ZGJiZTJmMjM0M2UiLCJpYXQiOjE2NzY0NTk5NjEsImV4cCI6MTY3NzA2NDc2MX0.G7yAq6excQ_Kn2i0Rr8mnfHIqscG4uyCEOnf2z91GLYWZ6DIhLt6QBYznceL8iK8TYMyZ-eQRA_6_YQKhMaMLw`

<aside class="notice">
로그인 토큰은 API 전송시 header에 <code>Authorization</code> 값으로 전달해야 합니다. (토큰이 없어도 로그인이 불필요한 API는 사용 가능)
</aside>

# 사용자 계정

## 회원 가입

> 회원가입 Mutation:

```
mutation Signup($user: UserInput!) {
  signup(user: $user)
}
```

> 결과값 예시

```
{
	"data": {
		"signup": "success"
	}
}
```

### UserInput 객체

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
email | String | Y | 이메일
name | String | Y | 이름(닉네임)
password | String | Y | 비밀번호 (최소 8자, 최대 20자, 영어/숫자/특수문자 포함, 공백 입력 불가)

## 로그인

> 로그인 Mutation:

```
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

> 결과값 예시

```
{
	"data": {
		"login": {
			"accessToken": {
				"tokenString": "bearer exampleiOiJIUzUxMiJ9.eyJzmWIiOiIwOTMxZWNjNy00ZDhkLTRhNjktYjU0MS02ZGJiZTJmMjM0M2UiLCJpYXQiOjE2NzY0NTk5NjEsImV4cCI6MTY3NzA2NDc2MX0.G7yAq6excQ_Kn2i0Rr8mnfHIqscG4uyCEOnf2z91GLYWZ6DIhLt6QBYznceL8iK8TYMyZ-eQRA_6_YQKhMaMLw",
				"tokenType": "Bearer "
			},
			"refreshToken": {
				"tokenString": "bearer exampleiOiJIUzUxMiJ9.eyJzmWIiOiIwOTMxZWNjNy00ZDhkLTRhNjktYjU0MS02ZGJiZTJmMjM0M2UiLCJpYXQiOjE2NzY0NTk5NjEsImV4cCI6MTY3NzA2NDc2MX0.G7yAq6excQ_Kn2i0Rr8mnfHIqscG4uyCEOnf2z91GLYWZ6DIhLt6QBYznceL8iK8TYMyZ-eQRA_6_YQKhMaMLw",
				"tokenType": "Bearer "
			}
		}
	}
}
```

**입력값**

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
email | String | Y | 이메일
password | String | Y | 비밀번호


결과값 (accessToken, refreshToken)

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
tokenString | String | Y | 토큰 값
tokenType | String | Y | 토큰 헤더 (Bearer)



## 내 정보 조회

UserInput 객체

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | -----------
email | String | Y | 이메일
name | String | Y | 이름(닉네임)
password | String | Y | 비밀번호 (최소 8자, 최대 20자, 영어/숫자/특수문자 포함, 공백 입력 불가)

> 회원가입 Mutation:

```
mutation Signup($user: UserInput!) {
  signup(user: $user)
}
```

> 결과값 예시

```
{
	"data": {
		"signup": "success"
	}
}
```



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

