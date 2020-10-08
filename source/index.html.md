---
title: WebWallet API

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the WebWallet API! 

>BASE URL

```http
https://api.baypay.com
```

# Authentication

WebWallet uses HashKey to allow access to the API

# 測試流程

## 註冊帳號

註冊帳號

> REQUEST BODY

```json
{
  "code": "a0bf8683a16b0a0a9f90d49c25745f95",
  "appId": 9,
  "phone": "+886987654321",
  "password": "p@ssw0rd",
  "time": 1601260773
}
```

> RESPONSE BODY

```json
{
  "returnCode": 4,
  "message": "電話尚未認證",
  "result": {
    "code": "80febc7300b8d6e784b64e6a972288bb"
  }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/WebWallet/register`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash 產生 (appId + phone + password + time + hashKey)
appId | number | appId (測試使用9)
phone | string | 有加國碼的手機號碼, 例:+886987654321
password | string | 密碼
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | 如果是4代表用電話註冊成功
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash 產生 (appId + returnCode + hashKey)

## 啟動使用者

透過上一個步驟

系統已經將驗證碼經由簡訊傳到所填入的手機號碼

接下來就是用驗證碼來啟動使用者

> REQUEST BODY

```json
{
  "code": "ba83f3a72d6f1b25790dc2334cdfb7e9",
  "appId": 9,
  "phone": "+886987654321",
  "email": "",
  "confirmCode": "666262",
  "time": 1601260773
}
```

> RESPONSE BODY

```json
{
  "returnCode": 1,
  "message": "",
  "result": {
    "code": "56c7c9ffd08158f55034a6b2db184721"
  }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/WebWallet/activateUser`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appId + phone + email + confirmCode + time + hashKey)
appId | number | appId (測試使用9)
phone | string | 有加國碼的手機號碼, 例:+886987654321
email | string | 電子郵件, 目前只有使用手機, 可帶空值
confirmCode | string | 輸入簡訊收到的驗證碼
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | 如果是1代表已經成功啟動使用者
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appId + returnCode + hashKey)

## 登入

Get UUID and sessionToken.

> REQUEST BODY

```json
{
  "code": "a0bf8683a16b0a0a9f90d49c25745f95",
  "appId": 9,
  "phone": "+886987654321",
  "password": "p@ssw0rd",
  "pushToken": "",
  "os": "",
  "appVersion": "",
  "appCode": "",
  "time": 1601260773
}
```

> RESPONSE BODY

```json
{
  "returnCode": 1,
  "message": "",
  "result": {
    "code": "56c7c9ffd08158f55034a6b2db184721",
    "uuid":"UVJYMGM108",
    "isShop":false,
    "time":1601266463,
    "sessionToken":"d1f2f499-0a9b-4323-89b0-15abe32d3b24"
  }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/WebWallet/login`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appid + phone + password + pushToken + os + appVersion + appCode + time + hashKey)
appId | number | appId (測試使用9)
phone | string | 有加國碼的手機號碼, 例:+886987654321
password | string | 密碼
pushToken | string | 推播用  目前可先空值
os | string | android or ios 可空值
appVersion | string | appVersion 可空值
appCode | string | appCode 可空值
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | returnCode
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appid+ retuenCode + uuid + isShop  + sessionToken +  time + hashKey)
uuid | string | unique user id
isShop | bool | 使用者是否是商家
time | number | [unix timestamp](https://www.unixtimestamp.com/)
seesionToken | string | 每次登入都會產生新的seesionToken

## 查看總資產

查看總資產.

coins 下可看到各個代幣的 數量, rate 及法幣價值

> REQUEST BODY

```json
{
  "code": "f6d56c1d5a3bc81048aa936575f2c0a4",
  "appId": 9,
  "uuid": "UVJYMGM108",
  "fiatType": "TWD",
  "net": "TESTNET",
  "sessionToken": "d1f2f499-0a9b-4323-89b0-15abe32d3b24",
  "time": 1601260773
}
```

> RESPONSE BODY

```json
{
  "returnCode": 1,
  "message": "",
  "result": {
   "code": "814fbdb466bc2d3d77f76f163decb596",
   "appId": 9,
   "uuid": "UVJYMGM108",
   "totalValue": 118.9000000,
   "fiatType": "TWD",
	"coins": [
	{
	 "coinType": "BPTW",
	 "coinName": "BPTW",
	 "amount": 109.00,
	 "fiatType": "TWD",
	 "walletID": "375",
	 "rate": 1.0,
	 "value": 109.0000000
	},
	{
	 "coinType": "BPUSD",
	 "coinName": "BPUSD",
	 "amount": 0.33,
	 "fiatType": "TWD",
	 "walletID": "376",
	 "rate": 30.0,
	 "value": 9.9000000
	},
	{
	 "coinType": "BPHK",
	 "coinName": "BPHK",
	 "amount": 0.00,
	 "fiatType": "TWD",
	 "walletID": "377",
	 "rate": 3.85,
	 "value": 0.0000000
	}
	],
	"fiats": [],
	"time": 1601266526
   }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/WebWallet/balance`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appId + uuid + fiatType + net + sessionToken + time + hashKey)
appId | number | appId (測試使用9)
uuid | string | 登入後取得的uuid
fiatType | string | 法定貨幣 TWD
net | string | MAIN or TESTNET
sessionToken | string | 登入後取得的sessionToken
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | returnCode
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appid + returnCode+ uuid + totalValue + fiatType + [coinType + coinName + amount + fiatType + walletID + rate + value] + [fiatType + fiatName + value + walletID] + time + hashKey)
appId | number | appId (測試使用9)
uuid | string | unique user id
totalValue | number | 是所有代幣的法幣價值總和
fiatType | string | 法定貨幣類型
coins | list | 擁有的各種代幣
fiats | list | 擁有的各種法定貨幣
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**coins**


Field | Type | Description
--------- | ------- | -----------
coinType | string | 代幣類型
coinName | string | 代幣名稱
amount | number | 代幣數量
fiatType | string | 法定貨幣
walletID | string | 錢包ID
rate | number | 匯率
value | number | 代幣的法定貨幣價值


**fiats**


Field | Type | Description
--------- | ------- | -----------
fiatType | string | 法定貨幣
fiatName | string | 法定貨幣名稱
value | number | 法定貨幣價值
walletID | string | 錢包ID

## SendCoin

給別人代幣.

> REQUEST BODY

```json
{
  "code": "499ae95bd43a7c01a83f3e470621fe3e",
  "appId": 9,
  "fromUUID": "UVJYMGM108",
  "toUUID": "USPVD87",
  "coinType": "BPTW",
  "coinAmount": "10",
  "net": "TESTNET",
  "sessionToken": "d1f2f499-0a9b-4323-89b0-15abe32d3b24",
  "time": 1601260773
}
```

> RESPONSE BODY

```json
{
  "returnCode": 1,
  "message": "",
  "result": {
    "code": "56c7c9ffd08158f55034a6b2db184721"
  }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/WebWallet/send`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appId + fromUUID + toUUID + coinType + coinAmount + net + sessionToken + time + hashKey)
appId | number | appId (測試使用9)
fromUUID | string | 要給代幣的uuid
toUUID | string | 要收代幣的uuid
coinType | string | 代幣類型
coinAmount | number | 要付多少代幣數量
net | string | MAIN or TESTNET
sessionToken | string | 要給代幣的人登入後取得的sessionToken
time | number | [unix timestamp](https://www.unixtimestamp.com/)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | returnCode
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appid + returnCode + hashKey)

# MD5 Hash

MD5 Hash.

> REQUEST BODY

```json
{
  "string": "original string",
  "appId": 9
}
```

> RESPONSE BODY

```json
{
  "returnCode": 1,
  "message": "",
  "result": {
    "code": "hash code"
  }
}
```


**Request Headers**


HeadersName | Required | Value
--------- | ------- | -----------
Content-Type | Yes | application/json


**HTTP Request**


`POST /api/App/getMD5HashCode`


**Request Body**


Field | Type | Description
--------- | ------- | -----------
string | string | 所需的資料組合後的原始字串
appId | number | appId (測試使用9)


**Response Body**


Field | Type | Description
--------- | ------- | -----------
returnCode | string | returnCode
message | string | message
result | object | response data


**result**


Field | Type | Description
--------- | ------- | -----------
code | string | MD5Hash產生 (appid + returnCode + hashKey)
