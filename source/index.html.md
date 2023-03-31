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
                "address1": "주소1",
                "address2": "주소2",
                "postalCode": "01234",
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

# 라이선스 (License)

## 라이선스 등록 (registerLicense)

> Mutation:

```graphql
mutation RegisterLicense($license: LicenseInput!) {
  registerLicense(license: $license)
}
```

> 결과값

```graphql
{
	"data": {
		"registerLicense": "success"
	}
}
```

### LicenseInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
isEnabled | Boolean | Y | 활성화 여부
isApprovalRequired | String | N | 승인 필요 여부 (미사용)
allowedBusinessTypes | [String] | N | 허용 사업 형태 (미사용)
representativeImageUrl | String | Y | 대표 이미지 url
thumbnailImageUrl | String | Y | 썸네일 이미지 url
minimumGuarantee | Int | N | 라이선스 MG 수수료 (미사용)
runningGuaranteeRate | Int | Y | 라이선스 RG 수수료
name | String | Y | 라이선스명
shortDescription | String | Y | 짧은 설명글
tags | [String] | N | 태그
descriptionHtml | String | Y | 설명 html
guideLine | String | N | 라이선스 가이드라인
endPeriod | DateTime | N | 라이선스 종료일 (무기한 == NULL)

## 라이선스 수정 (updateLicense)

> Mutation:

```graphql
mutation UpdateLicense($licenseId: String!, $license: LicenseInput!) {
  updateLicense(licenseId: $licenseId, license: $license)
}
```

> 결과값

```graphql
{
	"data": {
		"updateLicense": "success"
	}
}
```

### LicenseInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
isEnabled | Boolean | Y | 활성화 여부
isApprovalRequired | String | N | 승인 필요 여부 (미사용)
allowedBusinessTypes | [String] | N | 허용 사업 형태 (미사용)
representativeImageUrl | String | Y | 대표 이미지 url
thumbnailImageUrl | String | Y | 썸네일 이미지 url
minimumGuarantee | Int | N | 라이선스 MG 수수료 (미사용)
runningGuaranteeRate | Int | Y | 라이선스 RG 수수료
name | String | Y | 라이선스명
shortDescription | String | Y | 짧은 설명글
tags | [String] | N | 태그
descriptionHtml | String | Y | 설명 html
guideLine | String | N | 라이선스 가이드라인
endPeriod | DateTime | N | 라이선스 종료일 (무기한 == NULL)

## 라이선스 삭제 (deleteLicense)

상품 판매 정지 개발 필요 (사유까지)

> Mutation:

```graphql
mutation DeleteLicense($licenseId: String!, $license: LicenseInput!) {
  deleteLicense(licenseId: $licenseId, license: $license)
}
```

> 결과값

```graphql
{
	"data": {
		"deleteLicense": "success"
	}
}
```

## 라이선스 설명 에디터 이미지 업로드 (UploadLicenseHtmlImage)

> Mutation:

```graphql
mutation UploadLicenseHtmlImage($image: Upload!) {
  uploadLicenseHtmlImage(image: $image)
}
```

> 결과값

```json
{
	"data": {
		"uploadLicenseHtmlImage": "success"
	}
}
```

## 라이선스 대표 이미지 업로드 (UploadLicenseRepresentativeImage)

> Mutation:

```graphql
mutation UploadLicenseRepresentativeImage($image: Upload!) {
  uploadLicenseRepresentativeImage(image: $image) {
	representativeImageUrl
	thumbnailImageUrl
  }
}
```

> 결과값

```json
{
	"data": {
		"uploadLicenseRepresentativeImage": {
			"representativeImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"thumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png"
		}
	}
}
```

# 판매자 (Seller)

## 판매자 가입/등록 (RegisterSeller)

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

### SellerInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
country | String | Y | 국가코드
businessType | String | Y | 사업 종류


## 판매자 탈퇴 (DeleteSeller)

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

# 상품 (Product)

## 임시 상품 추천 (GetSimpleRecommendedProductsByCursor)

> Query:

```graphql
query GetSimpleRecommendedProductsByCursor(
	$currency: String!, 
	$shippingCountry: String!) {
  getSimpleRecommendedProductsByCursor(
		currency: $currency,
		shippingCountry: $shippingCountry) {
			id
			name
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			currency
			shippingCountries
			stockQuantity
			thumbnailImageUrl
			representativeImageUrl
			additionalImageUrls
			descriptionHtml
			isOnSale
			isAdultProduct
			isDeliveryBundled
			licenseThumbnailImageUrl
			licenseName
			sellerBusinessType
			sellerCountry
			sellerName
			sellerProfileImageUrl
			tags
			numberOfSales
			numberOfLikes
			averageReviewScore
			numberOfReviews
			createdDate
			modifiedDate
		}
}
```

> 결과값

```json
{
	"data": {
		"getSimpleRecommendedProductsByCursor": [
			{
				"id": "be465a41-d3cd-456e-940a-119760f2f638",
				"name": "제품명",
				"listPrice": 200000,
				"salePrice": 100000,
				"isDiscountApplied": true,
				"discountRate": 50.0,
				"currency": "KRW",
				"shippingCountries": [
					"KR"
				],
				"stockQuantity": 10,
				"thumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"representativeImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"additionalImageUrls": [
					"https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png"
				],
				"descriptionHtml": "",
				"isOnSale": false,
				"isAdultProduct": false,
				"isDeliveryBundled": true,
				"licenseThumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"licenseName": "라이선스",
				"sellerBusinessType": "INDIVIDUAL",
				"sellerCountry": "KR",
				"sellerName": "jmlee",
				"sellerProfileImageUrl": null,
				"tags": [
					"tag1",
					"tag2"
				],
				"numberOfSales": 0,
				"numberOfLikes": 0,
				"averageReviewScore": 0.0,
				"numberOfReviews": 0,
				"createdDate": "2023-03-31T12:27:25.139665",
				"modifiedDate": "2023-03-31T12:27:25.164689"
			}
		]
	}
}
```

## 상품 검색 (SearchProducts)

오류 수정 필요! 기능 완성/구현 필요!

> Query:

```graphql
query SearchProducts(
	$shippingCountry: String!, 
	$currency: String!,
	$searchString: String!, 
	$minPrice: Int, 
	$maxPrice: Int, 
	$pageNumber: Int) {
  searchProducts(
		shippingCountry: $shippingCountry,
    currency: $currency,
    searchString: $searchString,
    minPrice: $minPrice,
    maxPrice: $maxPrice,
    pageNumber: $pageNumber
  ) {
			id
			name
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			currency
			shippingCountries
			stockQuantity
			thumbnailImageUrl
			representativeImageUrl
			additionalImageUrls
			descriptionHtml
			isOnSale
			isAdultProduct
			isDeliveryBundled
			licenseThumbnailImageUrl
			licenseName
			sellerBusinessType
			sellerCountry
			sellerName
			sellerProfileImageUrl
			tags
			numberOfSales
			numberOfLikes
			averageReviewScore
			numberOfReviews
			createdDate
			modifiedDate
		}
}
```

> 결과값

```json
{
	"data": {
		"searchProducts": [
			{
				"id": "be465a41-d3cd-456e-940a-119760f2f638",
				"name": "제품명",
				"listPrice": 200000,
				"salePrice": 100000,
				"isDiscountApplied": true,
				"discountRate": 50.0,
				"currency": "KRW",
				"shippingCountries": [
					"KR"
				],
				"stockQuantity": 10,
				"thumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"representativeImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"additionalImageUrls": [
					"https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png"
				],
				"descriptionHtml": "",
				"isOnSale": false,
				"isAdultProduct": false,
				"isDeliveryBundled": true,
				"licenseThumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
				"licenseName": "라이선스",
				"sellerBusinessType": "INDIVIDUAL",
				"sellerCountry": "KR",
				"sellerName": "jmlee",
				"sellerProfileImageUrl": null,
				"tags": [
					"tag1",
					"tag2"
				],
				"numberOfSales": 0,
				"numberOfLikes": 0,
				"averageReviewScore": 0.0,
				"numberOfReviews": 0,
				"createdDate": "2023-03-31T12:27:25.139665",
				"modifiedDate": "2023-03-31T14:55:12.584047"
			}
		]
	}
}
```

## 상품 목록 단건 조회 (GetProductView)

> Query:

```graphql
query GetProductView(
	$currency: String!, 
	$shippingCountry: String!,
	$productId: String!) {
  getProductView(
    currency: $currency,
		shippingCountry: $shippingCountry,
		productId: $productId) {
			id
			name
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			currency
			shippingCountries
			stockQuantity
			thumbnailImageUrl
			representativeImageUrl
			additionalImageUrls
			descriptionHtml
			isOnSale
			isAdultProduct
			isDeliveryBundled
			licenseThumbnailImageUrl
			licenseName
			sellerBusinessType
			sellerCountry
			sellerName
			sellerProfileImageUrl
			tags
			numberOfSales
			numberOfLikes
			averageReviewScore
			numberOfReviews
			createdDate
			modifiedDate
		}
}
```

> 결과값

```json
{
	"data": {
		"getProductView": {
			"id": "be465a41-d3cd-456e-940a-119760f2f638",
			"name": "제품명",
			"listPrice": 200000,
			"salePrice": 100000,
			"isDiscountApplied": true,
			"discountRate": 50.0,
			"currency": "KRW",
			"shippingCountries": [
				"KR"
			],
			"stockQuantity": 10,
			"thumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"representativeImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"additionalImageUrls": [
				"https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png"
			],
			"descriptionHtml": "",
			"isOnSale": false,
			"isAdultProduct": false,
			"isDeliveryBundled": true,
			"licenseThumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"licenseName": "라이선스",
			"sellerBusinessType": "INDIVIDUAL",
			"sellerCountry": "KR",
			"sellerName": "jmlee",
			"sellerProfileImageUrl": null,
			"tags": [
				"tag1",
				"tag2"
			],
			"numberOfSales": 0,
			"numberOfLikes": 0,
			"averageReviewScore": 0.0,
			"numberOfReviews": 0,
			"createdDate": "2023-03-31T12:27:25.139665",
			"modifiedDate": "2023-03-31T12:27:25.164689"
		}
	}
}
```

## 상품 추가 (AddProduct)

> Mutation:

```graphql
mutation AddProduct(
	$product: ProductInput!) {
  addProduct(product: $product)
}
```

> 결과값

```json
{
	"data": {
		"addProduct": "success"
	}
}
```

### ProductInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
isOnSale | Boolean | Y | 판매 여부
isAdultProduct | Boolean | Y | 성인 상품 여부
isDeliveryBundled | Boolean | Y | 묶음 상품 여부
stockQuantity | Int | Y | 재고 수량
name | String | Y | 제품명
tags | [String] | N | 태그
representativeImageUrl | String | Y | 대표 이미지 Url
thumbnailImageUrl | String | Y | 썸네일 이미지 Url
additionalImageUrls | String | N | 추가 이미지 Url
descriptionHtml | String | Y | 상품 설명 HTML
licenseId | String | Y | 사용 라이선스 ID
productPriceInputs | [ProductPriceInput] | Y | 상품 가격 정보

### ProductPriceInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
listPrice | Int | Y | 정가
salePrice | Int | Y | 판매가
currency | String | Y | 통화 코드
productDeliveryInputs | [ProductDeliveryInput] | Y | 상품 배송 정보

### ProductDeliveryInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
countryCode | String | Y | 국가 코드
shippingFee | Int | Y | 배송비
deliveryLeadTime | String | Y | 배송 소요 시간

## 상품 수정 (UpdateProduct)

> Mutation:

```graphql
mutation UpdateProduct($productId: String!, $product: ProductInput!) {
  updateProduct(
		productId: $productId
    product: $product
  )
}
```

> 결과값

```json
{
	"data": {
		"updateProduct": "success"
	}
}
```

### ProductInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
isOnSale | Boolean | Y | 판매 여부
isAdultProduct | Boolean | Y | 성인 상품 여부
isDeliveryBundled | Boolean | Y | 묶음 상품 여부
stockQuantity | Int | Y | 재고 수량
name | String | Y | 제품명
tags | [String] | N | 태그
representativeImageUrl | String | Y | 대표 이미지 Url
thumbnailImageUrl | String | Y | 썸네일 이미지 Url
additionalImageUrls | String | N | 추가 이미지 Url
descriptionHtml | String | Y | 상품 설명 HTML
licenseId | String | Y | 사용 라이선스 ID
productPriceInputs | [ProductPriceInput] | Y | 상품 가격 정보

### ProductPriceInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
listPrice | Int | Y | 정가
salePrice | Int | Y | 판매가
currency | String | Y | 통화 코드
productDeliveryInputs | [ProductDeliveryInput] | Y | 상품 배송 정보

### ProductDeliveryInput

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
countryCode | String | Y | 국가 코드
shippingFee | Int | Y | 배송비
deliveryLeadTime | String | Y | 배송 소요 시간

## 상품 삭제 (DeleteProduct)

> Mutation:

```graphql
mutation DeleteProduct($productId: String!) {
  deleteProduct(productId: $productId)
}
```

> 결과값

```json
{
	"data": {
		"deleteProduct": "success"
	}
}
```

## 상품 설명 에디터 이미지 업로드 (UploadProductHtmlImage)

> Mutation:

```graphql
mutation UploadProductHtmlImage($image: Upload!) {
  uploadProductHtmlImage(image: $image)
}
```

> 결과값

```json
{
	"data": {
		"uploadProductHtmlImage": "success"
	}
}
```

## 상품 대표 이미지 업로드 (UploadProductRepresentativeImage)

> Mutation:

```graphql
mutation UploadProductRepresentativeImage($image: Upload!) {
  uploadProductRepresentativeImage(image: $image) {
	representativeImageUrl
	thumbnailImageUrl
  }
}
```

> 결과값

```json
{
	"data": {
		"uploadProductRepresentativeImage": {
			"representativeImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png",
			"thumbnailImageUrl": "https://d3otocwgekczt7.cloudfront.net/upload/image/users/0931ecc7-4d8d-4a69-b541-6dbbe2f2343e/e59e76b5-3011-4599-b3de-3885b1e32c2d.png"
		}
	}
}
```

## 상품 추가 이미지 업로드 (UploadProductAdditionalImage)

> Mutation:

```graphql
mutation UploadProductAdditionalImage($image: Upload!) {
  uploadProductAdditionalImage(image: $image)
}
```

> 결과값

```json
{
	"data": {
		"uploadProductAdditionalImage": "success"
	}
}
```

## 상품 추가 이미지 여러건 업로드 (UploadProductAdditionalImages)

> Mutation:

```graphql
mutation UploadProductAdditionalImages($images: [Upload!]!) {
  uploadProductAdditionalImages(images: $images)
}
```

> 결과값

```json
{
	"data": {
		"uploadProductAdditionalImages": "success"
	}
}
```

# 장바구니 (Cart)

## 장바구니 조회 (GetCartView)

테스트 필요!

> Query:

```graphql
query GetCartView(
	$language: String!, 
	$currency: String!,
	$shippingCountry: String!) {
  getCartView(
		currency: $currency,
		language: $language,
    shippingCountry: $shippingCountry) {
		#장바구니 상품 목록
    currency
    shippingCountry
    totalCartPrice
    cartViewItems {
      id
      productThumbnailImageUrl
      productName
			quantity
      stockQuantity
      sellerName
			sellerProfileImageUrl
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			totalProductPrice
			isCurrencyAvailable
			isShippingAvailable
    }
  }
}
```

> 결과값

```graphql
{
	"data": {
		"getCartView": {
			"currency": "KRW",
			"shippingCountry": "KR",
			"totalCartPrice": 200000,
			"cartViewItems": [
				{
					"id": "3255f15b-52f1-49d7-a139-9851bcde43e9",
					"productThumbnailImageUrl": "be465a41-d3cd-456e-940a-119760f2f638",
					"productName": "제품명",
					"quantity": 2,
					"stockQuantity": 10,
					"sellerName": "jmlee",
					"sellerProfileImageUrl": null,
					"listPrice": 200000,
					"salePrice": 100000,
					"isDiscountApplied": true,
					"discountRate": 50.0,
					"totalProductPrice": 200000,
					"isCurrencyAvailable": true,
					"isShippingAvailable": true
				}
			]
		}
	}
}
```

### CartView

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
cartViewItems | [CartViewItem] | N | 장바구니 상품
currency | String | N | 통화 코드
shippingCountry | String | N | 배송 국가 코드
totalCartPrice | Int | N | 장바구니 총 금액

### CartViewItem

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
id | String | N | 정가
productThumbnailImageUrl | String | N | 판매가
productName | String | N | 통화 코드
quantity | Int | N | 상품 배송 정보
stockQuantity | Int | N | 정가
sellerName | String | N | 판매가
sellerProfileImageUrl | String | N | 통화 코드
listPrice | Int | N | 상품 배송 정보
salePrice | Int | N | 정가
isDiscountApplied | Boolean | N | 통화 코드
discountRate | Float | N | 상품 배송 정보
totalProductPrice | Int | N | 정가
isCurrencyAvailable | Boolean | N | 판매가
isShippingAvailable | Boolean | N | 통화 코드

## 장바구니 상품 추가 (AddItemToCart)

테스트 필요!

> Mutation:

```graphql
mutation AddItemToCart(
	$shippingCountry: String!, 
	$currency: String!,
	$productId: String!,
	$quantity: Int!) {
  addItemToCart(
		shippingCountry: $shippingCountry,
    currency: $currency,
    productId: $productId,
    quantity: $quantity)
}
```

> 결과값

```graphql
{
	"data": {
		"addItemToCart": "success"
	}
}
```

### CartView

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
cartViewItems | [CartViewItem] | N | 장바구니 상품
currency | String | N | 통화 코드
shippingCountry | String | N | 배송 국가 코드
totalCartPrice | Int | N | 장바구니 총 금액

### CartViewItem

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
id | String | N | 정가
productThumbnailImageUrl | String | N | 판매가
productName | String | N | 통화 코드
quantity | Int | N | 상품 배송 정보
stockQuantity | Int | N | 정가
sellerName | String | N | 판매가
sellerProfileImageUrl | String | N | 통화 코드
listPrice | Int | N | 상품 배송 정보
salePrice | Int | N | 정가
isDiscountApplied | Boolean | N | 통화 코드
discountRate | Float | N | 상품 배송 정보
totalProductPrice | Int | N | 정가
isCurrencyAvailable | Boolean | N | 판매가
isShippingAvailable | Boolean | N | 통화 코드


## 장바구니 상품 수량 변경 (SetCartItemQuantity)

테스트 필요!

> Mutation:

```graphql
mutation SetCartItemQuantity(
	$shippingCountry: String!, 
	$currency: String!,
	$cartItemId: String!, 
	$quantity: Int!) {
  setCartItemQuantity(
		shippingCountry: $shippingCountry,
    currency: $currency,
		cartItemId: $cartItemId, 
		quantity: $quantity)
}
```

> 결과값

```graphql
{
	"data": {
		"setCartItemQuantity": "success"
	}
}
```

## 장바구니 상품 삭제 (DeleteCartItem)

테스트 필요!

> Mutation:

```graphql
mutation DeleteCartItem($cartItemId: String!) {
  deleteCartItem(cartItemId: $cartItemId)
}
```

> 결과값

```graphql
{
	"data": {
		"deleteCartItem": "success"
	}
}
```

## 장바구니 상품 다수 삭제 (DeleteCartItems)

테스트 필요!

> Mutation:

```graphql
mutation DeleteCartItems($cartItemIds: [String]!) {
  deleteCartItems(cartItemIds: $cartItemIds)
}
```

> 결과값

```graphql
{
	"data": {
		"deleteCartItems": "success"
	}
}
```

# 결제 (Payment)

작성중!

## 장바구니 조회 (GetCartView)

테스트 필요!

> Query:

```graphql
query GetCartView(
	$language: String!, 
	$currency: String!,
	$shippingCountry: String!) {
  getCartView(
		currency: $currency,
		language: $language,
    shippingCountry: $shippingCountry) {
		#장바구니 상품 목록
    currency
    shippingCountry
    totalCartPrice
    cartViewItems {
      id
      productThumbnailImageUrl
      productName
			quantity
      stockQuantity
      sellerName
			sellerProfileImageUrl
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			totalProductPrice
			isCurrencyAvailable
			isShippingAvailable
    }
  }
}
```

> 결과값

```graphql
{
	"data": {
		"getCartView": {
			"currency": "KRW",
			"shippingCountry": "KR",
			"totalCartPrice": 200000,
			"cartViewItems": [
				{
					"id": "3255f15b-52f1-49d7-a139-9851bcde43e9",
					"productThumbnailImageUrl": "be465a41-d3cd-456e-940a-119760f2f638",
					"productName": "제품명",
					"quantity": 2,
					"stockQuantity": 10,
					"sellerName": "jmlee",
					"sellerProfileImageUrl": null,
					"listPrice": 200000,
					"salePrice": 100000,
					"isDiscountApplied": true,
					"discountRate": 50.0,
					"totalProductPrice": 200000,
					"isCurrencyAvailable": true,
					"isShippingAvailable": true
				}
			]
		}
	}
}
```

### CartView

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
cartViewItems | [CartViewItem] | N | 장바구니 상품
currency | String | N | 통화 코드
shippingCountry | String | N | 배송 국가 코드
totalCartPrice | Int | N | 장바구니 총 금액

### CartViewItem

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
id | String | N | 정가
productThumbnailImageUrl | String | N | 판매가
productName | String | N | 통화 코드
quantity | Int | N | 상품 배송 정보
stockQuantity | Int | N | 정가
sellerName | String | N | 판매가
sellerProfileImageUrl | String | N | 통화 코드
listPrice | Int | N | 상품 배송 정보
salePrice | Int | N | 정가
isDiscountApplied | Boolean | N | 통화 코드
discountRate | Float | N | 상품 배송 정보
totalProductPrice | Int | N | 정가
isCurrencyAvailable | Boolean | N | 판매가
isShippingAvailable | Boolean | N | 통화 코드

# 주문 (Order)

작성중!

## 장바구니 조회 (GetCartView)

테스트 필요!

> Query:

```graphql
query GetCartView(
	$language: String!, 
	$currency: String!,
	$shippingCountry: String!) {
  getCartView(
		currency: $currency,
		language: $language,
    shippingCountry: $shippingCountry) {
		#장바구니 상품 목록
    currency
    shippingCountry
    totalCartPrice
    cartViewItems {
      id
      productThumbnailImageUrl
      productName
			quantity
      stockQuantity
      sellerName
			sellerProfileImageUrl
			listPrice
			salePrice
			isDiscountApplied
			discountRate
			totalProductPrice
			isCurrencyAvailable
			isShippingAvailable
    }
  }
}
```

> 결과값

```graphql
{
	"data": {
		"getCartView": {
			"currency": "KRW",
			"shippingCountry": "KR",
			"totalCartPrice": 200000,
			"cartViewItems": [
				{
					"id": "3255f15b-52f1-49d7-a139-9851bcde43e9",
					"productThumbnailImageUrl": "be465a41-d3cd-456e-940a-119760f2f638",
					"productName": "제품명",
					"quantity": 2,
					"stockQuantity": 10,
					"sellerName": "jmlee",
					"sellerProfileImageUrl": null,
					"listPrice": 200000,
					"salePrice": 100000,
					"isDiscountApplied": true,
					"discountRate": 50.0,
					"totalProductPrice": 200000,
					"isCurrencyAvailable": true,
					"isShippingAvailable": true
				}
			]
		}
	}
}
```

### CartView

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
cartViewItems | [CartViewItem] | N | 장바구니 상품
currency | String | N | 통화 코드
shippingCountry | String | N | 배송 국가 코드
totalCartPrice | Int | N | 장바구니 총 금액

### CartViewItem

변수명 | 자료형 | 필수 여부 | 설명
--------- | ------- | ----------- | ----------
id | String | N | 정가
productThumbnailImageUrl | String | N | 판매가
productName | String | N | 통화 코드
quantity | Int | N | 상품 배송 정보
stockQuantity | Int | N | 정가
sellerName | String | N | 판매가
sellerProfileImageUrl | String | N | 통화 코드
listPrice | Int | N | 상품 배송 정보
salePrice | Int | N | 정가
isDiscountApplied | Boolean | N | 통화 코드
discountRate | Float | N | 상품 배송 정보
totalProductPrice | Int | N | 정가
isCurrencyAvailable | Boolean | N | 판매가
isShippingAvailable | Boolean | N | 통화 코드