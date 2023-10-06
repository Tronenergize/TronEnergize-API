# TronEnergize Public Rest API


## General API Information
* The base endpoint is: **https://tronenergize.com/**
* The nile TESTNET endpoint is: **https://nile.tronenergize.com/**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in **seconds**.

## HTTP Return Codes

* HTTP `4XX` return codes are used for malformed requests;
  the issue is on the sender's side.
* HTTP `403` return code is used when the WAF Limit (Web Application Firewall) has been violated.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; 
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.


## Error Codes
* Any endpoint can return an ERROR

Sample Payload below:
```javascript
{
  "code": 404,
  "msg": "Invalid Account."
}
```

Code | Cause | Solution
--- | --- | ---
200 | Success | 
400 | Request body parameter format error | Usually appears on POST request, please check if the json structure is correct
403 | Request frequency is limited | The access frequency per second is limited to 5qps. Please reduce the request frequency or add a new key
404 | Missing required parameters | Please carefully check whether the parameter types and values of the document and the actual request are reasonable and valid
444 | Data does not exist | 
500 | Operation failed. Please try again later | 
502 | Insufficient balance | 

## General Information on Endpoints
* For `GET` endpoints, parameters must be sent as a `query string`.


# Public API Endpoints

## General endpoints
### Test connectivity
```
GET /api/v1/ping
```
Test connectivity to the Rest API.

**Parameters:**
NONE

**Response:**
```javascript
{"time": 1695219834}
```

## Market Data endpoints
###  Order List. 
```
GET /api/v1/market
```
This endpoint allows anyone to get a list of all active orders

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
      {
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "energy": 1000000, 
        "price": "45 SUN/Day", 
        "apy": "77%", 
        "duration": "7 days",
        "rawduration": 201600,
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru"
      }
      ...
    ]
```

## Trading endpoints
### Create unsigned RAW transaction for BUY Order.
```
GET /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
energyamount | integer | YES |  Energy amount in SUN
price | integer | YES |  Minimum 35
duration | STRING | YES |  1d, 3d, 7d, 14d, 1h and 6h are supported

**Response:**
```javascript
{
  "time": 1596477571, 
  "data":{"visible": false, "txID": "6debe75130870086575f90792f.. ..f8aa9b031"},
  "amount": 1.28}
}
```

### Create Order.
```
POST /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
receiver | STRING | YES |  Resource receiving address
amount | integer | YES |  Energy amount in SUN
price | integer | YES |  Minimum 35
duration | STRING | YES |  1d, 3d, 7d, 14d, 1h and 6h are supported
txid | STRING | YES |  Signed and Broadcasted Transaction Hash

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
      {
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "energy": 1000000, 
        "price": "45 SUN/Day", 
        "apy": "77%", 
        "duration": "7 days",
        "rawduration": 201600,
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru"
      }
      ...
    ]
```

###  Status of order.
```
GET /api/v1/status/[orderId]
```
This endpoint returns the status ('active', 'filled', 'canceled') of order.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
orderId | STRING | YES |  Order Id.

**Response:**
```javascript
{
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "energy": 1000000, 
        "price": "45 SUN/Day", 
        "apy": "77%", 
        "duration": "7 days",
        "rawduration": 201600,
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru",
        "status": "active"
}
```








