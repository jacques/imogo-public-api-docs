# Bill Payments

Bill Payments allow for a customer to make a payment to a Bill Issuer using their IMOGO Pay Now Code.

## Validate IMOGO Pay Now Code

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/billpayments/12345678901/verify"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"store_id":"12345678901","till_id":"1"}'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":{
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec",
    "customer": {
      "first_name": "Timothy",
      "last_name": "Colman"
    },
    "account_number":"12345678901",
    "type": "fixed",
    "min_amount": 50000,
    "max_amount": 50000
  }
}
```

> An example of an error response where a non-JSON formatted request arrives.

```
{
  "status": "error",
  "message": "Please ensure you are posting RAW json over."
}
```

> An example of an error response where the JSON formatted request is malformed.

```
Please ensure you are posting valid JSON over.
```

> An example of an error response where the Pay Now Code has expired.

```
{
  "status": "error",
  "message": "Pay Now Code has expired"
}
```

> An example of an error response where the merchant retreival number has already been processed.

```
{
  "status": "error",
  "message": "Merchant Retrieval Number has already been processed."
}
```

> An example of an error response where the merchant / retailer has insufficient funds.

```
{
  "status": "error",
  "message": "Merchant has insufficient funds."
}
```

This endpoint validates a IMOGO Pay Now Code against the IMOGO Platform.  This enables
the Retailer to see if the amount is fixed or variable and what the minimum and maximum
amount the Bill Issuer expects to be paid in.  It also lets the retailer know the name
of the Bill Issuers customer for printing on their receipt.

For once off IMOGO Pay Now codes, these expire after their first use.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/billpayments/<CODE>/verify`

### URL Parameters

Parameter | Description
--------- | -----------
CODE | IMOGO Pay Now Code

### JSON Payload Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
store_id | integer | true | Store ID
till_id | integer | true | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
status | enum | ok or error
details.uuid | string (36) | UUID of IMOGO Pay Now Order
details.customer.first_name | string(64) | First Name(s) of the Customer
details.customer.last_name | string(64) | Surname of the Customer
details.account_number | integer | IMOGO Pay Now Code
details.type | enum | fixed or variable for expected payment type
details.min_amount | integer | minimum amount to be allowed to tender to pay a bill
details.max_amount | integer | maximum amount to be allowed to tender to pay a bill
details.photo.data | string | base64 encoded photo of the user
details.photo.mime_type | string | mime type of the photo

## Verify customers ID Number

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/billpayments/12345678901/confirmid"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"id_type":"zaasylum","id_document_number":"ZAFCPT00123456","store_id":"12345678901","till_id":"1"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec",
    "customer": {
      "first_name": "Timothy",
      "last_name": "Colman"
    },
    "account_number":"12345678901",
    "amount": 50000,
    "photo": {
      "data": "base64 encoded string",
      "mime_type": "image/png"
    }
  }
}
```

As a retailer you need to obtain the customers id document number, passport
number or asylum document number.  If the id number does not match on the
IMOGO side, the agent must not accept the cash from the client to
authorise the transaction via the point of sales device.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/billpayments/<CODE>/verifyid`

### URL Parameters

Parameter | Description
--------- | -----------
CODE | IMOGO Pay Now Code

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
id_type | enum | zaid / passport / zaasylum
id_document_type | string | identification number for specified string
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
status | enum | ok or error
details.uuid | string (36) | UUID of IMOGO Pay Now Order
details.customer.first_name | string(64) | First Name(s) of the Customer
details.customer.last_name | string(64) | Surname of the Customer
details.account_number | integer | IMOGO Pay Now Code
details.amount | integer | Amount in cents paid against the bill payment

## Authorise payment against an IMOGO Pay Now Code

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/billpayments/12345678901/authorise"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"amount":"500","store_id":"12345678901","till_id":"1"}'
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec",
    "customer": {
      "first_name": "Timothy",
      "last_name": "Colman"
    },
    "account_number":"12345678901",
    "amount": 50000
  }
}
```

As a retailer you need to ensure that your stores have funds available before you
authorise a bill payment as been collected as part of your business rules applied
to transactions from stores.

Ensure that the store has sufficient funds per your risk process.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/billpayments/<CODE>/authorise`

### URL Parameters

Parameter | Description
--------- | -----------
CODE | IMOGO Pay Now Code

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
amount | integer | Amount in cents of the value being paid
mrn | string(12) | Merchants Retrieval Reference Number (RRN)
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
status | enum | ok or error
details.uuid | string (36) | UUID of IMOGO Pay Now Order
details.customer.first_name | string(64) | First Name(s) of the Customer
details.customer.last_name | string(64) | Surname of the Customer
details.account_number | integer | IMOGO Pay Now Code
details.amount | integer | Amount in cents paid against the bill payment
