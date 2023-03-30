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

Voiij 회원은 로그인 후 header에 Authorization 토큰을 넣어 회원 전용 기능을 이용할 수 있습니다.

토큰 예시

`Authorization: bearer exampleiOiJIUzUxMiJ9.eyJzmWIiOiIwOTMxZWNjNy00ZDhkLTRhNjktYjU0MS02ZGJiZTJmMjM0M2UiLCJpYXQiOjE2NzY0NTk5NjEsImV4cCI6MTY3NzA2NDc2MX0.G7yAq6excQ_Kn2i0Rr8mnfHIqscG4uyCEOnf2z91GLYWZ6DIhLt6QBYznceL8iK8TYMyZ-eQRA_6_YQKhMaMLw`

<aside class="notice">
로그인 토큰은 API 전송시 header에 <code>Authorization</code> 값으로 전달해야 합니다. 
(다만 토큰이 없어도 로그인이 불필요한 API는 사용 가능)
</aside>

# 사용자 (User)

## 회원가입 (Signup)

> Mutation:

```graphql
mutation Signup($user: UserInput!) {
  signup(user: $user)
}
```

> 결과값

```graphql
{
	"data": {
		"signup": "success"
	}
}
```

### UserInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
email | String | Y | 이메일
name | String | Y | 이름(닉네임)
password | String | Y | 비밀번호 (최소 8자, 최대 20자, 영어/숫자/특수문자 포함, 공백 입력 불가)

## 로그인 (Login)

> Mutation:

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

> 결과값

```json
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

### Input

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
email | String | Y | 이메일
password | String | Y | 비밀번호


### LoginToken

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
tokenString | String | Y | 토큰 값
tokenType | String | Y | 토큰 헤더 (Bearer)



## 내 정보 조회 (GetMyInfo)

> Query:

```graphql
query getMyInfo{
	id
	isVerified
	email
	name
	profileImageUrl
	Addresses {
		isDefaultAddress
		recipientName
		countryCode
		address1
		address2
	}
	isSeller
}
```

> 결과값

```json
{
	"data": {
		"getMyInfo": {
			"id": "6595f665-c21d-48ae-912f-562a5080fbf2",
			"isVerified": true,
			"email": "jmlee@voiij.co.kr",
			"name": "jmlee",
			"profileImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"Addresses": [
				{
				"isDefaultAddress": true,
                "address1": "서울 강서구 강서로37길 77",
                "address2": "지명아트빌라 201호",
                "postalCode": "07710",
                "countryCode": "KR",
                }
			],
			"isSeller": false
		}
	}
}
```

### User

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
id | String | Y | 사용자 ID
isVerified | Boolean | Y | 사용자 인증 완료 여부
email | String | Y | 이메일
name | String | Y | 이름(닉네임)
profileImageUrl | String | N | 프로필 사진 url
Addresses | [Address] | Y | 사용자 배송 주소
isSeller | String | Y | 판매자 여부

### Address

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
isDefaultAddress | String | Y | 대표 주소 여부
recipientName | Boolean | Y | 수령인 이름
countryCode | String | Y | 국가 코드
address1 | String | Y | 주소1
address2 | String | Y | 주소2
postalCode | String | N | 한국 (postalCode)
city | String | N | 미국, 영국 (city)
county | String | N | 영국 (county)
postCode | String | N | 영국 (postCode)
state | String | N | 미국 (state)
zipCode | String | N | 미국 (zipCode)

## 이메일 중복 체크 (CheckEmailDuplicate)

> Query:

```graphql
query CheckEmailDuplicate($email: String!) {
  checkEmailDuplicate(email: $email)
}
```

> 결과값

```json
{
	"data": {
		"checkEmailDuplicate": false
	}
}
```

## 닉네임 중복 체크 (CheckNameDuplicate)

> Query:

```graphql
query CheckNameDuplicate($name: String!) {
  checkNameDuplicate(name: $name)
}
```

> 결과값

```json
{
	"data": {
		"checkNameDuplicate": false
	}
}
```

## 로그인 토큰 유효성 체크 (CheckLoginTokenValid)

> Query:

```graphql
query CheckLoginTokenValid {
	checkLoginTokenValid
}
```

> 결과값

```json
{
	"data": {
		"checkLoginTokenValid": true
	}
}
```

## Refresh 토큰 로그인, 로그인 토큰 연장 (LoginWithRefreshToken)

> Mutation:

```graphql
mutation LoginWithRefreshToken($refreshToken: JWTTokenInput!) {
	loginWithRefreshToken(refreshToken : $refreshToken) {
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

> 결과값

```json
{
	"data": {
		"loginWithRefreshToken": {
			"accessToken": {
				"tokenString": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIyYTgwYjdiNC0wMjQ1LTQwZGUtOTRiOS0xODE5OTc0NzBjNDkiLCJpYXQiOjE2ODAxNjcyNDMsImV4cCI6MTY4MDc3MjA0M30.OmWGEDloujwusHHKp32Qvk90os9yEvRsM5kXaXG0CsUEPveLrFud9hMDF-AEy6lyKm2Nj4INhys6DCxSTH6dYw",
				"tokenType": "Bearer "
			},
			"refreshToken": {
				"tokenString": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIyYTgwYjdiNC0wMjQ1LTQwZGUtOTRiOS0xODE5OTc0NzBjNDkiLCJpYXQiOjE2ODAxNjcyNDMsImV4cCI6MTY4Mjc1OTI0M30.c9C3CtWd2HAI2VwrJR8QAcq3RzW0OiMOIqlhYpGiqsou4CXsm1DCi7qAN1dBMGLJ9m5oze-kMy7Upr7BptoNkg",
				"tokenType": "Bearer "
			}
		}
	}
}
```

## 비밀번호 변경 (UpdatePassword)

> Mutation:

```graphql
mutation UpdatePassword($password: String!, $newPassword: String!) {
  updatePassword(password: $password, newPassword: $newPassword)
}
```

> 결과값

```json
{
	"data": {
		"updatePassword": "success"
	}
}
```

## 닉네임 변경 (CheckLoginTokenValid)

> Mutation:

```graphql
mutation UpdateName($name: String!) {
  updateName(name: $name)
}
```

> 결과값

```json
{
	"data": {
		"updateName": "success"
	}
}
```

## 이메일 인증 (VerifyEmail)

> Mutation:

```graphql
mutation VerifyEmail($userId: String!, $authCode: String!) {
  verifyEmail(userId: $userId, authCode: $authCode)
}
```

> 결과값

```json
{
	"data": {
		"verifyEmail": "success"
	}
}
```

## 회원 탈퇴 (DeleteUserAccount)

로직 수정 필요! (현재 에러 발생)

> Mutation:

```graphql
mutation DeleteUserAccount {
  deleteUserAccount 
}
```

> 결과값

```json
{
	"data": {
		"deleteUserAccount": "success"
	}
}
```

## 사용자 프로필 사진 변경 (ChangeUserProfileImage)

별도 통합/단위 테스트 필요!

> Mutation:

```graphql
mutation ChangeUserProfileImage($userProfileImage: Upload!) {
  changeUserProfileImage(userProfileImage: $userProfileImage)
}
```

> 결과값

```json
{
	"data": {
		"changeUserProfileImage": "success"
	}
}
```

## 사용자 주소 추가 (AddUserAddress)

현재 일부 데이터 저장 안되는 버그 있음!

> Mutation:

```graphql
mutation AddUserAddress($isDefault: Boolean!, $address: AddressInput!) {
  addUserAddress(isDefault: $isDefault, address: $address)
}
```

> 결과값

```graphql
{
	"data": {
		"addUserAddress": "2a80b7b4-0245-40de-94b9-181997470c49"
	}
}
```

### AddressInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
recipientName | Boolean | Y | 수령인 이름
countryCode | String | Y | 국가 코드
address1 | String | Y | 주소1
address2 | String | Y | 주소2
postalCode | String | N | 한국 (postalCode)
city | String | N | 미국, 영국 (city)
county | String | N | 영국 (county)
postCode | String | N | 영국 (postCode)
state | String | N | 미국 (state)
zipCode | String | N | 미국 (zipCode)

## 사용자 주소 수정 (EditUserAddress)

현재 일부 데이터 저장 안되는 버그 있음!

> Mutation:

```graphql
mutation EditUserAddress($addressId: String!, $isDefault: Boolean!, $address: AddressInput!) {
  editUserAddress(
    addressId: $addressId
    isDefault: $isDefault
    address: $address
  )
}
```

> 결과값

```graphql
{
	"data": {
		"editUserAddress": "2a80b7b4-0245-40de-94b9-181997470c49"
	}
}
```

### AddressInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
recipientName | Boolean | Y | 수령인 이름
countryCode | String | Y | 국가 코드
address1 | String | Y | 주소1
address2 | String | Y | 주소2
postalCode | String | N | 한국 (postalCode)
city | String | N | 미국, 영국 (city)
county | String | N | 영국 (county)
postCode | String | N | 영국 (postCode)
state | String | N | 미국 (state)
zipCode | String | N | 미국 (zipCode)

## 사용자 주소 삭제 (DeleteUserAddress)

현재 일부 데이터 저장 안되는 버그 있음!

> Mutation:

```graphql
mutation DeleteUserAddress($addressId: String!) {
  deleteUserAddress(addressId: $addressId)
}
```

> 결과값

```graphql
{
	"data": {
		"deleteUserAddress": "2a80b7b4-0245-40de-94b9-181997470c49"
	}
}
```

## 사용자 기본 주소 설정 (SetDefaultUserAddress)

> Mutation:

```graphql
mutation SetDefaultUserAddress($addressId: String!) {
  setDefaultUserAddress(addressId: $addressId)
}
```

> 결과값

```graphql
{
	"data": {
		"setDefaultUserAddress": "2a80b7b4-0245-40de-94b9-181997470c49"
	}
}
```

## 비밀번호 초기화 메일 전송 (SendPasswordResetMail)

테스트 필요!

> Mutation:

```graphql
mutation SendPasswordResetMail($email: String!) {
	sendPasswordResetMail(email: $email)
}
```

> 결과값

```graphql
{
	"data": {
		"sendPasswordResetMail": "비밀번호 찾기 메일이 성공적으로 전송되었습니다."
	}
}
```

## 비밀번호 초기화 (ResetPassword)

테스트 필요!

> Mutation:

```graphql
mutation ResetPassword($email: String!, $passwordResetCode: String!, 
	$newPassword: String!) {
		
	resetPassword(email: $email, 
		passwordResetCode: $passwordResetCode, 
		newPassword: $newPassword)
}
```

> 결과값

```graphql
{
	"data": {
		"resetPassword": "success"
	}
}
```

# 판매자 (Seller)

## 판매자 등록 (RegisterSeller)

> Mutation:

```graphql
mutation RegisterSeller($sellerName: String!) {
  registerSeller(sellerName: $sellerName)
}
```

> 결과값

```graphql
{
	"data": {
		"registerSeller": "success"
	}
}
```

## 판매자 등록 (DeleteSeller)

현재 에러! 기능 수정 필요 (체크해야할 데이터가 많기 때문)

> Mutation:

```graphql
mutation DeleteSeller {
  deleteSeller
}
```

> 결과값

```graphql
{
	"data": {
		"deleteSeller": "success"
	}
}
```

# 장바구니 (Cart)

## 비밀번호 초기화 (ResetPassword)

테스트 필요!

> Mutation:

```graphql
mutation ResetPassword($email: String!, $passwordResetCode: String!, 
	$newPassword: String!) {
		
	resetPassword(email: $email, 
		passwordResetCode: $passwordResetCode, 
		newPassword: $newPassword)
}
```

> 결과값

```graphql
{
	"data": {
		"resetPassword": "success"
	}
}
```
