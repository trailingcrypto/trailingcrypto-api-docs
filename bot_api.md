# GRID BOT & DCA BOT API Documentation

---

## Important Note
Always include the following key-value pair in the header to receive the response in JSON format:

```
x-requested-with: XMLHttpRequest
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer UserXXXXapiXXXKey     //check below section to get user_api_key for normal user
```

## Table of Contents
- [GET User API key](#get-user-api-key)
- [Leaderboard table API](#leaderboard-table-api)
- [Trade Pair API](#trade-pair-api)
- [Fetch Balance API](#fetch-balance-api)
- [Gridbot Recommended Strategy List API](#gridbot-recommended-strategy-list-api)
- [Gridbot AI Settings API](#gridbot-ai-settings-api)
- [Gridbot Calculate Info API](#gridbot-calculate-info-api)
- [Gridbot Create API](#gridbot-create-api)
- [Gridbot Active API & Gridbot History API](#gridbot-active-api--gridbot-history-api)
- [Gridbot Stop API](#gridbot-stop-api)
- [Gridbot View Details API](#gridbot-view-details-api)
- [DCA Bot Recommended strategies](#dca-bot-recommended-strategies)
- [DCA Bot Create API](#dca-bot-create-api)
- [DCA Bot Active & History API](#dca-bot-active--history-api)
- [DCA Bot Stop API](#dca-bot-stop-api)
- [DCA Bot Details API](#dca-bot-details-api)

---

## GET User API key

Follow the steps mentioned in this link: [Get User API Key](https://github.com/trailingcrypto/trailingcrypto-api-docs/blob/master/orders_api.md#get-user-api-key)

---

## Leaderboard table API

**Endpoint:** `GET /api/bots/leaderboard`

**Query Parameters:**
- `sort`: Sorting criteria for the leaderboard (e.g., "apr").
- `bot_type`: Type of bot (e.g., "grid").
- `market`: Trading market (e.g., "BTC/USDT").
- `duration`: Duration range in milliseconds (e.g., 86400000-1728000000).
- `apr`: APR range (e.g., 100-500).

**Note:** If no filters are selected, no filter parameters need to be passed. The duration range should be specified in milliseconds.

**Example Usage:**
```http
GET /api/bots/leaderboard?sort=apr&bot_type=grid&market=BTC/USDT&duration=86400000-1728000000&apr=100-500
```

**Response:**
```json
{
	"success": true,
	"data": [
		{
			"user": "vi*ha*sa*u2*10",
			"market": "BTC/USDT",
			"type": "futures-grid",
			"sub_type": "short",
			"position_type": "short",
			"running_time": "18 hours",
			"apr": 37632.864,
			"max_drawdown": "TODO",
			"meta_data": {
				"lower_price": 20000,
				"grid_count": 10,
				"direction": 0,
				"step_price": 2000,
				"grid_profit": {
					"low": 5.26,
					"high": 10,
					"low_actual": 5.063157,
					"high_actual": 9.8
				},
				"grid_mode": "arithmetic"
			}
		},
		{...},
		{...}
	]
}
```

---

## Trade Pair API

**Endpoint:** `GET /api/trade/ticker`

**Query Parameters:**
- `exchange`: Exchange name ("binance").

**Response:**
```json
{
    "ETH/BTC": {
        "bid": 0.068191,
        "ask": 0.068192,
        "last": 0.068191,
        "volume24h": 13167.7377624,
        "timestamp": 1678815301886
    }
}
```

---

## Fetch Balance API

**Endpoint:** `GET /api/trade/balances`

**Query Parameters:**
- `exchange`: Exchange name (e.g., "binance").

**Response:**
```json
{
    "BTC": {
        "free": 0.00000478,
        "used": 0,
        "total": 0.00000478,
        "free_usd_val": 0.12,
        "used_usd_val": 0,
        "total_usd_val": 0.12
    }
}
```

**Note:** Use the balance of the base coin. For example, if the trading pair is BTC/USDT or XRP/USDT, use the balance of USDT.

---

## Gridbot Recommended Strategy List API

**Endpoint:** `GET /api/bots/grid/recommended`

**Query Parameters:**
- `sort`: Sorting criteria for the strategy list (e.g., "apr", "risk", "rating", "24h_change", "volatility", "24h_vol").
- `exchange`: Exchange name(binance etc.).
- Default sort is "24h_vol".

**Response:** 
```json
{
    "success": true,
    "data": [
        {
            "market": "BTC/USDT",
            "apr": -17.66216124285714,
            "risk": "moderate",
            "rating": "buy",
            "24h_change": -2.05226063,
            "volatility": 2.92137807,
            "24h_vol": 1207972552.8492582
        },
        {...},
        {...}
    ]
}
```

---

## Gridbot AI Settings API

**Endpoint:** `GET /api/bots/grid/ai-settings/:mode`

**Example Usage:**
```http
GET /api/bots/grid/ai-settings/moderate?pair=ETH/USDT&exchange=binance
```

**Note:** `mode` can be one of "safe," "moderate," or "aggressive."

**Response:**
```json
{
    "success": true,
    "data": {
        "exchange": "binance",
        "exchange_subaccount": "binance",
        "coin": "ETH/USDT",
        "lower_price": 1740,
        "upper_price": 2019,
        "grid_count": 27,
        "amount_per_trade": null,
        "entry_price": 1871.79,
        "step_price": 10.333333333333334,
        "grid_profit": {
            "low": 0.5144374377696649,
            "high": 0.5938697318007664,
            "low_actual": 0.3144374377696649,
            "high_actual": 0.3938697318007664
        }
    }
}
```

---

## Gridbot Calculate Info API

**Endpoint:** `GET /api/bots/grid/calculate-info`

**Query Parameters:**
- `exchange`: Exchange name (e.g., "binance").
- `coin`: Coin trading pair (e.g., "BTC/USDT").
- `lower_price`: Lower price limit.
- `upper_price`: Upper price limit.
- `grid_count`: Number of grid counts.
- `leverage`: Leverage value.
- `amount`: Investment amount.
- `position_type`: Position type (e.g., "neutral").
- `grid_mode`: Grid mode (e.g., "arithmetic").

**Note:** Omit any unavailable data in the API call.

**Response:**
```json
{
    "success": true,
    "params": {
        "exchange": "binance",
        "exchange_subaccount": "binance",
        "coin": "ETH/USDT",
        "amount_per_trade": null,
        "entry_price": 1870.24,
        "step_price": null,
        "grid_profit": {
            "low": null,
            "high": null,
            "low_actual": null,
            "high_actual": null
        }
    }
}
```

---

## Gridbot Create API

**Endpoint:** `POST /api/bots/grid/create`

**Parameters:**
- `exchange`: Exchange name (e.g., "binance").
- `coin`: Coin trading pair (e.g., "BTC/USDT").
- `lower_price`: Lower price limit.
- `upper_price`: Upper price limit.
- `grid_count`: Number of grid counts.
- `leverage`: Leverage value.
- `amount`: Investment amount.
- `position_type`: Position type (e.g., "neutral").
- `grid_mode`: Grid mode (e.g., "arithmetic").
- `type`: Type of bot (e.g., "futures-grid", "spot-grid", etc.).
- `demo`: Demo order (e.g., "true", "false").

Note: If `demo` param is 'false' or empty or undefined then it will create live orders.

**Response:**
```json
{
    "success": true,
    "msg": "Bot created successfully",
    "data": {
        "bot_id": "64774bca6e4fd7117c1c4018"
    }
}
```

---

## Gridbot Active API & Gridbot History API

**Endpoint:** `GET /api/bots/grid/open` \
**Endpoint:** `GET /api/bots/grid/history`

**Query Parameters:**
- `exchange`: Exchange name (e.g., "binance").
- `type`: Type of bot ("ALL", "GRID", or "DCA").

**Response:**
```json
{
    "_id": "64089d4659296e5f2c342206",
    "updatedAt": "2023-03-08T14:51:20.204Z",
    "createdAt": "2023-03-08T14:35:50.215Z",
    "exchange": "binance",
    "exchange_subaccount": "binance",
    "coin": "BTC/USDT",
    "lower_price": 22000,
    "upper_price": 25000,
    "grid_count": 50,
    "leverage": 1,
    "amount": 100,
    "grid_mode": "arithmetic",
    "running_time": "2 months",
    "type": "grid",
    "sub_type": "neutral",
    "amount_per_trade": 2,
    "entry_price": 21965.54,
    "step_price": 60,
    "grid_profit": {
        "low": 0.24057738572574178,
        "high": 0.2727272727272727,
        "low_actual": 0.040577385725741766,
        "high_actual": 0.0727272727272727
    },
    "user": "63f3285ece5b8341f852f498",
    "__v": 0,
    "pnl": 0.016784712443283403,
    "pnl_usd": 0.016784712443283403,
    "total_pnl": 0.016784712443283403,
    "total_pnl_usd": 0.016784712443283403,
    "stop_method": "exit",
    "buy_count": 0,
    "sell_count": 0,
    "status": "CLOSE"
}
```

---

## Gridbot Stop API

**Endpoint:** `POST /api/bots/grid/stop`

**Query Parameters:**
- `bot_id`: ID of the bot to stop.
- `stop_method`: Method to stop the bot ("cancel" or "exit").

**Response:**
```json
{
    "success": true,
    "msg": "Bot stopped successfully"
}
```

---

## Gridbot View Details API

**Endpoint:** `GET /api/bots/grid/details/:bot_id`

Retrieve details of a specific Gridbot.

**Example Usage:**
```http
GET /api/bots/grid/details/6xxxxxxxx4444xxxxxxxxxc7
```

**Response:**
```json
{
    "success": true,
    "msg": "Bot details fetched successfully",
    "data": {
        "_id": "6xxxxxxxx4444xxxxxxxxxc7",
        "updatedAt": "2023-05-30T18:16:26.184Z",
        "createdAt": "2023-05-30T18:08:04.728Z",
        "coin": "BTC/USDT",
        "lower_price": 19000,
        "upper_price": 32000,
        "grid_count": 10,
        "amount": 100,
        "type": "futures-grid",
        "exchange": "binance",
        "exchange_subaccount": "binance",
        "grid_mode": "arithmetic",
        "position_type": "long",
        "amount_per_trade": 10,
        "entry_price": 27675.67,
        "step_price": 1300,
        "grid_profit": {
            "low": 4.234527687296417,
            "high": 6.842105263157895,
            "low_actual": 4.034527687296417,
            "high_actual": 6.6421052631578945
        },
        "user": "5efe022ce86c150eecf73f0a",
        "price_levels": [
            {
                "buy_count": 1,
                "price": 27675.67,
                "_id": "6476xxxxxxxxxxxxe782d1"
            }
        ],
        "trades": [
            {
                "order_group_id": "4b82e4e6-34e9-458f-8ac0-4aae2cefdc62",
                "position_type": "long",
                "buy_price": 26800,
                "sell_price": 28100,
                "createdAt": "2023-05-30T18:08:05.265Z",
                "_id": "647xxxxxxxxxxxx82ca",
                "status": "OPEN"
            },
            {...},
            {...}
        ],
        "status": "CLOSE",
        "__v": 0,
        "apr": null,
        "pnl": null,
        "pnl_usd": null,
        "stop_method": "cancel",
        "closedAt": "2023-05-30T18:16:26.183Z",
        "duration": 501455,
        "total_pnl": 0,
        "bot_pnl": 0,
        "bot_pnl_quantity": 0,
        "position_pnl_quantity": 0,
        "total_pnl_quantity": 0
    }
}
```

---

## DCA Bot Recommended strategies
Endpoint: `GET /api/bots/dca/recommended`

### Parameters
- `sort` (optional): Sorting criteria for the strategy list (e.g., "apr", "risk", "rating", "24h_change", "volatility", "24h_vol"). Default sort is "24h_vol".
- `exchange` (optional): Exchange name (e.g., "binance").

### Response
```json
{
    "success": true,
    "data": [
        {
            "market": "BTC/USDT",
            "apr": -33.94314945,
            "risk": "moderate",
            "rating": "buy",
            "24h_change": -2.35913027,
            "volatility": 2.98589667,
            "24h_vol": 1249844393.3430703
        },
        {...},
        {...}
    ]
}
```

---

## DCA Bot Create API
Endpoint: `POST /api/bots/dca/create`

### Parameters
- `type`: Bot type (e.g., "dca").
- `exchange`: Exchange name (e.g., "binance").
- `coin`: Coin symbol (e.g., "ETH/USDT").
- `averaging_orders`: Number of averaging orders.
- `amount`: Trade amount.
- `step_percentage`: Step percentage.
- `profit_per_cycle`: Profit per cycle.
- `position_type`: Position type.
- `demo`: Demo order (e.g., "true", "false").

Note: If `demo` param is 'false' or empty or undefined then it will create live orders.

### Response
```json
{
    "success": true,
    "msg": "Bot created successfully",
    "data": {
        "bot_id": "64775eb66e4fd7117c1c4032"
    }
}
```

---

## DCA Bot Active & History API
Endpoint: `GET /api/bots/dca/open`  
Endpoint: `GET /api/bots/dca/history`

### Parameters
- `exchange` (optional): Exchange name (e.g., "binance").
- `type` (optional): Type of bot ("ALL", "GRID", or "DCA").

### Response
```json
{
    "success": true,
    "msg": "Bot list fetched successfully",
    "data": [
        {
            "_id": "64775eb66e4fd7117c1c4032",
            "updatedAt": "2023-05-31T14:50:36.437Z",
            "createdAt": "2023-05-31T14:50:30.089Z",
            "coin": "ETH/USDT",
            "amount": 100,
            "step_percentage": 1,
            "averaging_orders": 2,
            "profit_per_cycle": 5,
            "exchange": "binance",
            "exchange_subaccount": "binance",
            "type": "dca",
            "position_type": "short",
            "base_trade_amount": 33.333333333333336,
            "entry_price": 1861.79,
            "user": "5efe022ce86c150eecf73f0a",
            "status": "OPEN",
            "__v": 0,
            "apr": null,
            "pnl": null,
            "pnl_usd": null,
            "bot_pnl": 0,
            "position_pnl": 0,
            "total_pnl": 0,
            "bot_pnl_quantity": 0,
            "position_pnl_quantity": 0,
            "total_pnl_quantity": 0,
            "running_time": "2 minutes",
            "buy_count": 0,
            "sell_count": 1,
            "current_price": 1861.76
        },
        {...},
        {...}
    ]
}
```

---

## DCA Bot Stop API
Endpoint: `POST /api/bots/dca/stop`

### Query Parameters
- `bot_id`: ID of the bot to stop.
- `stop_method`: Method to stop the bot ("cancel" or "exit").

### Response
```json
{
    "success": true,
    "msg": "Bot stopped successfully"
}
```

---

## DCA Bot Details API
Endpoint: `GET /api/bots/dca/details/:bot_id`

### Query Parameters
- `bot_id`: ID of the bot to stop.

**Example Usage:**
```http
GET /api/bots/dca/details/6477xxxxx4fd711xxxxx032
```

### Response
```json
{
    "success": true,
    "msg": "Bot details fetched successfully",
    "data": {
        "_id": "6477xxxxx4fd711xxxxx032",
        "updatedAt": "2023-05-31T14:59:43.874Z",
        "createdAt": "2023-05-31T14:50:30.089Z",
        "coin": "ETH/USDT",
        "amount": 100,
        "step_percentage": 1,
        "averaging_orders": 2,
        "profit_per_cycle": 5,
        "exchange": "binance",
        "exchange_subaccount": "binance",
        "type": "dca",
        "position_type": "short",
        "base_trade_amount": 33.333333333333336,
        "entry_price": 1861.79,
        "user": "5efe0xxxxxxxxxxxxxxxx73f0a",
        "price_levels": [
            {
                "sell_count": 1,
                "price": 1861.57,
                "_id": "647xxxxxxxxxxxxbd"
            }
        ],
        "trades": [
            {
                "filled_price": 1861.57,
                "closedAt": "2023-05-31T14:50:36.436Z",
                "_id": "64775xxxxxxxxxxxxxxx4038",
                "order_group_id": "62xxxx-xxxx-xxxxx-xxxx-xxx-xxxxx-2exxxx7a0b",
                "position_type": "short",
                "side": "sell",
                "price": 1861.7713821,
                "createdAt": "2023-05-31T14:50:31.243Z",
                "status": "CLOSE"
            }
        ],
        "status": "CLOSE",
        "__v": 0,
        "apr": null,
        "pnl": null,
        "pnl_usd": null,
        "stop_method": "cancel",
        "position_pnl": 0,
        "closedAt": "2023-05-31T14:59:43.867Z",
        "duration": 553778,
        "total_pnl": 0,
        "bot_pnl": 0,
        "bot_pnl_quantity": 0,
        "position_pnl_quantity": 0,
        "total_pnl_quantity": 0
    }
}
```

---
