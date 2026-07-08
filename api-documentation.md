API Documentation

Ridgeline Perps API Reference

Base URL

https://api.ridgelineperps.xyz/v1

Authentication
All endpoints require a bearer token in the header:

Authorization: Bearer YOUR_API_KEY


GET /account/balance

Returns current collateral and margin usage.

Response

json{
  "available_collateral": 500.00,
  "used_margin": 120.00,
  "unrealized_pnl": 8.42
}


POST /orders

Places a new order.

FieldTypeRequiredDescriptionmarketstringYese.g. "BTC-PERP"sidestringYes"long" or "short"sizenumberYesPosition size in USDleveragenumberYes1–20order_typestringYes"market" or "limit"limit_pricenumberConditionalRequired if order_type is "limit"

Response

json{
  "order_id": "ord_7c2f91ab",
  "status": "filled",
  "entry_price": 3021.50,
  "liquidation_price": 2480.10
}


GET /positions

Returns all open positions for the authenticated account.

Response

json[
  {
    "position_id": "pos_44a1",
    "market": "ETH-PERP",
    "side": "long",
    "size": 100,
    "leverage": 3,
    "entry_price": 3021.50,
    "mark_price": 3040.10,
    "unrealized_pnl": 1.85
  }
]


DELETE /positions/{position_id}

Closes an open position, fully or partially.

FieldTypeRequiredDescriptionclose_percentagenumberNoDefaults to 100 (full close) if omitted


GET /markets/{market}/funding

Returns the current and historical funding rate for a market.

Response

json{
  "market": "BTC-PERP",
  "current_funding_rate": 0.00012,
  "next_funding_time": "2026-07-06T16:00:00Z"
}


Rate limits: 10 requests/second per API key across all endpoints. Exceeding this returns 429 Too Many Requests with a Retry-After header.

Error codes

CodeMeaning400Invalid or missing request field401Invalid or expired API key403Insufficient collateral for requested action404Resource not found (e.g., invalid position_id)429Rate limit exceeded


