# TronEnergize Public Rest API
## TronEnergize Public Rest API Overview:

TronEnergize Public Rest API provides developers with a powerful toolset to interact programmatically with the TronEnergize energy marketplace. This API allows users to access essential market data, manage energy orders, and perform trading operations.

## Key Features:

- Market Data: Retrieve minimal prices and available energy to make informed decisions.
- Order Management: View a list of active orders, create orders, and check the status of existing ones.
- Trading Operations: Create unsigned RAW transactions for BUY orders, create and pay orders from internal wallets.

## Authentication:

Secure your transactions with API Key and Secret for authentication.

## HTTP Return Codes:

Easily interpret responses with clear HTTP return codes and error messages.

## Ease of Use:

JSON responses make integration seamless, allowing developers to build customized interfaces and automate buying processes.

Explore the TronEnergize API to enhance your experience in the Tron energy market!

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
505 | Inactivated address | 

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
### Minimum prices
```
GET /api/v1/minprices
```


**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1695219834,
  "data": {
      "1200":35,
      "7200":35
    },
  "bandwidth": {
      "1200":600,
      "7200":600
    },
}
```
### Available Energy
```
GET /api/v1/available
```

**Parameters:**
Name | value | Mandatory | Description
------------ | ------------ | ------------ | ------------
filter | 'yes' | NO |  Return of filtered available energy

**Response:**
```javascript
{
  "time": 1695219834,
  "data": {
       "energy":50000000,
       "bandwidth": 60000000
    }
}
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
### Create unsigned RAW transaction for BUY ENERGY Order.
```
GET /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
energyamount | integer | YES |  Energy amount in SUN, minimum 32000
price | integer | YES |  Minimum 45
duration | STRING | YES |  5m, 1d, 3d, 7d, 14d, 30d, 1h and 6h are supported

**Response:**
```javascript
{
  "time": 1596477571, 
  "data":{
           "tx": {"visible": false, "txID": "6debe75130870086575f90792f.. ..f8aa9b031"},
           "amount": 1.28
         }
}
```
### Create unsigned RAW transaction for BUY BANDWIDTH Order.
```
GET /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
bandwidthamount | integer | YES |  Bandwidth amount, minimum 2000
price | integer | YES |  Minimum 600
duration | STRING | YES |  5m, 1d, 3d, 7d, 14d, 30d, 1h and 6h are supported

**Response:**
```javascript
{
  "time": 1596477571, 
  "data":{
           "tx": {"visible": false, "txID": "6debe75130870086575f90792f.. ..f8aa9b031"},
           "amount": 1.28
         }
}
```

### Create BUY ENERGY Order.
```
POST /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
receiver | STRING | YES |  Resource receiving address
energyamount | integer | YES |  Energy amount in SUN, minimum 32000
price | integer | YES |  Minimum 45
duration | STRING | YES |  5m, 1d, 3d, 7d, 14d, 30d, 1h and 6h are supported
txid | STRING | YES |  Signed and Broadcasted Transaction Hash
webhook | STRING | NO |  Once the order has been filled, we will send event to your webhook url

**Webhook Example :**
When the order has been filled, TronEnergize will send a GET HTTP request with order data. If your server is set up to listen for webhook deliveries at that URL, it can take action when it receives one.

* **webhook:**
  ```
  https://github.com/webhook
  ```
* **GET HTTP request:**
  ```
  GET https://github.com/webhook?id=187&payout=777.5&stake=77000&energy=1000000&price=45&rawduration=201600&receiver=TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru
  ```

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": {
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
}
```
### Create BUY BANDWIDTH Order.
```
POST /api/v1/neworder
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
buyer | STRING | YES |  Resource purchase address
receiver | STRING | YES |  Resource receiving address
bandwidthamount | integer | YES |  Bandwidth amount, minimum 2000
price | integer | YES |  Minimum 600
duration | STRING | YES |  5m, 1d, 3d, 7d, 14d, 30d, 1h and 6h are supported
txid | STRING | YES |  Signed and Broadcasted Transaction Hash
webhook | STRING | NO |  Once the order has been filled, we will send event to your webhook url

**Webhook Example :**
When the order has been filled, TronEnergize will send a GET HTTP request with order data. If your server is set up to listen for webhook deliveries at that URL, it can take action when it receives one.

* **webhook:**
  ```
  https://github.com/webhook
  ```
* **GET HTTP request:**
  ```
  GET https://github.com/webhook?id=187&payout=777.5&stake=77000&energy=1000000&price=45&rawduration=201600&receiver=TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru
  ```

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": {
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "bandwidth": 1000000, 
        "price": "600", 
        "apy": "77%", 
        "duration": "7 days",
        "rawduration": 201600,
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru"
      }
}
```

###  Status of order.
```
GET /api/v1/status/[orderId]
```
This endpoint returns the status ('active', 'filled', 'canceled') of order.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
orderid | STRING | YES |  Order Id.

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [{
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "energy": 1000000, 
        "price": "45 SUN/Day", 
        "apy": "77%", 
        "duration": "7 days",
        "rawduration": 201600,
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru",
        "status": "active",
        "delegations": [
                         {
                           "txid": "7089e44e52e...513eba2bdfa78",
                           "energy": 1000000,
                         },
                         ...
                       ]
    }]
}
```

### Sell energy.
```
POST /api/v1/delegate
```

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
orderid | STRING | YES |  Order Id
sender | STRING | YES |  The address will receive payout 
txid | STRING | YES |  Signed and Broadcasted Transaction Hash

**How to get order_id ?**
```
GET /api/v1/market
```

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": {
        "amount": 777.5, 
        "txid": "8b317f47d79547a2...24cc6317d84d9"
      }
}
```




# Authentication
Authenticating to the TronEnergize API requires a valid API Key.
Your API Key identifies your account (think of it as a username) and the API Secret authenticates your account (think of it as a password). Please follow the instructions below to secure your API Key and API Secret:

* Do not send your API Secret with your API requests. Only send the API Key.
* Do not share your API Secret or the API Key with anyone.
* Do not commit your API Secret into source control systems like github.
* If you lose your API Key or Secret, immediately contact Tronenergize support.

Please take note that if your API Secret is compromised, your funds are at risk.

# Endpoint security type
* Each next endpoint has a security type that determines how you will
  interact with it. This is stated next to the NAME of the endpoint.
* If no security type is stated, assume the security type is NONE.
* API-keys are passed into the Rest API via the `X-TR-APIKEY` header.
* API-keys and secret-keys **are case sensitive**.

# SIGNED Endpoint security
* `SIGNED` endpoints require an additional parameter, `signature`, to be sent in the  `query string` or `request body` or `HEADERS`.
* The `signature` is **not case sensitive**.

## Timing security
* A `SIGNED` endpoint also requires a parameter, `timestamp`, to be sent which
  should be the second timestamp of when the request was created and sent.
  
## Request signing
Authenticated calls require you to send a request signature with every request. This signature should be re-generated for every request.

### Example API Key/API Secret: 
Key | Value
------------ | ------------
`apiKey` | API_my6mqen60xpwnewo2efckxtz
`secretKey` | S2808_61xkvn_bprxy5_s3szia

## SIGNED Endpoint Examples for POST /api/v1/order

Here is a step-by-step example of how to send a valid signed payload from the
Linux command line using `echo`, `openssl`, and `curl`.


Parameter | Value
------------ | ------------
`receiver` | TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t
`energyamount` | 32000
`price` | 35
`duration` | 1d
`timestamp` | 1695219834

#### Example 1: As a request body
* **requestBody:** receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834
* **HMAC SHA256 signature:**

    ```
    [linux]$ echo -n "receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834" | openssl dgst -sha256 -hmac "S2808_61xkvn_bprxy5_s3szia"
    (stdin)= 4e79a51a5bb2df5d3b894095d27d994c02f9f7705bc95eb4e4296b08b0af35fb
    ```


* **curl command:**

    ```
    (HMAC SHA256)
    [linux]$ curl -H "X-TR-APIKEY: API_my6mqen60xpwnewo2efckxtz" -X POST 'https://nile.tronenergize.com/api/v1/order' -d 'receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834&signature=4e79a51a5bb2df5d3b894095d27d994c02f9f7705bc95eb4e4296b08b0af35fb'
    ```

#### Example 2: As a query string
* **queryString:** receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834
* **HMAC SHA256 signature:**

    ```
    [linux]$ echo -n "receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834" | openssl dgst -sha256 -hmac "S2808_61xkvn_bprxy5_s3szia"
    (stdin)= 4e79a51a5bb2df5d3b894095d27d994c02f9f7705bc95eb4e4296b08b0af35fb
    ```

* **curl command:**

    ```
    (HMAC SHA256)
    [linux]$ curl -H "X-TR-APIKEY: API_my6mqen60xpwnewo2efckxtz" -X GET 'https://nile.tronenergize.com/api/v1/order?receiver=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&energyamount=32000&price=35&duration=1d&timestamp=1695219834&signature=4e79a51a5bb2df5d3b894095d27d994c02f9f7705bc95eb4e4296b08b0af35fb'
    ```


### Create Order and pay from internal wallet.
```
POST /api/v1/order
```

**HEADERS:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
X-TR-APIKEY | STRING | YES |  Your API Key


**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
receiver | STRING | YES |  Resource receiving address
energyamount | integer | YES |  Energy amount in SUN, minimum 32000
price | integer | YES |  Minimum 35
duration | STRING | YES |  1d, 3d, 7d, 14d, 30d, 1h and 6h are supported
timestamp | integer | YES |  Current timestamp in seconds
signature | STRING | YES |  HMAC SHA256 signature

**Response:**
```javascript
{
  "time": 1596477571, 
  "data":{
        "id": 787, 
        "payout": 777.5, 
        "stake": 77000, 
        "energy": 1000000, 
        "price": "45 SUN/Day", 
        "duration": "7 days",
        "txid": "8b317f47d79547a2...24cc6317d84d9",
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru"
      }
}
```
### Remove the Order.
```
POST /api/v1/removeorder
```

**HEADERS:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
X-TR-APIKEY | STRING | YES |  Your API Key


**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
receiver | STRING | YES |  Money receiving address
orderid | integer | YES |  Order Id
timestamp | integer | YES |  Current timestamp in seconds
signature | STRING | YES |  HMAC SHA256 signature

**Response:**
```javascript
{
  "time": 1596477571, 
  "data":{
        "id": 787, 
        "txid": "8b317f47d79547a2...24cc6317d84d9",
        "receiver": "TLwpQv9N6uXZQeE4jUudLPjcRffbXXAuru"
      }
}
```

