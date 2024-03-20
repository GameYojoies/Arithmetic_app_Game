# 导航

- [全局数据结构](#全局数据结构-global-data-structure)
- [全局响应码](#全局响应码-response-code)
- [身份验证](#身份验证)
- [HTTP API](#http-api)
    - [帐户相关](#帐户相关-account)
        - [登录](#登录-login)
        - [刷新token](#刷新token-refresh-token)
        - [用户名存在检测](#手机号存在检测-mobile-exist)
        - [手机号存在检测](#注册帐户-sign-up)
        - [Get profile](#get-profile)
        - [注册帐户](#修改密码-change-password)
        - [修改密码](#销毁帐号-destroy-account)
        - [Get rebate history](#get-rebate-history)
        - [Get points](#get-points)
    - [Invite Code](#invite-code)
        - [Get invite code](#get-invite-code)
        - [My team](#my-team)
        - [Get Member With Inviter](#get-member-with-inviter)
    - [2FA](#2fa)
        - [Get 2FA](#get-2fa)
        - [Enable 2FA](#enable-2fa)
        - [Change password 2FA](#change-password-2fa)
    - [Financial Organization](#financial-organization)
        - [Get financial organization](#get-financial-organization)
        - [Create financial organization](#create-financial-organization)
        - [Update financial organization](#update-financial-organization)
    - [Identity](#identity)
        - [Get identity](#get-identity)
        - [Create identity](#create-identity)
    - [Wallet](#wallet)
        - [Get wallet](#get-wallet)
        - [Get wallet history](#get-wallet-history)
        - [Deposit](#get-wallet-deposit)
        - [Withdraw](#withdrawal)
        - [TransferCips](#transfer-cips)
        - [Deposit CNY](#deposit-cny)
        - [Convert](#convert)
    - [Event check in](#event-check-in)
        - [Get event check in](#get-event-check-in)
        - [Create event check in](#create-event-check-in)
    - [Product](#product)
        - [Get product](#get-products)
        - [Create product](#create-product-purchase)
        - [Get Apply product](#apply-product)
        - [Create Apply product](#apply-product)
        - [Get Apply product list](#get-apply-products-list)
    - [Orders](#orders)
        - [Get history](#get-orders-history)
        - [Get queue](#orders-queue)
    - [Announcement](#announcement)
        - [Get all announcement](#get-all-announcement)
        - [Get announcement by id](#get-announcement-by-id)
    - [Exchange rate](#exchange-rate)
        - [Get exchange rate](#get-exchange-rate)
    - [Carousel](#carousel)
        - [Get carousel](#get-carousel)
    - [Files](#Files)
        - [Show file](#show-file)
    - [Notifications](#notifications)
        - [Get notifications](#get-notifications)
        - [Read notifications](#read-notifications)
    - [Bank Info](#bank-info)
        - [Get bank info](#get-bank-info)
        - [Create withdraw CNY](#create-withdraw-cny)
        - [Create deposit CNY](#create-deposit-cny)
    - [Debt settlement fund](#debt-settlement-fund)
        - [Create debt settlement fund](#create-debt-settlement-fund)
    - [Product2]
        - [Get product2](#get-product2)
        - [Create product2](#create-product2)
        - [Get apply history products2](#get-apply-history-products2)

# 全局数据结构 (global data structure)

服务端响应的数据结构，除特别说明，均使用`application/json`的格式响应客户端请求，且响应格式如下:

```json
{
  "code": 0,
  "description": "USERNAME_NOT_FOUND",
  "records": 0,
  "result": {}
}
```

| key         | 类型     | 必须 | 说明                      |
|-------------|--------|----|-------------------------|
| code        | number | 是  | 响应码,参考[全局响应码](#全局响应码)   |
| description | string | 否  | 人类可读的描述                 |
| records     | number | 否  | 若`result`为列表，则表示总记录数    |
| result      | any    | 否  | 业务数据,参考[API](#HTTP-API) |

# 全局响应码 (response code)

| code | 说明              | text                                          |
|------|-----------------|-----------------------------------------------|
| 0    | 请求成功            | succcess                                      |
| 2011 | 请求已受理           | accepted                                      |
| 4001 | token过期         | token expire                                  |
| 4002 | refresh token过期 | refresh token expire                          |
| 4010 | 用户名密码错误         | mobile password                               |
| 4011 | 无效的密码格式         | invalid password                              |
| 4014 | 无效的用户名          | invalid account name                          |
| 4021 | 密码与当前密码不匹配      | password not matches                          |
| 4022 | 用户名已经存在         | existed account name                          |
| 4023 | 手机号已经存在         | existed mobile                                |
| 4024 | 短信验证码不一致        | sms code sample                               |
| 4025 | 已经注销的帐户         | destroy account                               |
| 4029 | 无效的手机号          | invalid mobile                                |
| 4030 |                 | invalid user id                               |
| 4031 |                 | balance not enough                            |
| 4032 |                 | invalid wallet address                        |
| 4033 |                 | invalid amount                                |
| 4034 |                 | withdraw fail                                 |
| 4035 |                 | invalid invite code                           |
| 4036 |                 | invalid financial organization bank name      |
| 4037 |                 | invalid financial organization card number    |
| 4038 |                 | invalid financial organization card name      |
| 4039 |                 | existed financial organization card number    |
| 4040 |                 | invalid name                                  |
| 4041 |                 | invalid identity passport code                |
| 4042 |                 | invalid step                                  |
| 4043 |                 | existed checked in                            |
| 4044 |                 | invalid product id                            |
| 4045 |                 | not allow repeat purchase                     |
| 4046 |                 | purchase method is invalid                    |
| 4047 |                 | product state is invalid                      |
| 4048 |                 | purchase method is not match                  |
| 4049 |                 | product is off                                |
| 4050 | 用户已被禁用          | account disabled                              |
| 4051 |                 | invalid announcement id                       |
| 4052 |                 | invalid wallet type                           |
| 4053 |                 | invalid transaction type                      |
| 4054 |                 | invalid 2fa                                   |
| 4055 |                 | withdraw not enabled                          |
| 4056 |                 | invalid notifications                         |
| 4057 |                 | identity not verified                         |
| 4058 |                 | not found bank name                           |
| 4059 |                 | not found bank info apply type                |
| 4060 |                 | invalid card name                             |
| 4061 |                 | invalid card number                           |
| 4062 |                 | reward debt settlement A not enough           |
| 4063 |                 | reward debt settlement C not enough           |
| 4064 |                 | point not enough                              |
| 4065 |                 | not found debt settlement fund A              |
| 4066 |                 | not found debt settlement fund C              |
| 4067 |                 | not support debt settlement fund A            |
| 4068 |                 | invalid remark                                |
| 4069 |                 | invalid financial organization wallet address |
| 4070 |                 | existed financial organization wallet address |
| 4071 |                 | invalid financial organization type           |
| 9999 | 未知错误            | unknow                                        |

# 身份验证

api服务要求涉及身份类敏感API均需携带用户身份信息:鉴权token，该token遵守[jwt](https://jwt.io)签名标准以及数据格式,由用户登录时签发。
payload结构如下:

```json
{
  "iss": "https://<domain>/v1/auth/login",
  "aud": "ZOeCHxMUODqAgnrHIhRqrFwbQD9r78uk",
  "sub": "7IqqobUHZOeMRPRvIS0cqoYxeZ1ddmdp",
  "iat": 1670840707,
  "exp": 1670840707,
  "attrs": {}
}
```

| key   | 类型     | 简称   | 说明               | 例                                |
|-------|--------|------|------------------|----------------------------------|
| iss   | string | 签发   | 签发此token的机构标识    | `https://<domain>/v1/login`      |
| aud   | string | 接收   | 接收此token的机构或个人标识 | ZOeCHxMUODqAgnrHIhRqrFwbQD9r78uk |
| sub   | string | 接收者  | 通常为用户id          | 7IqqobUHZOeMRPRvIS0cqoYxeZ1ddmdp |
| iat   | number | 签发时间 | 签发该token时的时间     | 1670840707                       |
| exp   | number | 过期时间 | 该token的过期时间      | 1670840707                       |
| attrs | object | 预留   | 后期预留             |                                  |

客户端将用户令牌装载于`request header`的`Authorization`属性

```http request
GET /v1/auth/login

Authorization: xxx
```

# HTTP API

## 帐户相关 (account)

### 登录 (login)

- request

```http request
POST /v1/auth/login
Content-Type: application/json

{
  "mobile": "xxxx",
  "password": "xxxx"
}
```

| key      | type   | require | description |
|----------|--------|---------|-------------|
| mobile   | string | true    | 手机号         |
| password | string | true    | 密码          |

- response

```json
{
  "token": "xxxxx",
  "refresh": "xxxxxxx",
  "expire": 1670840707,
  "refreshExpire": 1670840707
}
```

| key           | type   | require | description  |
|---------------|--------|---------|--------------|
| token         | string | true    | 用于身份验证的令牌    |
| refresh       | string | true    | 刷新令牌专用的令牌    |
| expire        | number | true    | token的过期时间   |
| refreshExpire | number | true    | refresh的过期时间 |

### 刷新token (refresh token)

- request

```http request
POST /v1/auth/refresh
Content-Type: application/json

{
  "token":"xxxxxx"
}
```

- response

```json
{
  "token": "xxxxx",
  "refresh": "xxxxxxx",
  "expire": 1670840707,
  "refreshExpire": 1670840707
}
```

| key           | type   | require | description  |
|---------------|--------|---------|--------------|
| token         | string | true    | 用于身份验证的令牌    |
| refresh       | string | true    | 刷新令牌专用的令牌    |
| expire        | number | true    | token的过期时间   |
| refreshExpire | number | true    | refresh的过期时间 |

### 手机号存在检测 (mobile exist)

检测手机号是否已被注册

- request

```http request
GET /v1/auth/mobile/{mobile}
```

- response

```text
true | false
```

- `true`: 已存在
- `false`: 尚未存在

### 注册帐户 (sign up)

- request

```http request
POST /v1/auth/signUp
Content-Type: application/json

{
  "mobile": "11122223333",
  "name": "xxxx",
  "password": "xxxxx",
  "inviteCode": "xxxxx",
  "passportName": "xxx",
  "passportNumber": "xxx"
}
```

| key            | type   | require | description |
|----------------|--------|---------|-------------|
| mobile         | string | true    | 手机号         |
| name           | string | true    | 用户名         |
| password       | string | true    | 密码          |
| inviteCode     | string | false   | 邀请码         |
| passportName   | string | true    | 护照姓名        |
| passportNumber | string | true    | 护照号码        |

- response

```json
{
  "token": "xxxxx",
  "refresh": "xxxxxxx",
  "expire": 1670840707,
  "refreshExpire": 1670840707
}
```

| key           | type   | require | description  |
|---------------|--------|---------|--------------|
| token         | string | true    | 用于身份验证的令牌    |
| refresh       | string | true    | 刷新令牌专用的令牌    |
| expire        | number | true    | token的过期时间   |
| refreshExpire | number | true    | refresh的过期时间 |

### Get profile

- request

```http request
GET /v1/profile
```

- response

```json
{
  "accountId": 214214,
  "phoneNumber": "xxxxxxxxx",
  "walletAddress": "bjqxxxxxxxxxxxxxxsf6"
}
```

| key           | type   | description    |
|---------------|--------|----------------|
| accountId     | number | account id     |
| phoneNumber   | string | phone number   |
| walletAddress | string | wallet address |

### 修改密码 (change password)

修改当前用户的密码

```http request
POST /v1/profile/password
Content-Type: application/json

{
  "cur": "xxxxx",
  "latest": "xxxxx"
}
```

| key    | type   | require | description |
|--------|--------|---------|-------------|
| cur    | string | true    | 当前密码        |
| latest | string | true    | 新密码         |

### 销毁帐号 (destroy account)

```http request
POST /v1/profile/destroy
```

### Get rebate history

- request

```http request
GET /v1/profile/rebateHistory?page=1&limit=10
```

- response

```json
[
  {
    "date": "2023-08-22",
    "pid": 37,
    "name": "xxx",
    "commission": 15.00000000
  },
  {
    "date": "2566-08-22",
    "pid": 37,
    "name": "xxx",
    "commission": 15.00000000
  }
]
```

| key        | type   | description  |
|------------|--------|--------------|
| date       | string | date         |
| pid        | number | product id   |
| name       | string | product name |
| commission | number | commission   |

### Get points

- request

```http request
GET /v1/profile/points
```

- response

```json
{
  "debt": 7840.00000000,
  "gift": 2000.00000000,
  "activity": 180.00000000,
  "points": 0,
  "activityPercent": 100,
  "debtRate": 100,
  "debtActivity": 0
}
```

| key             | type   | description          |
|-----------------|--------|----------------------|
| debt            | number | debt settlement fund |
| gift            | number | gift                 |
| activity        | number | activity             |
| points          | number | points               |
| activityPercent | number | activity percent     |
| debtRate        | number | rate point to swap   |
| debtActivity    | number | debt activity        |

## invite Code

### Get invite code

get invite code

- request

```http request
GET /v1/inviteCode
```

- response

```json
{
  "inviteCode": "ABC123",
  "link": "http://127.0.0.1/?inviteCode=ABC123"
}
```

| key        | type   | description |
|------------|--------|-------------|
| inviteCode | string | invite code |

### My team

get invite code for team

```http request
GET /v1/inviteCode/myTeam/level{step}
```

- response

```json
[
  {
    "uid": 214940,
    "name": "light",
    "mobile": "700****1"
  },
  {
    "uid": 214941,
    "name": "white",
    "mobile": "700****2"
  }
]
```

| key    | type   | description  |
|--------|--------|--------------|
| uid    | number | user id      |
| name   | string | name         |
| mobile | string | mobile phone |

### Get Member With Inviter

```http request
GET /v1/inviteCode/myTeam/of/inviter{uid}
```

- response

```json
{
  "uid": 999999,
  "name": "vv yojoies",
  "mobile": "09628428360",
  "members": [
    {
      "uid": 214914,
      "name": "Aoffyyojoies",
      "mobile": "5432112345"
    },
    {
      "uid": 214913,
      "name": "Uun Uun",
      "mobile": "06284584491"
    },
    {
      "uid": 214918,
      "name": "nisann",
      "mobile": "82369188888"
    },
    {
      "uid": 215013,
      "name": "vv fix2",
      "mobile": "09628428366"
    }
  ],
  "income": 48947.61904761,
  "commission": 8324.42000000,
  "teamCount": 28,
  "bottomTeamCount": 14
}
```

| key             | type   | description      |
|-----------------|--------|------------------|
| uid             | number | user id          |
| name            | string | name             |
| mobile          | string | mobile phone     |
| members         | array  | members          |
| income          | number | income           |
| commission      | number | commission       |
| teamCount       | number | total team count |
| bottomTeamCount | number | my team count    |

## 2FA

### Get 2FA

- request

```http request
GET /v1/twoFA
```

- response

```json
{
  "status": false
}
```

| key    | type | description |
|--------|------|-------------|
| status | bool | status 2FA  |

- `status`:
    - true - enable
    - false - disable

### Enable 2FA

- request

```http request
POST /v1/twoFA
Content-Type: application/json

{
  "password": "xxxxx"
}
```

| key      | type   | require | description |
|----------|--------|---------|-------------|
| password | string | true    | password    |

### change password 2FA

- request

```http request
PATCH /v1/twoFA
Content-Type: application/json

{
  "oldPassword": "xxxxx",
  "newPassword": "xxxxx"
}
```

| key         | type   | description  |
|-------------|--------|--------------|
| oldPassword | string | old password |
| newPassword | string | new password |

## Financial organization

### Get financial organization

- request

```http request
GET /v1/financialOrganization
```

- response

```json
[
  {
    "id": 3,
    "type": 30,
    "walletAddress": "TSZjxxxxxxxxxxxxVVtc6dJ",
    "bankName": "Wallet address",
    "cardNumber": "",
    "cardName": "xxxxx"
  },
  {
    "id": 2,
    "type": 20,
    "walletAddress": "",
    "bankName": "Alipay",
    "cardNumber": "1234567890",
    "cardName": "xxxxx"
  },
  {
    "id": 1,
    "type": 10,
    "walletAddress": "",
    "bankName": "BANK NAME",
    "cardNumber": "1234567890",
    "cardName": "xxxxx"
  }
]
```

| key           | type   | description    |
|---------------|--------|----------------|
| id            | number | id             |
| type          | number | type           |
| walletAddress | string | wallet address |
| bankName      | string | bank name      |
| cardNumber    | string | card number    |
| cardName      | string | card name      |

- `type`
    - 10 - BANK CARD
    - 20 - ALIPAY
    - 30 - USDT

### create financial organization

- request

```http request
POST /v1/financialOrganization
Content-Type: application/json

{
    "type":"10",
    "walletAddress":"xxxxxxxxxxxx",
    "bankName":"xxxxxxxx",
    "cardNumber":"xxxxxxxxxx",
    "cardName":"xxxxxx"
}
```

- example 1

```http json
{
    "type":"10",
    "bankName":"xxxxxxxx",
    "cardNumber":"xxxxxxxxxx",
    "cardName":"xxxxxx"
}
```

- example 2

```http json
{
    "type":"20",
    "cardNumber":"xxxxxxxxxx",
    "cardName":"xxxxxx"
}
```

- example 3

```http json
{
    "type":"30",
    "walletAddress":"TSZjxxxxxxxxxxxxVVtc6dJ",
    "cardName":"xxxxxx"
}
```

| key           | type   | require | description    |
|---------------|--------|---------|----------------|
| type          | number | true    | type           |
| walletAddress | string | false   | wallet address |
| bankName      | string | false   | bank name      |
| cardNumber    | string | false   | card number    |
| cardName      | string | false   | card name      |

`response code`

- 4036 - invalid financial organization bank name
- 4037 - invalid financial organization card number
- 4038 - invalid financial organization card name
- 4039 - existed financial organization card number
- 4069 - invalid financial organization wallet address
- 4070 - existed financial organization wallet address
- 4071 - invalid financial organization type

### update financial organization

- request

```http request
PATCH /v1/financialOrganization/{id}
Content-Type: application/json

{
  "bankName":"xxxxx",
  "cardNumber":"1234567890",
  "cardName":"xxxxx"
}
```

- example 1 (Bank Card)

```http json
{
  "bankName":"xxxxx",
  "cardNumber":"1234567890",
  "cardName":"xxxxx"
}
```

- example 2 (Alipay)

```http json
{
  "cardNumber":"1234567890",
  "cardName":"xxxxx"
}
```

- example 3 (USDT)

```http json
{
  "walletAddress":"TSZjxxxxxxxxxxxxVVtc6dJ",
  "cardName":"xxxxx"
}
```

| key           | type   | require | description    |
|---------------|--------|---------|----------------|
| bankName      | string | false   | bank name      |
| walletAddress | string | false   | wallet address |
| cardNumber    | string | false   | card number    |
| cardName      | string | false   | card name      |

## Identity

### Get identity

- request

```http request
GET /v1/identity
```

- response

```json
{
  "name": "xxxxx",
  "passportCode": "1234567890123",
  "state": 10
}
```

| key          | type   | description   |
|--------------|--------|---------------|
| name         | string | name          |
| passportCode | string | passport code |
| state        | number | state         |

- `state`
    - 10 - UNVERIFIED
    - 20 - VERIFIED
    - 30 - REJECTED

### create identity

- request

```http request
POST /v1/identity
Content-Type: application/json

{
  "name": "xxxxx",
  "passportCode": "1234567890123"
}
```

| key          | type   | require | description   |
|--------------|--------|---------|---------------|
| name         | string | true    | name          |
| passportCode | string | true    | passport code |

## Wallet

### Get wallet

- request

```http request
GET /v1/wallet
```

- response

```json
[
  {
    "address": "xxxxxxxxxx",
    "balance": 0E-8,
    "fiat": 0,
    "type": 10
  },
  {
    "address": "xxxxxxxxxx",
    "balance": 0,
    "fiat": 0,
    "type": 20
  }
]
```

| key     | type   | description                     |
|---------|--------|---------------------------------|
| address | string | address wallet                  |
| balance | number | balance in wallet (cips - USDT) |
| fiat    | number | fiat (USDT - CNY)               |
| type    | number | type wallet                     |

- `type`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY

### Deposit CNY

- request

```http request
GET /v1/wallet/payment?amount={amount}&method={method}
```

- use payUrl to open browser to pay
- example

```http request
GET /v1/wallet/payment?amount=100&method=900
```

- response

```json
[
  {
    "code": 0,
    "description": null,
    "result": {
      "retCode": "0",
      "retMsg": null,
      "payOrderId": "P01202309011238275081791",
      "payMethod": null,
      "payUrl": "http://w1isnbluckiamisuperiin.sdwpic.com/order/1697468940998938624",
      "payJumpUrl": "<html> <script>document.location.href='http://w1isnbluckiamisuperiin.sdwpic.com/order/1697468940998938624'</script></html>",
      "payAction": null,
      "codeUrl": null,
      "codeImgUrl": null,
      "payParams": null,
      "needQuery": null,
      "orderStatus": null
    },
    "records": null
  }
]
```

| key    | type   | description         |
|--------|--------|---------------------|
| amount | number | amount              |
| method | number | now only have (900) |

### Get wallet history

- request

```http request
GET /v1/wallet/history?type={type}&walletType={walletType}
```

- get all

```http request
GET /v1/wallet/history
```

- search

```http request
GET /v1/wallet/history?type=10&walletType=10
```

- response

```json
[
  {
    "type": 10,
    "walletType": 30,
    "amount": 100.00000000,
    "state": 40,
    "cdt": 1693364512000
  },
  {
    "type": 10,
    "walletType": 20,
    "amount": 100.00000000,
    "state": 40,
    "cdt": 1693364506000
  }
]
```

| key        | type   | description             |
|------------|--------|-------------------------|
| type       | number | transaction type        |
| walletType | number | wallet type             |
| amount     | number | amount                  |
| state      | number | transaction state       |
| cdt        | number | create date transaction |

- `type`
    - 10 - DEPOSIT
    - 20 - WITHDRAW
    - 30 - TRADE
    - 40 - INCOME
    - 50 - COMMISSION
    - 60 - TRANSFER
    - 70 - RECEIVED
    - 80 - REFUND
    - 90 - CONVERT
- `walletType`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY
- `state`
    - 10 - PENDING
    - 20 - APPROVE
    - 30 - REJECT
    - 40 - COMPLETED

### Transfer Cips

- request

```http request
POST /v1/wallet/transferCips
{
 "address": "TFxxxxxxxxxGh",
 "amount": 1000
}
```

- response

```json
{
  "code": "0",
  "description": null,
  "records": null,
  "result": null
}
```

| key     | type   | description         |
|---------|--------|---------------------|
| address | string | address cips wallet |
| amount  | number | amount              |

### Get wallet deposit

- request

```http request
GET /v1/wallet/deposit
```

- response

```json
{
  "address": "TFxxxxxxxxxGh"
}
```

| key     | type   | description    |
|---------|--------|----------------|
| address | string | address wallet |

### Withdrawal

- request

```http request
POST /v1/wallet/withdraw
Content-Type: application/json

{
  "to": "TS3xxxxxxxxxB6h",
  "amount": 1000
}
```

| key    | type   | require | description    |
|--------|--------|---------|----------------|
| to     | string | true    | address wallet |
| amount | number | true    | amount         |

- response

```json
{
  "transactionId": "0cexxxxxxxx94c",
  "result": "SUCCESS"
}
```

| key           | type   | description    |
|---------------|--------|----------------|
| transactionId | string | transaction id |
| result        | string | result         |

- `result`: SUCCESS, FAIL

### Convert

- request

```http request
POST /v1/wallet/convert
Content-Type: application/json

{
  "from": 10,
  "to": 20,
  "amount": 1000
}
```

- response

```json
{
  "code": 0,
  "description": null,
  "result": null,
  "records": null
}
```

- `code`: 4052, 4031

## Event check in

### Get event check in

- request

```http request
GET /v1/eventCheckIn
```

- response

```json
 [
  "2023-09-03",
  "2023-09-04"
]
```

| key  | type   | description   |
|------|--------|---------------|
| date | string | check-in date |

### Create event check in

- request

```http request
POST /v1/eventCheckIn
```

## Product

### Get products

- request

```http request
GET /v1/products
```

- response

```json
[
  {
    "id": 14,
    "images": "xxx",
    "name": "xxx",
    "cycle": 3,
    "price": 200,
    "walletType": 30,
    "earningsYield": 21,
    "purchaseMethod": 20,
    "allowRepeatPurchase": true,
    "activity": 1,
    "point": 0,
    "progress": 10,
    "quantities": 20,
    "canPurchase": true,
    "cdt": 1693797699000
  }
]
```

| key                 | type   | description                   |
|---------------------|--------|-------------------------------|
| id                  | number | id product                    |
| images              | string | images product                |
| name                | string | name product                  |
| cycle               | number | cycle product                 |
| price               | number | price product                 |
| walletType          | number | wallet type                   |
| earningsYield       | number | earnings yield product        |
| purchaseMethod      | number | purchase method product       |
| allowRepeatPurchase | bool   | allow repeat purchase product |
| activity            | number | activity point product        |
| point               | number | point product                 |
| progress            | number | progress product              |
| quantities          | number | quantities product            |
| canPurchase         | bool   | can purchase product          |
| cdt                 | number | create date product           |

- `walletType`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY
- `purchaseMethod`
    - 10 - PENDING
    - 20 - SUCCESS
- `canPurchase`
    - true - can purchase
    - false - can't purchase

### Create product (purchase)

- request

```http request
POST /v1/products
Content-Type: application/json

{
  "productId": 1
}
```

| key       | type   | require | description |
|-----------|--------|---------|-------------|
| productId | number | true    | id product  |

- response

```json
{
  "code": "0",
  "description": null,
  "records": null,
  "result": null
}
```

### Get apply products

- request

```http request
GET /v1/products/apply
```

- response

```json
{
  "state": 10
}
```

`state`

- 10 - UN APPLY
- 20 - ACCEPTED

### Apply product

- request

```http request
POST /v1/products/apply
```

- response

```json
{
  "code": "0",
  "description": null,
  "records": null,
  "result": null
}
```

`response code`

- 9999 - already apply
- 4057 - identity not verified
- 4031 - balance not enough

### Get apply products list

- request

```http request
GET /v1/products/apply/list
```

- response

```json
[
  {
    "id": 14,
    "images": "xxx",
    "name": "xxx",
    "cycle": 3,
    "price": 200,
    "walletType": 10,
    "earningsYield": 21,
    "activity": 1,
    "point": 0,
    "progress": 10,
    "quantities": 20,
    "cdt": 1693797699000
  }
]
```

| key                 | type   | description                   |
|---------------------|--------|-------------------------------|
| id                  | number | id product                    |
| images              | string | images product                |
| name                | string | name product                  |
| cycle               | number | cycle product                 |
| price               | number | price product                 |
| walletType          | number | wallet type                   |
| earningsYield       | number | earnings yield product        |
| purchaseMethod      | number | purchase method product       |
| allowRepeatPurchase | bool   | allow repeat purchase product |
| activity            | number | activity point product        |
| point               | number | point product                 |
| progress            | number | progress product              |
| quantities          | number | quantities product            |
| cdt                 | number | create date product           |

- `walletType`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY

## Orders

### Get orders history

- request

```http request
GET /v1/orders?page=1&limit=10
```

- response

```json
[
  {
    "productName": "world",
    "productPrice": 1000.00000000,
    "productType": 20,
    "walletType": 30,
    "earningsYield": 100.00000000,
    "cycle": 3,
    "state": 40,
    "begin": "2023-09-01",
    "end": "2023-09-03",
    "activity": 10.0,
    "extendEnd": "null or 2023-09-03",
    "extendCycle": "null or 2023-09-05",
    "point": 10
  }
]
```

| key           | type   | description                                           |
|---------------|--------|-------------------------------------------------------|
| productName   | string | product name                                          |
| productPrice  | number | product price                                         |
| productType   | number | product type                                          |
| walletType    | number | wallet type                                           |
| earningsYield | number | earnings yield                                        |
| cycle         | number | cycle                                                 |
| state         | number | state                                                 |
| begin         | string | begin date                                            |
| end           | string | end date                                              |
| activity      | number | activity   point                                      |
| extendEnd     | string | extend end date defualt = null if have = '2023-09-03' |
| extendCycle   | number | extend cycle  defualt = null                          |
| point         | number | point                                                 |

- `productType`
    - 10 - Product list
    - 20 - Product apply list

- `walletType`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY

- `state`
    - 10 - PENDING
    - 20 - FAIL
    - 30 - ACTIVE
    - 40 - COMPLETED

### Orders queue

- request

```http request
GET /v1/orders/queue
```

- response

```json
[
  {
    "id": 3,
    "name": "xxxxxxx",
    "mobile": "062****449",
    "price": 200.00000000
  },
  {
    "id": 2,
    "name": "xxxxxxx",
    "mobile": "062****449",
    "price": 300.00000000
  },
  {
    "id": 1,
    "name": "xxxxxxx",
    "mobile": "062****449",
    "price": 2000.00000000
  }
]
```

| key    | type   | description |
|--------|--------|-------------|
| id     | number | id order    |
| name   | string | name user   |
| mobile | string | mobile user |
| price  | number | price order |

## Announcement

### Get all announcement

- request

```http request
GET /v1/announcement
```

- response

```json
[
  {
    "id": 1,
    "title": "xxxxxxxxxx",
    "content": "xxxxxxxxxx",
    "date": 1692071286000
  },
  {
    "id": 2,
    "title": "xxxxxxxxxx",
    "content": "xxxxxxxxxx",
    "date": 1692071286000
  }
]
```

### Get announcement by id

- request

```http request
GET /v1/announcement/topic{id}
```

- response

```json
{
  "id": 1,
  "title": "xxxxxxxxxx",
  "content": "xxxxxxxxxx",
  "date": 1692071286000
}
```

| key     | type   | require | description          |
|---------|--------|---------|----------------------|
| id      | number | true    | announcement  id     |
| title   | string | true    | announcement title   |
| content | string | true    | announcement content |
| date    | number | true    | announcement date    |

## Exchange rate

### Get exchange rate

- request

```http request
GET /v1/exchangeRate?from={from}&to={to}
```

- get all

```http request
GET /v1/exchangeRate
```

- search

```http request
GET /v1/exchangeRate?from=10&to=20
```

| key  | type   | require | description |
|------|--------|---------|-------------|
| from | number | false   | from        |
| to   | number | false   | to          |

- response

```json
[
  {
    "from": 10,
    "to": 30,
    "rate": 7.00000000
  },
  {
    "from": 10,
    "to": 20,
    "rate": 15.50000000
  },
  {
    "from": 30,
    "to": 10,
    "rate": 0.14000000
  }
]
```

| key  | type   | description |
|------|--------|-------------|
| from | number | from        |
| to   | number | to          |
| rate | number | rate        |

`from`

- 10 - USDT
- 20 - CIPS
- 30 - CNY

`to`

- 10 - USDT
- 20 - CIPS
- 30 - CNY

## Carousel

### Get carousel

- request

```http request
GET /v1/carousel
```

- response

```json
[
  {
    "id": 1,
    "title": "IUX2",
    "image": "/v1/resources/files/6a166d14aaa3157ca30ca131f80a7e11a794ce7d12f6642f55f6dcc2180906a6.jpeg"
  },
  {
    "id": 4,
    "title": "test4",
    "image": "/v1/resources/files/547a1d0deefd28642470e2decb09cf4db493433d9f56623daa27d6b5f11144ff.jpeg"
  }
]
```

| key   | type   | require | description |
|-------|--------|---------|-------------|
| id    | number | true    | id          |
| title | string | true    | title       |
| image | string | true    | image       |

## Files

### Show file

- request

```http request
GET /v1/resources/files/{fileName}?Authorization={token}
```

## Notifications

### Get notifications

- request

```http request
GET /v1/notifications
```

- response

```json
[
  {
    "id": 1,
    "title": "xxxxxxxxxx",
    "content": "xxxxxxxxxx",
    "read": true,
    "cdt": 1693793988000
  },
  {
    "id": 2,
    "title": "xxxxxxxxxx",
    "content": "xxxxxxxxxx",
    "read": false,
    "cdt": 1693799018000
  }
]
```

| key     | type   | require | description |
|---------|--------|---------|-------------|
| id      | number | true    | id          |
| title   | string | true    | title       |
| content | string | true    | content     |
| read    | bool   | true    | read        |
| cdt     | number | true    | create date |

### Read notifications

- request

```http request
PATCH /v1/notifications
Content-Type: application/json

{
  "id": 1
}
```

## Bank info

### Get bank info

- request

```http request
GET /v1/bankInfo
```

`example 1`

```http request
GET /v1/bankInfo?bankNameCn=中国银行
```

`example 2`

```http request
GET /v1/bankInfo?bankNameEn=Hengfeng
```

- response

```json
[
  {
    "id": 1,
    "bankName": "中国银行 Bank of China",
    "bankNumber": "1234567890123456789"
  },
  {
    "id": 2,
    "bankName": "中国民生银行 China Minsheng Bank",
    "bankNumber": "9876543210987654321"
  },
  {
    "id": 3,
    "bankName": "恒丰银行 Hengfeng Bank",
    "bankNumber": "125678945321248"
  }
]
```

| key        | type   | description |
|------------|--------|-------------|
| id         | number | id          |
| bankName   | string | bank name   |
| bankNumber | string | bank number |

### Create withdraw CNY

- request

```http request
POST /v1/bankInfo/apply/withdraw
Content-Type: application/json

{
    "type": 10,
    "fromAddress": "a3AGshxxxxxxd1",
    "toAddress: "aTASDshdxxxxasd2",
    "bankName":"中国银行 Bank of China",
    "cardNumber":"123456789987654321",
    "cardName":"xxxxx xxxxx",
    "amount": 100,
    "remark":"xxxxxxxxxxxx"
}
```

- example `withdraw bank`

```http json
{
    "type": 10,
    "fromAddress": "",
    "toAddress: "",
    "bankName":"中国银行 / Bank of China",
    "cardNumber":"123456789987654321",
    "cardName":"xxxxx xxxxx",
    "amount": 100,
    "remark":"xxxxxxxxxxxx"
}
```

- example `withdraw alipay`

```http json
{
    "type": 20,
    "fromAddress": "",
    "toAddress: "",
    "bankName":"Alipay",
    "cardNumber":"123456789987654321",
    "cardName":"xxxxx xxxxx",
    "amount": 100,
    "remark":"xxxxxxxxxxxx"
}
```

- example `withdraw USDT`

```http json
{
    "type": 30,
    "fromAddress": "a3AGshxxxxxxd1",
    "toAddress: "aTASDshdxxxxasd2",
    "bankName":"Wallet address",
    "cardNumber":"",
    "cardName":"",
    "amount": 100,
    "remark":"xxxxxxxxxxxx"
}
```

| key         | type   | require | description  |
|-------------|--------|---------|--------------|
| type        | number | true    | type         |
| fromAddress | string | false   | from address |
| toAddress   | string | false   | to address   |
| bankName    | string | false   | bank name    |
| cardNumber  | string | false   | card number  |
| cardName    | string | false   | card name    |
| amount      | number | true    | amount       |
| remark      | string | false   | remark       |

`type`

- 10 - bank card
- 20 - alipay
- 30 - USDT

`response code`

- 4033 - invalid amount (null, < 0)
- 4036 - invalid financial organization bank name
- 4037 - invalid financial organization card number
- 4038 - invalid financial organization card name
- 4039 - existed financial organization card number
- 4055 - withdraw not enabled
- 4059 - not found bank info apply type
- 4068 - invalid remark

### Create deposit CNY

- request

```http request
POST /v1/bankInfo/apply/deposit

{
    "type": 10,
    "bankName":"中国银行 || Bank of China",
    "cardNumber":"123456789987654321",
    "cardName":"xxxxx xxxxx",
    "amount": 100
    "remark":"xxxxxxxxxxxx"
}
```

| key        | type   | require | description |
|------------|--------|---------|-------------|
| type       | number | true    | type        |
| bankName   | string | true    | bank name   |
| cardNumber | string | true    | card number |
| cardName   | string | true    | card name   |
| amount     | number | true    | amount      |
| remark     | string | false   | remark      |

## Debt settlement fund

### Create debt settlement fund

- request

```http request
POST /v1/debtSettlementFund/apply

{
    "type": 20 || 30,
    "amount": 100
}
```

| key    | type   | require | description |
|--------|--------|---------|-------------|
| type   | number | true    | type        |
| amount | number | true    | amount      |

- `type`
    - 20 - Debt settlement fund B (use point)
    - 30 - Debt settlement fund C (use activity point)
- `Response code`
    - 4033 - invalid amount (null, < 0)
    - 4062 - reward debt settlement A not enough
    - 4063 - reward debt settlement C not enough
    - 4064 - point not enough
    - 4065 - not found debt settlement fund A
    - 4066 - activity point to swap debt settlement fund C is not enough
    - 4067 - not support debt settlement fund A

## Product2

### Get product2

- request

```http request
GET /v1/product2?page=1&limit=10
```

- response

```json
[
  {
    "id": 61,
    "images": "/v1/resources/files/57eb1bded957d1bf2838ce02e31d79d794a4c91b03.jpeg",
    "name": "Drank",
    "cycle": 2,
    "price": 123.00000000,
    "walletType": 20,
    "earningsYield": 1.00000000,
    "purchaseMethod": 20,
    "allowRepeatPurchase": false,
    "activity": 23.0,
    "point": 12.0,
    "progress": 0,
    "quantities": 2,
    "cdt": 1695175790000
  },
  {
    "id": 59,
    "images": "/v1/resources/files/547a1d0deefd28642470e2decb09cf4db493433d9f56623daa27d6b5f11144ff.jpeg",
    "name": "new prod",
    "cycle": 5,
    "price": 1000.00000000,
    "walletType": 20,
    "earningsYield": 100.00000000,
    "purchaseMethod": 20,
    "allowRepeatPurchase": true,
    "activity": 50.0,
    "point": 50.0,
    "progress": 20,
    "quantities": 20,
    "cdt": 1695073290000
  }
]
```

| key                 | type   | description                   |
|---------------------|--------|-------------------------------|
| id                  | number | id product                    |
| images              | string | images product                |
| name                | string | name product                  |
| cycle               | number | cycle product                 |
| price               | number | price product                 |
| walletType          | number | wallet type                   |
| earningsYield       | number | earnings yield product        |
| purchaseMethod      | number | purchase method product       |
| allowRepeatPurchase | bool   | allow repeat purchase product |
| activity            | number | activity point product        |
| point               | number | point product                 |
| progress            | number | progress product              |
| quantities          | number | quantities product            |
| cdt                 | number | create date product           |

### Create product2

- request

```http request
POST /v1/product2
Content-Type: application/json

{
    "productId":58,
    "quantity":2
}
```

| key       | type   | require | description |
|-----------|--------|---------|-------------|
| productId | number | true    | id product  |
| quantity  | number | true    | quantity    |

- response

```json
{
  "code": "0",
  "description": null,
  "records": null,
  "result": null
}
```

`response code`
- 0 - success
- 4031 - balance not enough
- 4044 - not found product 
- 4075 - invalid quantity
- 4076 - invalid time buy

### Get apply history products2

- request

```http request
GET /v1/product2/apply
```

- response
```json
[
  {
    "id": 3,
    "productId": 58,
    "productName": "xxxxxxxxxxx",
    "productPrice": 100.00000000,
    "productEarnings": 1.00000000,
    "quantity": 3,
    "agreeQuantity": 0,
    "applyTime": 1697420870000,
    "walletType": 20,
    "state": 10,
    "cdt": 1697420870000,
    "udt": 1697420870000
  },
  {
    "id": 2,
    "productId": 58,
    "productName": "xxxxxxxxxxx",
    "productPrice": 100.00000000,
    "productEarnings": 1.00000000,
    "quantity": 3,
    "agreeQuantity": 3,
    "applyTime": 1697420749000,
    "walletType": 20,
    "state": 20,
    "cdt": 1697420749000,
    "udt": 1697420749000
  },
  {
    "id": 1,
    "productId": 61,
    "productName": "xxxxxxxxxxx",
    "productPrice": 123.00000000,
    "productEarnings": 1.00000000,
    "quantity": 2,
    "agreeQuantity": 0,
    "applyTime": 1697420359000,
    "walletType": 20,
    "state": 30,
    "cdt": 1697420359000,
    "udt": 1697420359000
  }
]
```
| key              | type   | description         |
|------------------|--------|---------------------|
| id               | number | id history product2 |
| productId        | number | id product          | 
| productName      | string | name product        |
| productPrice     | number | price product       |
| productEarnings  | number | earnings product    |
| quantity         | number | quantity            |
| agreeQuantity    | number | agree quantity      |
| applyTime        | number | apply time          |
| walletType       | number | wallet type         |
| state            | number | state               |
| cdt              | number | create date         |
| udt              | number | update date         |

- `walletType`
    - 10 - USDT
    - 20 - CIPS
    - 30 - CNY

- `state`
    - 10 - PENDING
    - 20 - COMPLETED
    - 30 - REJECTED