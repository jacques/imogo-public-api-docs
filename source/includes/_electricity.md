# Prepaid Electrcity

## Vend Prepaid Electricity

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/electricity/recharge"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"amount":"10000","meter_number":"08012345679","store_id":"12345678901","till_id":"1"}'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":{
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec"
    "token": "1234 1234 1234 1234 1234"
  }
}
```

This endpoint requests a token from the utility the meter gelongs to.

Please note not all prepaid electricity meters are supported.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/bongotel/recharge`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
amount | integer | The amount in cents to load to the users BongoTel Dialer
meter_number | integer | Meter Number
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)
