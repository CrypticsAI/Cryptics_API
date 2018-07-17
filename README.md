# Public Cryptics Rest API
# General API Information
* The base endpoint is: https://devapi.cryptics.tech
* All endpoints return either a JSON object or an array.
* Data is returned in **ascending** order: recent first, latest at the end.
* All of the time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for malformed requests;
  the issue is on the sender's side.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors
* HTTP `504` return code is used when the API successfully sent the message
 but did not get a response within the timeout period.
It is important to **NOT** treat this as a failure; the execution status is
**UNKNOWN** and could have been a success.
* Any endpoint can retun an ERROR.
* For all questions regarding api, please, contact: support@cryptics.tech


# Public API


## API endpoints


Endpoints | Description
------------ | ------------
/coin_list | Returns a list of valid forecasted pairs.
/hourly_fcast | Returns current Hourly forecast.
/hourly_history | Returns all hourly forecasts.
/four_hour_fcast | Returns all 4-hour ahead forecasts.

* First forecast timestamp may vary for different pairs, since we are adding new pairs to forecasts.

## Examples

```
GET /coin_list
```
Returns a list of valid pairs.

**Parameters:**
NONE

**Response:**
```javascript
{"BTC/USD", "ARDR/USD", "ETH/USD"}
```

### Hourly Forecast
```
GET /hourly_fcast
```
Current hourly forecast

**Parameters:**

Param | Example | Description
------------ | ------------ | ------------
pair | BTC/USD | Pair form a valid pairs (returned by **/coin_list**)

**Returned Values:**

Value | Example | Description
------------ | ------------ | ------------
fcast | 0.2565 | Forecasted price price
pair | ARDR/USD | Forecasted Pair
move | down | Forecasted Move
price | 0.2572 | price Price at Forecast Generation Time
timestamp | 2018-04-09 13:00 | Forecast Generation Time

**Response:**
```javascript
[{
"fcast":0.2565, // Forecasted price price
"pair":"ARDR/USD", // Forecasted Pair
"move":"down", // Forecasted Move
"price":0.2572, // price Price at Forecast Generation Time
"timestamp":"2018-04-09 13:00" // Forecasts Generation Time
  },
{
"fcast":423.2196, // Forecasted price price
"pair":"ETH/USD", // Forecasted Pair
"move":"up", // Forecasted Move
"price":423.15, // price Price at Forecast Generation Time
"timestamp":"2018-04-09 13:00" // Forecasts Generation Time
  }]

```
### Hourly historical forecasts

```
GET /hourly_history

```
**Response:**
```javascript
[{
"fcast":0.5452,
"pair":"ARDR/USD",
"move":"down",
"price":0.5492,
"timestamp":"2018-02-13 10:00:00"
  },
{
"fcast":840.2655,
"pair":"ETH/USD",
"move":"down",
"price":844.44,
"timestamp":"2018-02-13 10:00:00"
  },
"fcast":1.0094,
"pair":"XRP/USD",
"move":"down",
"price":1.02,
"timestamp":"2018-02-13 10:00:00"
  }]

```




### Daily Forecasts 

Daily Forecasts. Currently returns current daily forecast for all forecasted pairs.

**Parameters:**

Param | Example | Description
------------ | ------------ | ------------
pair | ETH/USD | Pair form a valid pairs 


```
GET /daily_fcast

```
**Response:**
```javascript
[{
"timestamp":"2018-07-17",
"pair":"ETH/USD",
"fcast":"473.635214647127",
"price":478.75
},
{
"timestamp":"2018-07-17",
"pair":"XRP/USD",
"fcast":"0.472484155946305",
"price":0.4824
}]

```





### Daily Forecasts Accuracy

Daily Forecasts Accuracy. Currently returns accuracy for all forecasted pairs.


```
GET /daily_stats

```
**Response:**
```javascript
[{
"asset":"ETH/USD",
"accuracy":"67.5862068965517"
},
{
"asset":"XRP/USD",
"accuracy":"69.9300699300699"
}]

```




### 4-Hour Ahead Forecast
```
GET /four_hour_fcast
```
Historical Data for 4-hour ahead forecasts.

* Note: New forecast generated each hour, but the forecasting horizon is 4-hours long.

**Parameters:**

Param | Example | Description
------------ | ------------ | ------------
pair | ETH/BTC | Currently supports only ETC/BTC and LTC/BTC as pairs.

**Returned Values:**

Value | Example | Description
------------ | ------------ | ------------
fcast | 0.2565 | Relative strength of the forecasted move
pair | ETH/BTC | Forecasted Pair
move | up | Forecasted Move
price | 0.2572 | price Price at Forecast Generation Time
timestamp | 1522767600 | Forecast Generation Time in epoch/Unix timestamp

**Response:**
```javascript
[{
"timestamp":"2018-04-03 21:00:00",
"price":"0.05573",
"fcast":"0.00825",
"move":"up",
"pair":"ETH/BTC"
  },
{
"timestamp":"2018-04-03 22:00:00",
"price":"0.05585",
"fcast":"0.00542",
"move":"up",
"pair":"ETH/BTC"
  }]

```
