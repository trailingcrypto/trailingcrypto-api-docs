**Table of Contents**
- [Check Server Status](#check-server-status)
- [Authentication](#authentication)
- [User Endpoints](#user-profile-endpoints)
	- [Get Profile Info](#get-profile-info-(including-added-exchange-list-and-other-info))
	- [Configure Api Keys](#configure-exchange-api-keys)
	- [Fetch Balance](#fetch-balance)
- [Trading Endpoints](#trading-endpoints)
	- [Create a new trade or order](#create-a-new-trade-or-order)
	- [Get Trade/Order history](#get-trade/order-history)
	- [Cancel or Delete an order](#cancel-or-delete-an-order)
	- [Fetch Trade info/Order Details](#fetch-trade-info/order-details)
- [Order Matrix Explanation](#order-matrix-explanation)
	- [Order Execution Sequence](#order-execution-sequence)
	- [Order Parameters Explanation](#order-parameters-explanation)
<br></br>

### Check Server Status

**Request:**

    GET /api/ping
    
**Response:**
```javascript  
{
   err:  false,
   msg:  "pong",
   time:  1569750557563
}
```

</br></br>
## Authentication
All API endpoints are Authenticated using `Bearer token Authorization`.
 [Click here](https://swagger.io/docs/specification/authentication/bearer-authentication/) for more info about this Authorization mechanism

An existing user can obtain their apikey by opening this endpoint in their browser once logged-in https://www/trailingcrypto.com/api/user/profile

Each Protected Endpoint should have these header values in API calls:

|KEY  | VALUE |DESCRIPTION |
| ------------- |:-------------:| -----:|
|Authorization|Bearer `07cbefr5...`| Api Key to authenticate the requests|
| x-requested-with | XMLHttpRequest | This is to always receive response in JSON|
|Content-Type|application/x-www-form-urlencoded|In Post requests where you need to submit form data|

</br></br>

## User Profile Endpoints

### Get profile info (Including added exchange list and other info)

    GET /api/user/profile

 
 **Parameters: [None]**
 

**Response:**
```Javascript
{
    "id": "5f1f1497416bd41ad45607de",
    "_id": "5f1f1497416bd41ad45607de",
    "name": "SALMAN KHAN L",
    "email": "salmankhn411@gmail.com",
    "provider": "email",
    "type": 1,
    "referred": 0,
    "apikey": "e2bb2b3d-08e0-4e8a-855e-028f2cc7efee",
    "telegram": {
        "verified": true,
        "enabled": false,
        "token": "b35da051-fba1-49ce-ac2e-6764bc567292",
        "userId": "1220313908"
    },
    "exchange_list": [
        "binance"
    ]
}
```
**Possible Values:**

provider: `email`, `google`, `facebook`

type: `TRIAL:1, FREE:2, PAID:3, TELEGRAM_ADMIN:4, BUSINESS:5`

referred: Count of users signed-up using current user referral link
<br/><br/>

### Configure Exchange API Keys

    POST /api/user/saveapikey
 
 **Parameters:**
 
|Key |Value  | Description|
|--|--|--|
|exchange  | binance |Exchange for which api key needs to be configured|
|key|binance api key|User needs to generate a new API key for each app or tool 
|secret| binance api secret|This is stored encrypted on trailingcrypto server and once configured won't be visible to user again for security reasons.
|uid|users uid| Only for exchanges which requires uid like CEX.IO
|password|api key passphrase| Only for exchange which requires passphrase like Kucoin.com|

**Response:**
```Javascript
{
    "status": true
}
```
<br/><br/>

### Fetch Balance

    GET /api/trade/balances?exchange=binance

 
 **Parameters:**
 
|Type|Key |Value  | Description|
|--|--|--|--|
|Query|exchange  | binance |Configured exchange |

<!-- TODO: Add success response here -->
**Response:**
```Javascript
{
	"statusCode":401,
	"msg":"API-key format invalid."
}
```
<br><br/>


## Trading Endpoints
<br/>

**Types of orders**
|Type | Description|
|--|--|
|LIMIT_BUY  | Add info here 
|LIMIT_SELL | Add info here
|MARKET_BUY | Add info here
|MARKET_SELL | Add info here
|TRAILING_STOP_BUY | Add info here
|TRAILING_STOP_SELL | Add info here
|STOP_BUY | Add info here
|STOP_LOSS | Add info here
|TAKE_PROFIT | Add info here
|OSO - CUSTOM | Add info here
|OCO BUY | Add info here
|OCO SELL | Add info here
|OCO - CUSTOM| Add info here|


### Create a new trade or order

    POST /api/trade/order

 
 **Body Parameters:**
 
|Key |Value  | Description|
|--|--|--|
|exchange  | binance |Exchange on which order needs to be placed|
|type|MARKET_SELL|Type of Trades specified in above
|orders| Order Matrix in JSON String| Order matrix contains all info regarding order or a group of orders in series or parrell. *(Examples given below)*

**Response:**
```Javascript
{  
"success":true,  
"msg":{  
	"_id":"5d908dc78045eb20c0ec46ab",  
	"updatedAt":"2019-09-29T10:56:07.543Z",  
	"createdAt":"2019-09-29T10:56:07.476Z",  
	"coin":"BNB/BTC",  
	"exchange":"binance",  
	"status":"CLOSE",  
	"stop_price":null,  
	"relative_stop_price":null,  
	"max_price":0.0019041,  
	"min_price":0.0019041,  
	"stop_delay":0,  
	"bot_multiplier":null,  
	"order_volume":10,  
	"auto_volume":false,  
	"base_quantity":0.01905605,  
	"public_id":"dae507bc-7d3a-4506-bae1-7c61348cdf98",  
	"entry_price":0.0019037,  
	"user":"5ca3612fbee94d24588d6624",  
	"type":"MARKET_SELL",  
	"comment":"Order Placed",    
	"close_price":0.0019041,  
	"live":false,  
	"repeat":0,  
	"retry_count":0,  
	"bot_attempt":0  
	}  
}
```
<br/><br/>

### Get Trade/Order history

**Get open orders:**

    GET /api/trade/orders/open?exchange=binance&coin=ETH/BTC&filters=[]&_=1598293629659

**Get order history:**

	GET /api/trade/orders/history?exchange=binance&coin=ETH/BTC&filters=[DEMO]&_=1598292590875
 
 **Query Parameters:**
 
|Key |Value  | Description|
|--|--|--|
|exchange  | binance |Exchange on which order needs to be placed|
|coin|ETH/BTC|Coin to Trade (Quote)/ Market (Base)
|filters| ["DEMO","LIVE"]| Array of filters based on which data should be fetched, Details of Various filters are mentioned below
|_|1598292590878| Current timestamp in milliseconds
<br></br>

**Filters:**
|Filter Type|Description|
|--|--|
|DEMO|Demo orders placed|
|LIVE|Orders which are currently live|
|EXCHANGE|Remote orders on exchanges like Kucoin.com|
|LOCAL|Local orders placed on Trailing Crypto Server|
|NOT_EXPIRED|Orders which are not yet expired|
|EXPIRED|Expired orders|

**Response:**
```Javascript
{
    "err": null,
    "data": [
        {
            "location": "local",
            "id": "5f4401c1b558c15ebc3f3226",
            "exchange": "binance",
            "public_id": "0abd3e1f",
            "group_id": "0abd3e1f",
            "coin": "BTC/ETH",
            "type": "MARKET BUY",
            "status": "CLOSE",
            "volume": "1 ETH",
            "quote_volume": 1,
            "price": "0.03436 -> 0.03436 (Last)",
            "current_price": 0.034245,
            "condition": "",
            "stop_price": null,
            "relative_stop_price": null,
            "closedAt": "2020-08-24T18:06:57.520Z",
            "openedAt": "2020-08-24T18:06:57.520Z",
            "expiry": "2020-11-22T18:06:57.496Z",
            "entry": 0.03436,
            "price_type": "Last",
            "action": "resume",
            "comment": "Demo -  Order Placed",
            "notes": ""
        }
    ]
}
```
<br/><br/>

### Cancel or Delete an order

    Delete /api/trade/order?exchange=binance&coin=ETH/BTC

 
 **Parameters:**
 
|Type|Key |Value  | Description|
|--|--|--|--|
|Query|exchange  | binance |Exchange on which order needs to be placed|
|Query|coin  | ETH/BTC |Coin to Trade (Quote)/ Market (Base)|
|Body|type |TRAILING STOP SELL|Type of Trades 
|Body|id | [Order ID]| Order matrix contains all info regarding order or a group of orders in series or parrell. *(Examples given below)*
|Body|location | local \| exchange| **local** (On Trailing Crypto Server) \|  **exchange** (Remote Order on Exchange)

**Response:**
```Javascript
{
    "success":true,
    "id":"5f3e97c9852ee620f8c2edfa"
}
```
<br/><br/>

### Fetch trade info/Order Details

    GET /api/trade/updateOrder?id=[order_id]
 **Parameters:**
 
|Type|Key |Value  |Description|
|--|--|--|--|
|Query|id |[order_id] |order_id string value |

 **Response:**
```Javascript
{
    "success": true,
    "order": {
        "entry_price": [
            "Entry Price",
            0.034217
        ],
        "trailing_loss": [
            "Trailing Offset",
            1
        ],
        "upper_stop": [
            "Upper Stop",
            null,
            true
        ],
        "lower_stop": [
            "Lower Stop",
            null,
            true
        ],
        "stop_delay": [
            "Stop Delay",
            0,
            false
        ],
        "expiry": [
            "Expiry (Days)",
            89,
            false
        ]
    }
}
```
<br/><br/>

# Order Matrix Explanation

## Order Execution Sequence
Lets assume a,b,c,d,e,f are different trades/orders that we wish to execute in series or parallel sequence.

    [[a, b], [c, d], [e, f]]
Above sequence means `a` & `b` on same level parallel or OCO order, similarly `c` & `d` is another OCO order and `e` & `f` another OCO order. and all of them are in series. When this order will be placed then either `a` or `b` will be executed then either c or d will be executed then either `e` or `f` will be executed. Lets clarify more with more examples.

    [[a,b]]
This is the simplest example of OCO order here `a` could be stop loss and `b` could be take profit running in parallel and when either of these two hits stop price then another one will be cancelled.

    [[a], [b]]
In above example `a` & `b` are in series (OSO order). `b` will wait for `a` to complete. if `a` is market buy and `b` is market sell then market sell will be executed once market buy finishes.
 
## Order Parameters Explanation

Param | Example | Details|
---|---|---|
order_type | STOP_BUY | Possible Values are STOP_BUY... |
order_sub_type | TRAILING | Exit strategy for advance orders. Possible values are LIMIT, MARKET, TRAILING, INSTANT |---
exchange | BINANCE | BINANCE, BITTREX, BINANCEFUTURES, BINANCEMARGIN, BITMEX, KUCOIN, etc. |
coin | RDN/BTC | Coin to Trade (Quote)/ Market (Base) | 
volume | 100 | Coin volume (Quote) |
auto_volume | true | Default is False. This is only applicable in secondary OSO order.|
entry_price | 0.00002176 | No effect on trade. Only used for profit calculation. If omitted current price will be used.
stop_price | 0.00002176 | Positive decimal value depicting stop price of conditional trades.
relative_stop_price | -5 | Negative number denotes -X% example use case is `STOP_LOSS`. Positive Number denoted X% example use case is `TAKE_PROFIT`. `STOP_BUY` supports both the direction in single order so specify negative number for stop price less then current price and positive number for price greater than current price. <br>When `relative_stop_price` is defined then `stop_price` field is ignored. |
stop_condition | lte | Only required in case of `STOP_BUY` order. Possible values `lte`, `gte`. <br>`lte` means Less than or Equal to current price and `gte` means greater than or equal to current price. |
stop_delay | 60 | Stop Timeout (In Seconds). Number of Seconds to wait for price to stay above or below stop price to execute the trade.|
expiry | 60 | Expiry duration (In Minutes). Once this duration is reached order will be automatically filled or cancelled as defined by user
expiry_action | FILL | Default action is CANCEL. Possible actions are FILL, CANCEL. 
live | true | true means live order, false means demo order.|
break_even_stop | true | only application in case of OCO order |
price_type | LAST | Price to follow or Trail for conditional orders. Possible values are LAST, BID, ASK |
targets | [<br>{"stop_price":"+5",<br>"relative_stop_price":"+5",<br>"quantity":"70"},<br>{"stop_price":"+10",<br>"relative_stop_price":"+10",<br>"quantity":"30"}<br>] | Array of objects containing definition of each targets in case of multiple targets order. When target is defined stop_price field will be ignored. 
 
 
<br><br>

**price_type:** </br>

--- 
- **ASK  :** Highest price that a trader is willing to pay to go long, There's no guarantee when a bid order is placed that the trader placing the bid will receive the number of coins that they want.
---
- **BID  :** The ask price is the lowest price someone is willing to sell a stock for  
---
- **LAST :** The ask price is the lowest price someone is willing to sell a stock for

<br></br>
 **Quantity/Volume Units:**
 
 To Understand Quantity/Volume units for trading kindly check out our [Article](https://www.trailingcrypto.com/support/article/understanding-trade-volume-relative-volume-base-quote-volume)   

