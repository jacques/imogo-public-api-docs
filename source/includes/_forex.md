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

`GET https://127.0.0.1.xip.io/api/v1/forex/recipients`

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

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the recipient
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
middle_name | string (64) | Middle name of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)

## Create Forex Recipient

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/forex"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{""}'
```

This endpoint retrieves a collection of recipients for sending forex to.  These users have been
added as recipients for the user.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/forex/recipients`

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":[
    {
      "uuid":"72d11c48-a565-4731-8915-462084ee0a9f",
      "first_name":"Timothy",
      "middle_name":"Michael",
      "last_name":"Colman"
    }
  ]
}
```

## Request Quote (before sending money)

```shell
curl -X GET "https://127.0.0.1.xip.io/api/v1/users/c3797604-6e78-486e-be5d-433f80cc4993/forex"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

This endpoint is used to request a quotation to send forex to the specified recipient.

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

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/mobile/users/<USER>/forex/deals/quote`

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------

## Accept Quote (to send money)

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"mobile_number":"0801234567","quote":"
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

This endpoint accepts the quote and creates the forex deal and moves money for
execution of the forex deal.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/mobile/users/<USER>/forex/deals/accept`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
mobile_number | integer | Mobile Number of the Sender
quote | string(36) | UUID of the quotation
