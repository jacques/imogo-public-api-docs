# BongoTel Forex

 * Destination countries for Forex:
    - Bangladesh (BD)
    - Botswana (BW)
    - Malawi (MW)
    - Mozambique (MZ)
    - Zimbabwe (ZW)

## List Forex Recipients

```shell
curl -X GET "https://127.0.0.1.xip.io/api/v1/mobile/users/c3797604-6e78-486e-be5d-433f80cc4993/forex"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

This endpoint retrieves a collection of recipients for sending forex to.  These users have been
added as recipients for the user.

### HTTP Request

`GET https://127.0.0.1.xip.io/users/<USER>/forex/recipients`

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":[
    {
      "uuid":"72d11c48-a565-4731-8915-462084ee0a9f",
      "first_name":"TIMOTHY",
      "middle_name":"MICHAEL",
      "last_name":"COLMAN"
    }
  ]
}
```

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the recipient
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
middle_name | string (64) | Middle name of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)

## Create Forex Recipient

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/forex/<MSISDN>/recipients"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"first_name":"TIMOTHY","middle_name":"MICHAEL","last_name":"COLMAN","mobile_number":"27802345678","country":"BDT","account_number":"123456","swift_code":"DBBLBDDH102"}'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":[
    {
      "uuid":"72d11c48-a565-4731-8915-462084ee0a9f",
    }
  ]
}
```

This endpoint retrieves a collection of recipients for sending forex to.  These users have been
added as recipients for the user.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/forex/<MSISDN>/recipients`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the recipient
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
middle_name | string (64) | Middle name of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)

## Request Quote (before sending money)

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/forex/deals/quote"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"mobile_number":"27801234567","recipient_uuid":"72d11c48-a565-4731-8915-462084ee0a9f","amount":"500000"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "quote": "e70c34d5-1a89-452f-b8b3-12be4c022ef0",
    "send": {
      "currency": "ZAR",
      "amount": "5000"
    },
    "receive": {
       "currency": "BDT",
       "amount": "2906396"
    }
  }
}
```

This endpoint is used to request a quotation to send forex to the specified recipient.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/mobile/users/<USER>/forex/deals/quote`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
mobile_number | integer | MSISDN for the user in E.164 format (minus the leading +) sending the forex
recipient_uuid | string(36) | UUID of the recipient you to be sent forex
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
quote | string (36) | UUID of the quote
send.currency | string (3) | Currency code for currency sending from
send.amount | integer | Amount in cents to quote on
receive.currency | string (3) | Currency code for currency being sent to
receive.amount | integer | Amount in cents to receive if quote accepted

## Accept Quote (first confirmation)

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/forex/deals/accept"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"mobile_number":"0801234567","quote":"c3d820ba-ac36-437d-b916-c1dc8833ad80","otp":"123456"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid": "e70c34d5-1a89-452f-b8b3-12be4c022ef0"
  }
}
```

This endpoint accepts the quote.  Requires confirmation.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/forex/deals/accept`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
mobile_number | integer | Mobile Number of the Sender
otp | string(6) | OTP that was sent to the customer to accept quote (before confirmation)
quote | string(36) | UUID of the quotation
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the accepted deal

## Confirm Accept Quote (to actually send money)

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/forex/deals/accept"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"mobile_number":"0801234567","quote":"c3d820ba-ac36-437d-b916-c1dc8833ad80"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid": "e70c34d5-1a89-452f-b8b3-12be4c022ef0"
  }
}
```

This endpoint confirms the acceptance of the quote and creates the forex deal and moves money for
execution of the forex deal.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/forex/deals/confirm`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
mobile_number | integer | Mobile Number of the Sender
quote | string(36) | UUID of the quotation
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the accepted deal
