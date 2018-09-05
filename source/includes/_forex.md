# BongoTel Forex

Support for delivery of forex worldwide via IMOGO.

Tim can you confirm which countries we are unable to send money to?

## List Forex Recipients

```shell
curl -X GET "https://127.0.0.1.xip.io/api/v1/users/c3797604-6e78-486e-be5d-433f80cc4993/forex"
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
curl -X POST "https://127.0.0.1.xip.io/api/v1/users/<USERS>/forex/recipients"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"first_name":"TIMOTHY","last_name":"COLMAN","mobile_number":"27802345678","country":"BD","account_number":"123456","swift_code":"DBBLBDDH102"}'
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

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/forex/recipients`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)
destination_type | enum | bank / cashpoint / wallet
account_number | integer | Account number of the recipient
swift_code | string(16) | Swift Code for recipients bank
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the recipient
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)

### Destination Types

Value | Description
----- | ----------
bank  | Cash is sent to the recipients bank account
cashpoint | Cash is collected from the specified cash point
wallet | Money is transfered into the specified account at the specified wallet provider.

## Request Quote

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users/<USER>/forex/deals/quote"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"mobile_number":"27801234567","recipient_uuid":"72d11c48-a565-4731-8915-462084ee0a9f","amount":"500000"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "quote": "e70c34d5-1a89-452f-b8b3-12be4c022ef0"
  }
}
```

This endpoint is used to request a quotation to send forex to the specified recipient.  The sender receives a SMS message with a Pay@ reference and the amount in the destination currency for the recipient.  The quote is valid for 6 hours.  The amount deposited via Pay@ needs to be the exact amount.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/forex/deals/quote`

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
quote | string (36) | UUID of the request for quotation