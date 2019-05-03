# FinexBox

The open api of finexbox

## API Documentation

## Public API Methods

    Note that if you use more than two calls per second for a public API and/or perform a unnecessary retrieval of redundant volumes of data, your IP address may be banned.

Code for use in public functions:

```PHP
$href = 'https://xapi.finexbox.com/v1/market';
$ch = curl_init($href);
$obj = json_decode(curl_exec($ch));
```

### Get the entire market

`/api/v1/market`

    Returns last 24-hour summary of all active markets.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "market": "BTC_USD",
      "currency": "Bitcoin",
      "base": "USD",
      "volume": "6.81629536",
      "high": "12475.09000000",
      "low": "11225.82000000",
      "price": "11901.00000000",
      "average": "11850.45500000",
      "percent": "-4.60",
      "timestamp": 1517020192,
      "active": true
    },
    {
      "market": "BCH_USD",
      "currency": "Bitcoin Cash",
      "base": "USD",
      "volume": "20.50012296",
      "high": "1763.00000000",
      "low": "1603.01200000",
      "price": "1741.41900000",
      "average": "1683.00600000",
      "percent": "0.25",
      "timestamp": 1517020192,
      "active": true
    }
  ]
}
```

`Call: https://xapi.finexbox.com/v1/market`

### Ticker for a pair

`/api/v1/ticker`

    Used to get current tick values for the market.
    
Sample output:

```JSON
{
  "success": true,
  "result": {
    "volume": "0.00000000",
    "high": "0.00000000",
    "low": "0.00000000",
    "open": "0.00000000",
    "price": "0.40000000",
    "average": "0.00000000",
    "percent": "0.00",
    "timestamp": 1522774844
  }
}
```

Required parameters:
- market

`Call: https://xapi.finexbox.com/v1/ticker?market=doge_btc`

### Get orders for a pair

`/api/v1/order`

    Returns the order book for this market.
    
Sample output:

```JSON
{
  "success": true,
  "result": {
    "buy": [
      {
        "price": "14813.87600000",
        "amount": "0.00105677",
        "timestamp": 1515638453
      },
      {
        "price": "14799.00000000",
        "amount": "0.00100000"
      }
    ],
    "sell": [
      {
        "price": "13843.91100000",
        "amount": "0.00274425",
        "timestamp": 1515644518
      },
      {
        "price": "14007.00000000",
        "amount": "0.00399200"
      },
      {
        "price": "14386.00000000",
        "amount": "0.02650000"
      }
    ]
  }
}
```

Required parameters:
- market
- count (max. 200)

`Call: https://xapi.finexbox.com/v1/orders?market=doge_btc&count=100`

### Get historical transactions of a pair

`/api/v1/history`

    Gets the historical transactions of a particular market.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "price": "14489.00000000",
      "amount": "0.01634253",
      "total": "236.78691717",
      "type": "buy",
      "timestamp": 1515680304
    },
    {
      "price": "14513.44400000",
      "amount": "0.00064696",
      "total": "9.38961773",
      "type": "buy",
      "timestamp": 1515680222
    }
  ]
}
```

Required parameters:
- market,
- count (max. 200)

`Call: https://xapi.finexbox.com/v1/history?market=doge_btc&count=100`<br>


### Get historical trade data about a pair

`/api/v1/chart`

    Gets candles in the format of historical trades of a particular market.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "price": "14489.00000000",
      "amount": "0.01634253",
      "total": "236.78691717",
      "type": "buy",
      "timestamp": 1515680304
    },
    {
      "price": "14326.27600000",
      "amount": "0.01077543",
      "total": "154.37178420",
      "type": "buy",
      "timestamp": 1515657104
    }
  ]
}
```

Required parameters:
- market
- period (in minutes)

`Call: https://xapi.finexbox.com/v1/chart?market=doge_btc&period=30`

## Private API Methods

    INFO: The private API is in development. Features might be added or removed without warning.

Code for use in private functions:

```PHP
$key = 'YOUR_API_KEY'; $secret = 'YOUR_API_SECRET';
$nonce = microtime();
$signature = hash_hmac('sha512', https://xapi.finexbox.com/v1/private/balances, $secret);
$href = 'https://xapi.finexbox.com/v1/private/balances?apikey='.$key.'&nonce='.$nonce.'&signature='.$signature;
$ch = curl_init($href);
$obj = json_decode(curl_exec($ch));
```

### Get a token

For successful authentication, you need to send the nonce. Nonce is a timestamp in integer, stands for milliseconds elapsed since Unix epoch. Tonce must be within 30 seconds of server's current time. Each nonce can only be used once.

You can generate your keys on this page: API KEYS

### Get the balance of a pair on your account

`/api/v1/private/balance`

Get the balance of a specific currency.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "currency": "BTC",
      "balance": 0.46843,
      "available": 0.16843,
      "pending": 0,
      "address": "13LdW6iSetHpVzKLYbU24x5ijGQcTv1cix"
    }
  ]
}
```

Required parameters:
- apikey
- nonce
- signature
- market

`Call: https://xapi.finexbox.com/v1/private/balance?apikey=4b62-53ce1d-143c-56e&nonce=1515737101&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&market=btc_usd`

### Get all balances of your account

`api/v1/private/balances`

    Get the balance of all your coins.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "currency": "BTC",
      "balance": 0.46843,
      "available": 0.16843,
      "pending": 0,
      "address": "13LdW6iSetHpVzKLYbU24x5ijGQcTv1cix"
    },
    {
      "currency": "DASH",
      "balance": 6.48893,
      "available": 6.25843,
      "pending": 0,
      "address": "XoU1B5wTZYKnhWhBmmn4gZayKxEvRp3x7s"
    }
  ]
}
```

Required parameters:
- apikey
- nonce
- signature

`Call: https://xapi.finexbox.com/v1/private/balances?apikey=4b62-53ce1d-143c-56e&nonce=1515737101&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

### Create a order / trade for a pair

`/api/v1/private/trade`

    Create orders and trade on the trade.

Sample output:

```JSON
{
  "success": true,
  "result": {
    "orderid": 7456
  }
}
```

Required parameters:
- apikey
- nonce
- signature
- market
- type (sell, buy)
- price
- amount

`Call: https://xapi.finexbox.com/v1/private/trade?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&market=btc_usd&type=sell&price=14000.45&amount=0.0345`

### Cancel an order

`/api/v1/private/cancel`

    Cancel an order.

Sample output:

```
{
  "success": true
}
```

Required parameters:
- apikey
- nonce
- signature
- orderid

`Call: https://xapi.finexbox.com/v1/private/cancel?apikey=4b62-53ce1d-143c-56e&nonce=1515737103&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&orderid=2356`

### Get data about a order

`/api/v1/private/order`

    Get order data.

Sample output:
```JSON
{
  "success": true,
  "result": {
    "orderid": 596,
    "market": "btc_usd",
    "type": "sell",
    "amount": 1,
    "price": 16500,
    "status": "open",
    "time": 1415680355
  }
}
```

Required parameters:
- apikey
- nonce
- signature
- orderid

`Call: https://xapi.finexbox.com/v1/private/order?apikey=4b62-53ce1d-143c-56e&nonce=1515737104&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&orderid=596`

### Get all orders you placed

`/api/v1/private/orders`

    Get the list your open orders.

Sample output:

```JSON
{
  "success": true,
  "result": [
    {
      "orderid": 546,
      "market": "btc_usd",
      "type": "buy",
      "amount": 20,
      "price": 16000,
      "status": "open",
      "time": 1415680355
    },
    {
      "orderid": 596,
      "market": "btc_usd",
      "type": "sell",
      "amount": 1,
      "price": 16500,
      "status": "open",
      "time": 1415680555
    }
  ]
}
```

Required parameters:
- apikey
- nonce
- signature

`Call: https://xapi.finexbox.com/v1/private/orders?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

### Get your order history

`/api/v1/private/orderhistory`

    Get the list historic trades.

Sample output:
```
{
  "success": true,
  "result": [
    {
      "orderid": 504,
      "market": "btc_usd",
      "type": "buy",
      "amount": 6,
      "price": 14060,
      "status": "closed",
      "time": 1415660355
    },
    {
      "orderid": 506,
      "market": "btc_usd",
      "type": "sell",
      "amount": 0.056,
      "price": 15900,
      "status": "closed",
      "time": 1415660455
    }
  ]
}
```

Required parameters:
- apikey
- nonce
- signature

`Call: https://xapi.finexbox.com/v1/private/orderhistory?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

### Withdraw coins to your personal wallet

`/api/v1/private/withdraw`

    Withdraw their currencies to another wallet.

Sample output:

```JSON
{
  "success": true
}
```

Required parameters:
- apikey
- nonce
- signature
- currency
- amount
- address

`Call: https://xapi.finexbox.com/v1/private/withdraw?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&currency=btc&amount=0.2341&address=13LdW6iSetHpVzKLYbU24x5ijG`
