# Prepaid Airtime

## Vend Airtime

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/airtime/recharge"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"amount":"10000","network_id":"1","mobile_number":"08012345679","store_id":"12345678901","till_id":"1"}'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":{
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec"
  }
}
```

This endpoint recharges the users mobile on their Mobile Network Operator (MNO) with the value in cents specified.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/airtime/recharge`

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
amount | integer | The amount in cents to load to the users BongoTel Dialer
network_id | integer | Network Id
mobile_number | integer | MSISDN for the user in E.164 format (minus the leading +)
store_id | integer | Store ID
till_id | integer | Till Id (i.e. Lane 1 == Till 1)
