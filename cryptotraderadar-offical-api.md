# API

### 1. General API Information 

* The base endpoint for PUBLIC REST API is: https://cryptotraderadar.com/api
* The base endpoint for WEBSOCKET API is: wss://gateway.cryptotraderadar.com/v1

### 2. Public API Methods

#### Exchange Settings
If you want to know which exchanges are currently being used by our platform, and which pairs we send orderbooks for.
#### IMPORTANT: The exchanges and pairs we use will be subject to change from time time, please always check if there was a change. ####

#### Request
```
GET /wpf/exchange_settings
```

#### Response

```yaml
{
    "exchangesSettings": [
      {"name":"BITTREX"},
      {"name":"BINANCE"},
      ...
      ]
    "exchangePairsSettings": [
      {"exchange":"KRAKEN","coin":"XTZ","baseCoin":"BTC","pair":"XTZ-BTC"},
      {"exchange":"KRAKEN","coin":"LINK","baseCoin":"BTC","pair":"LINK-BTC"},
      {"exchange":"KRAKEN","coin":"ETH","baseCoin":"BTC","pair":"ETH-BTC"},
      ...
      ]
    "allPairs": [
      "XTZ-BTC",
      "LINK-BTC",
      "ETH-BTC",
      "ADC-BTC",
      ...
      ]
}
```

### 3. WebSocket API Methods
If you wish to use our WebSocket API, you must first register at https://www.cryptotraderadar.com
Once you sign up, you will receive an WebSocket API Key so you can use to subscribe to our websocket channels.
By default, anyone who signs up, can use our platform for free up to 2 exchanges (Binance and Okex).

* You can download our optional software tool to test it out.
* Please be aware that you must send ping every 30 seconds to keep the connection alive.

#### Ping

#### Request
```yaml
  {
    "Channel":"ping",
    "Timestamp":"1603992146", (in milliseconds)
  }
```

#### Response
```yaml
  {
    "Channel":"pong",
    "Timestamp":"1603992146", (in milliseconds)
  }
```


After you connect to our websocket (via your software) you can subscribe to the following channels:

* Subscribe to Free orderbook

If you are a free level user, and want to test our orderbook, you can choose any pair you want to subscribe from our default exchanges.

##### Example:

#### Request
```yaml
  {
    "Channel":"subscribe_free_orderbook",
    "Pair":"ETH-BTC",
    "Key":"yourWebSocketApiKey"
  }
```

#### Response
```yaml
  {
    "IsBid":true, (true = bid order)
    "Pair":"ETH-BTC",
    "Exchange":"BINANCE",
    "Price":0.028879,
    "Amount":0.0, (Amount 0 means this order is finished)
    "Channel":"subscribe_free_orderbook"
  }
  ...
  {
    "IsBid":false, (false = ask order)
    "Pair":"ETH-BTC",
    "Exchange":"BINANCE",
    "Price":0.02889,
    "Amount":19.004,
    "Channel":"subscribe_free_orderbook"
  }
  ...
```

* Unsubscribe to Free orderbook

#### Request
```yaml
  {
    "Channel":"unsubscribe_free_orderbook",
    "Pair":"ETH-BTC",
    "Key":"yourWebSocketApiKey"
  }
```

#### Response
```yaml
No response will be sent, you will just stop receiving orderbook updates.
```

 
* Subscribe to Top orderbook (requires to be a paid subscriber)

This will provide you the best 5 asks and bids per selected exchange.

##### Example:

#### Request
```yaml
  {
    "Channel":"subscribe_top_orderbook",
    "Pair":"ETH-BTC",
    "Exchange":"Binance",
    "Key":"yourWebSocketApiKey"
  }
```

#### Response
```yaml
  {
    "Asks": [
      {"Price":0.028845,"Size":25.314},
      {"Price":0.028847,"Size":0.172},
      {"Price":0.028849,"Size":0.172},
      {"Price":0.02885,"Size":1.0},
      {"Price":0.028853,"Size":11.749}
    ],
    "Bids": [
      {"Price":0.028844,"Size":1.942},
      {"Price":0.028843,"Size":0.007},
      {"Price":0.028842,"Size":1.034},
      {"Price":0.028841,"Size":7.363},
      {"Price":0.02884,"Size":1.036}
    ],
    "Exchange":"BINANCE",
    "Pair":"ETH-BTC",
    "Channel":"subscribe_top_orderbook"
  }
```

* Unsubscribe from Top orderbook

#### Request
```yaml
  {
    "Channel":"unsubscribe_top_orderbook",
    "Pair":"ETH-BTC",
    "Exchange":"Binance",
    "Key":"yourWebSocketApiKey"
  }
```

#### Response
```yaml
No response will be sent, you will just stop receiving orderbook updates.
```
 
 
* Subscribe to Full orderbook (requires to be a paid subscriber)

This will provide you the best asks and bids per from all of our active exchanges.
At first response from our websocket you will receive full orderbook (according to selected depth, or default depth)
Every response after that will be an update to the orderbook (new order, updated order etc..)

##### Example:

#### Request
```yaml
  {
    "Channel":"subscribe_full_orderbook",
    "Pair":"ETH-BTC",
    "Key":"yourWebSocketApiKey"
    "Depth":5 (optional, default = 50, max = 50)
  }
```

#### Response (First)
```yaml
  {
    "Pair": "ETH-BTC",
    "Asks": [
      {
        "Amounts": [
          {
            "Exchange": "COINEX",
            "Amount": 12.018619
          }
        ],
        "Price": 0.02884555,
        "Amount": 12.018619
      },
      {
        "Amounts": [
          {
            "Exchange": "BIBOX",
            "Amount": 12.1565
          }
        ],
        "Price": 0.02887077,
        "Amount": 12.1565
      },
      {
        "Amounts": [
          {
            "Exchange": "HITBTC",
            "Amount": 0.9
          }
        ],
        "Price": 0.028876,
        "Amount": 0.9
      },
      {
        "Amounts": [
          {
            "Exchange": "HITBTC",
            "Amount": 0.5
          }
        ],
        "Price": 0.028878,
        "Amount": 0.5
      },
      {
        "Amounts": [
          {
            "Exchange": "COINEX",
            "Amount": 0.1
          },
          {
            "Exchange": "HITBTC",
            "Amount": 2.4865
          }
        ],
        "Price": 0.02888,
        "Amount": 2.5865
      }
    ],
    "Bids": [
      {
        "Amounts": [
          {
            "Exchange": "OKEX",
            "Amount": 27.929529
          }
        ],
        "Price": 0.02899,
        "Amount": 27.929529
      },
      {
        "Amounts": [
          {
            "Exchange": "OKEX",
            "Amount": 35.467539
          }
        ],
        "Price": 0.02898,
        "Amount": 35.467539
      },
      {
        "Amounts": [
          {
            "Exchange": "OKEX",
            "Amount": 27.49
          }
        ],
        "Price": 0.02897,
        "Amount": 27.49
      },
      {
        "Amounts": [
          {
            "Exchange": "OKEX",
            "Amount": 3.04
          }
        ],
        "Price": 0.02896,
        "Amount": 3.04
      },
      {
        "Amounts": [
          {
            "Exchange": "OKEX",
            "Amount": 4.449398
          }
        ],
        "Price": 0.02895,
        "Amount": 4.449398
      }
    ],
    "Channel": "subscribe_full_orderbook"
  }
```
 
#### Response (Second)
```yaml
  {
    "IsBid": true,
    "Pair": "ETH-BTC",
    "Exchange": "BITFINEX",
    "Price": 0.02888,
    "Amount": 2.4365409,
    "Channel": "subscribe_full_orderbook"
  }
```

#### Response (Third etc..)
```yaml
  {
    "IsBid": true,
    "Pair": "ETH-BTC",
    "Exchange": "BITFINEX",
    "Price": 0.028879,
    "Amount": 0.0,
    "Channel": "subscribe_full_orderbook"
  }
```

* Unsubscribe to Full orderbook

#### Request
```yaml
  {
    "Channel":"unsubscribe_full_orderbook",
    "Pair":"ETH-BTC",
    "Key":"yourWebSocketApiKey"
  }
```

#### Response
```yaml
No response will be sent, you will just stop receiving orderbook updates.
```

