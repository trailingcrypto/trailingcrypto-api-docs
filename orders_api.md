# orders-api-docs

## Table of Contents
- [GET User API key](#get-user-api-key)
- [Wallet API](#wallet-api)
- [GET Placed Orders](#get-placed-orders)
- [Placed Trigger Order Details](#placed-trigger-order-details)
- [Placing Order](#placing-order)
- [Order Matrix Explanation](#order-matrix-explanation)
	- [Order Execution Sequence](#order-execution-sequence)
	- [Order Parameters Explanation](#order-parameters-explanation)
- [Cancel Order](#cancel-order)


## Important Note
Always include the following key-value pair in the header to receive the response in JSON format:

```
x-requested-with: XMLHttpRequest
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer UserXXXXapiXXXKey     //check below section to get user_api_key for user
```


## Get User API key 
Ignore this section if you already have **user apikey**, Follow below steps to get API key
1. Login to www.trailingcrypto.com with desktop or laptop
2. Now open this link in new tab: https://www.trailingcrypto.com/api/user/profile
3. copy **apikey** from this JSON response (attached image)

![image](https://github.com/trailingcrypto/trailingcrypto-api-docs/assets/45059136/82d46fcd-de0a-4dec-a608-ee42501ad661)



## Wallet API
### GET `/api/trade/balances?exchange=binance_testlabel`
Returns the wallet balances for a specific exchange.

#### Response
```json
{
  "BTC": {
    "free": 0.00000408,
    "used": 0,
    "total": 0.00000408,
    "free_usd_val": 0.11,
    "used_usd_val": 0,
    "total_usd_val": 0.11,
    "name": "Bitcoin",
    "logo": "https://s2.coinmarketcap.com/static/img/coins/128x128/1.png"
  },
  {...},
  {...}
}
```

#### Error
```json
{
  "statusCode": 401,
  "msg": "API-key format invalid."
}
```

## GET Placed Orders
### Open Orders
#### GET `/api/trade/orders/open?exchange=binance_keytesttest&coin=AAVE/USDT`
Returns the open orders for a specific coin pair on an exchange.

### History
#### GET `/api/trade/orders/history?exchange=binance_keytesttest&coin=DOGE/USDT`
Returns the order history for a specific coin pair on an exchange. Passing a coin pair will add coin pair orders placed on the exchange in the API response.

#### Response
```json
{
  "err": null,
  "data": [
    {
      "location": "local",
      "id": "6451fe5ef6d8b45b963aec01",
      "exchange": "binance",
      "public_id": "a4061ccd",
      "group_id": "a4061ccd",
      "coin": "USDT/BTC",
      "type": "Custom - TELEGRAM - STOP BUY - MARKET",
      "status": "CLOSE",
      "volume": "100 USDT",
      "price": "28515.67",
      "current_price": 26699.01,
      "close_price": 0,
      "condition": "Stop: lte - 25555",
      "stop_price": 25555,
      "relative_stop_price": null,
      "stop_condition": "lte",
      "targets": "",
      "closedAt": "2023-05-03T06:28:03.258Z",
      "openedAt": "2023-05-03T06:25:34.291Z",
      "expiry": "2023-05-18T06:25:34.367Z",
      "entry": 28515.67,
      "price_type": "Last",
      "avg_price": 0,
      "break_even_price": 0,
      "action": "resume",
      "comment": "Demo -  Manually Cancelled | last state: OPEN",
      "notes": "",
      "group_pnl": "",
      "min_price": 28515.67,
      "max_price": 28515.67,
      "signalOrder": "6451fe3af6d8b45b963aebfb",
      "live": false,
      "signalInfo": {
        "channelIds": ["binary_beast"],
        "channelNames": "binary_beast"
      },
      "leverage": "",
      "leverage_type":

 "",
      "matrix_position": "0,0",
      "sort_date": "2023-05-03T06:25:34.344Z",
      "volume_usd": 100.024,
      "pnl_usd": null
    },
    {...},
    {...}
  ]
}
```

## Placed Trigger Order Details
### GET `/api/signaltrade/order`
Returns the details of placed trigger orders.

#### Response
```json
{
  "success": true,
  "count": 4,
  "data": [
    {
      "id": "6451fe3af6d8b45b963aebfb",
      "source": "telegram",
      "channel": "binary_beast",
      "auto_target": false,
      "status": "OPEN",
      "cancel_secondary": false,
      "auto_buy_target": true,
      "auto_sell_target": true,
      "auto_stop_loss": true,
      "take_profit_weight": [],
      "condition": "",
      "date": "2023-05-03T06:24:58.857Z",
      "orderId": "e2569ec8",
      "channelIds": [["binary_beast"]],
      "repeat": 99999998,
      "expiry": 0,
      "coin": "USDT",
      "minVolume": null,
      "maxPositionsMarket": null,
      "maxPositionsProvider": null,
      "riskPerTrade": null,
      "signal_algorithm": "",
      "tv_rsi": {
        "value": null,
        "condition": ""
      },
      "tv_rating": {
        "value": "",
        "timeframe": ""
      }
    },
    {...},
    {...}
  ]
}
```

## Placing Order
### POST `/api/trade/order`
Places an order.

#### Params: Market buy order
```json
{
  "type": "MARKET_BUY",
  "exchange": "binance_binance",
  "repeat": 1,
  "orders": [
    [
      {
        "order_type": "MARKET_BUY",
        "exchange": "binance_binance",
        "coin": "ACA/USDT",
        "relative_volume": "100",
        "relativeVolumeFormatted": "100%USDT",
        "auto_volume": false,
        "entry_price": "0.04460000",
        "stop_delay": 0,
        "expiry_duration": 0,
        "expiry_datetime": false,
        "expiry_action": "CANCEL",
        "live": false,
        "break_even_stop": false,
        "price_type": "LAST"
      }
    ]
  ]
}
```

#### Params: Stop Buy + Take Profit Order
```json
{
  "type": "OSO - CUSTOM",
  "exchange": "binance_binance",
  "repeat": 1,
  "orders": [
    [
      {
        "order_type": "STOP_BUY",
        "order_sub_type": "MARKET",
        "exchange": "binance_binance",
        "coin": "BTC/USDT",
        "auto_volume": false,
        "base_quantity": "1000",
        "entry_price": "0.04470000",
        "stop_price": "-10",
        "relative_stop_price": "-10",
        "stop_delay": 0,
        "expiry_duration": 0,
        "expiry_datetime": false,
        "expiry_action": "CANCEL",
        "stop_condition": "lte",
        "live": false,
        "break_even_stop": false,
        "price_type": "LAST"
      }
    ],
    [
      {
        "order_type": "TAKE_PROFIT",
        "order_sub_type": "MARKET",
        "exchange": "binance_binance",
        "coin": "BTC/USDT",
        "volume": "1000",
        "auto_volume": true,
        "entry_price": "0.04470000",
        "stop_delay": 0,
        "expiry_duration": 0,
        "expiry_datetime": false,
        "expiry_action": "CANCEL",
        "live": false,
        "break_even_stop": false,
        "price_type": "LAST",
        "targets": [
          {
            "stop_price": "+10",
            "relative_stop_price": "+10",
            "quantity": "40"
          },
          {
            "stop_price": "+20",
            "relative_stop_price": "+20",
            "quantity": "60"
          }
        ]
      }
    ]
  ]
}
```

#### Response: Success
```json
{
  "success": true,
  "msg": {
    "public_id": "291928fa-c63d-4d9b-9abe-29c33fa40780"
  }
}
```

#### Response: Error
```json
{
  "success": false,
  "msg": "Invalid Json Orders Object"
}
```

## Order Matrix Explanation

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
 
<br>

**price_type:** </br>
--- 
- **ASK  :** Highest price that a trader is willing to pay to go long, There's no guarantee when a bid order is placed that the trader placing the bid will receive the number of coins that they want.
---
- **BID  :** The ask price is the lowest price someone is willing to sell a stock for  
---
- **LAST :** The ask price is the lowest price someone is willing to sell a stock for
---
 **Quantity/Volume Units:** 
 To Understand Quantity/Volume units for trading kindly check out our [Article](https://www.trailingcrypto.com/support/article/understanding-trade-volume-relative-volume-base-quote-volume)   
  <br></br>

## Cancel Order
### DELETE `/api/trade/order?exchange=binance_binance`
Cancels an order.

#### Params
- `id`: 6472f87ca439xxxx4xxxf59 (order_id in string form)

#### Response
```json
{
  "success": true,
  "status": true,
  "msg": "Order/s cancelled successfully"
}
```
