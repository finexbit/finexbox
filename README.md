# FinexBox
The open api of finexbox

API Documentation
1) Public API Methods
2) Private API Methods<br><br>

Public API Methods<br>

https://coinmarketcap.com/  api


Note that if you use more than two calls per second for a public API or multiple and unnecessary retrieval of redundant volumes of data, your IP address may be banned.<br>

Code for use in public functions:<br>

$href = 'https://xapi.finexbox.com/v1/market';<br>
$ch = curl_init($href);  <br>
$obj = json_decode(curl_exec($ch));  <br>


/api/v1/market<br>
Returns last 24-hour summary of all active markets. Sample output:<br>
```
{"success":true, "result":[{"market":"BTC_USD", "currency":"Bitcoin", "base":"USD", "volume":"6.81629536", "high":"12475.09000000", "low":"11225.82000000", "price":"11901.00000000", "average":"11850.45500000", "percent":"-4.60", "timestamp":1517020192, "active":true}, {"market":"BCH_USD", "currency":"Bitcoin Cash", "base":"USD", "volume":"20.50012296", "high":"1763.00000000", "low":"1603.01200000", "price":"1741.41900000", "average":"1683.00600000", "percent":"0.25", "timestamp":1517020192, "active":true}]}
```
`Call: https://xapi.finexbox.com/v1/market`

/api/v1/ticker<br>
Used to get current tick values for the market. Sample output:<br>
```
{"success":true,"result":{"volume":"0.00000000", "high":"0.00000000", "low":"0.00000000", "open":"0.00000000", "price":"0.40000000", "average":"0.00000000", "percent":"0.00", "timestamp":1522774844}}
```
Required parameters: market<br>

`Call: https://xapi.finexbox.com/v1/ticker?market=doge_btc`

/api/v1/orders<br>
Returns the order book for this market. Sample output:<br>
```
{"success":true, "result":{"buy":[{"price":"14813.87600000", "amount":"0.00105677", "timestamp":1515638453},{"price":"14799.00000000", "amount":"0.00100000"}],"sell":[{"price":"13843.91100000", "amount":"0.00274425", "timestamp":1515644518},{"price":"14007.00000000", "amount":"0.00399200"},{"price":"14386.00000000", "amount":"0.02650000"}]}}
```
Required parameters: market, count (max: 200)<br>

`Call: https://xapi.finexbox.com/v1/orders?market=doge_btc&count=100`

/api/v1/history<br>
Gets the historical transactions of a particular market. Sample output:<br>
```
{"success":true, "result":[{"price":"14489.00000000", "amount":"0.01634253", "total":"236.78691717", "type":"buy", "timestamp":1515680304},{"price":"14513.44400000", "amount":"0.00064696", "total":"9.38961773", "type":"buy", "timestamp":1515680222},...]}
```
Required parameters: market, count (max: 200)<br>

`Call: https://xapi.finexbox.com/v1/history?market=doge_btc&count=100`<br>

/api/v1/chart<br>
Gets candles in the format of historical trades of a particular market. Sample output:<br>
```
{"success":true, "result":[{"price":"14489.00000000", "amount":"0.01634253", "total":"236.78691717", "type":"buy", "timestamp":1515680304},{"price":"14326.27600000", "amount":"0.01077543", "total":"154.37178420", "type":"buy", "timestamp":1515657104}]}
```
Required parameters: market, period in min. (5, 15, 30, 60, 360, 720, 1440, 10080)<br>

`Call: https://xapi.finexbox.com/v1/chart?market=doge_btc&period=30`












Private API Methods<br>
(Private API in progress of development)<br><br>


Code for use in private functions:<br>
`
$key = 'YOUR_API_KEY'; $secret = 'YOUR_API_SECRET';<br>
$nonce = microtime();<br>
$signature = hash_hmac('sha512', https://xapi.finexbox.com/v1/private/balances, $secret);<br>
$href = 'https://xapi.finexbox.com/v1/private/balances?apikey='.$key.'&nonce='.$nonce.'&signature='.$signature;<br>
$ch = curl_init($href);<br>
$obj = json_decode(curl_exec($ch));`<br>
For successful authentication, you need to send the nonce. Nonce is a timestamp in integer, stands for milliseconds elapsed since Unix epoch. Tonce must be within 30 seconds of server's current time. Each tonce can only be used once.<br>

You can generate your keys on this page: API KEYS<br><br>

/api/v1/private/balance<br>
Get the balance of a specific currency. Sample output:<br>
```
{"success":true,"result":[{"currency":"BTC", "balance":0.46843000,"available":0.16843000,"pending":0.00000000, "address":"13LdW6iSetHpVzKLYbU24x5ijGQcTv1cix"}]}
```
Required parameters: market<br>

`Call: https://xapi.finexbox.com/v1/private/balance?apikey=4b62-53ce1d-143c-56e&nonce=1515737101&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&market=btc_usd`

/api/v1/private/balances<br>
Get the balance of all your coins. Sample output:<br>
```
{"success":true,"result":[{"currency":"BTC", "balance":0.46843000,"available":0.16843000,"pending":0.00000000, "address":"13LdW6iSetHpVzKLYbU24x5ijGQcTv1cix"},{"currency":"DASH", "balance":6.48893000,"available":6.25843000,"pending":0.00000000, "address":"XoU1B5wTZYKnhWhBmmn4gZayKxEvRp3x7s"}]}
```
`Call: https://xapi.finexbox.com/v1/private/balances?apikey=4b62-53ce1d-143c-56e&nonce=1515737101&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

/api/v1/private/trade<br>
Create orders and trade on the trade. Sample output:<br>
```
{"success":true, "result":{"orderid":7456}}
```
Required parameters: market, type (sell, buy), price, amount<br>

`Call: https://xapi.finexbox.com/v1/private/trade?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&market=btc_usd&type=sell&price=14000.45&amount=0.0345`

/api/v1/private/cancel<br>
Cancel an order. Sample output:<br>
```
{"success":true}
```
Required parameters: orderid<br>

`Call: https://xapi.finexbox.com/v1/private/cancel?apikey=4b62-53ce1d-143c-56e&nonce=1515737103&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&orderid=2356`

/api/v1/private/order<br>
Get order data. Sample output:<br>
```
{"success":true, "result":{"orderid":596", "market":"btc_usd", "type":"sell", "amount":1.00000000,"price":16500.00000000, "status":"open", time":1415680355}}
```
Required parameters: orderid<br>

`Call: https://xapi.finexbox.com/v1/private/order?apikey=4b62-53ce1d-143c-56e&nonce=1515737104&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&orderid=596`

/api/v1/private/orders<br>
Get the list your open orders. Sample output:<br>
```
{"success":true, "result":[{"orderid":546,"market":"btc_usd", "type":"buy", "amount":20.00000000,"price":16000.00000000, "status":"open", time":1415680355},{"orderid":596", "market":"btc_usd", "type":"sell", "amount":1.00000000,"price":16500.00000000, "status":"open", time":1415680555}]}
```
`Call: https://xapi.finexbox.com/v1/private/orders?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

/api/v1/private/orderhistory<br>
Get the list historic trades. Sample output:<br>
```
{"success":true, "result":[{"orderid":504,"market":"btc_usd", "type":"buy", "amount":6.00000000,"price":14060.00000000, "status":"closed", time":1415660355},{"orderid":506", "market":"btc_usd", "type":"sell", "amount":0.05600000,"price":15900.00000000, "status":"closed", time":1415660455}]}
```
`Call: https://xapi.finexbox.com/v1/private/orderhistory?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb`

/api/v1/private/withdraw<br>
Withdraw their currencies to another wallet. Sample output:<br>
```
{"success":true}
```
Required parameters: currency, amount, address<br>

`Call: https://xapi.finexbox.com/v1/private/withdraw?apikey=4b62-53ce1d-143c-56e&nonce=1515737102&signature=LXcegwerdDdf79nM62dv9bZAG3Jp8EwGYnzoogb&currency=btc&amount=0.2341&address=13LdW6iSetHpVzKLYbU24x5ijG`
