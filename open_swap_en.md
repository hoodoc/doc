
Hoo Access API Introduction
---

1. Request Format: GET and POST two methods are available.
2. After all the request parameters are sorted in character order, they are combined into a string in the form of「key=value&key1=value」and then encrypted, and the encrypted hexadecimal string is passed in the parameter sign.
3. The encryption method is: HMAC-SHA256, test key:12345, encryption result is expressed in hexadecimal, refer to python example
4. United Response Format of Access: {"code": "10000", "message": "success", "data": {}}, code=10000 means access success, other is access error
5. The access is restricted by IP address, which must be provided before the access is connected.

* Request Example (Python)
```plain
# coding=utf-8
import hmac
import hashlib

client_id = 'FFFFFFFBBBBXXXXXXCCCCCCCFFFFFF'
client_secret = 'FFFFFFFAAAAAAAABBBBBBXXXXXXXWQT8FFFFFFKKKKKKADXXXXXX'

# Sign
def get_payload(data):
    keys = sorted(data)
    sign_str = '&'.join(['%s=%s' % (k, data.get(k)) for k in keys])
    sign_mac = hmac.new(
        client_secret.encode("utf-8"),
        sign_str.encode("utf-8"),
        digestmod=hashlib.sha256)
    sign = sign_mac.hexdigest()
    payload = ""
    for k, v in data.items():
        payload += k + "=" + v + "&"
    payload += "sign=" + sign
    return payload
def get_swap_lp:
  url = 'https://wallet.hoogeek.com/api/open/swap/lp'
  data = {
      'client_id': client_id,
      'symbol': 'USDT-ETH',
  }
  payload = get_payload(data)
  print_resp(HttpPost(url, payload, None))
```
* Request Example (Curl)
```plain
curl -X POST 'https://wallet.hoogeek.com/api/open/swap/lp'\
-H 'Content-Type: application/x-www-form-urlencoded'
-d 'client_id=FFFFFFFBBBBXXXXXXCCCCCCCFFFFFF&symbol=USDT-ETH&sign=AAAAAAAA4a49de0b43e0aba1XXXXXXXX4d0353de0a5f445f6f8d85baaBBBBBBBB'
{
  "code": "10000",
  "message": "success",
  "data": {
    "records": [
      {
        "symbol": "USDT-ETH",
        "base_name": "USDT",
        "token_name": "ETH",
        "base_icon": "https://wallet.hoogeek.com/static_pc/icons/usdt.png?v=1",
        "token_icon": "https://wallet.hoogeek.com/static_pc/icons/eth.png?v=1",
        "lp_name": "USDT-ETH LP",
        "is_stablecoin": false,
        "base_amount": "95.0155",
        "token_amount": "11.6529",
        "percent": "2.58",
        "lp_amount": "33.22174066",
        "lp_freeze": "0",
        "fee_per": "1.38",
        "pool_year_rate": "0",
        "base_deposit": "96.875",
        "token_deposit": "11.4287",
        "lp_per": "-0.01"
      }
    ]
  }
}
```
### [Swap](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/ex
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format:ETH-USDT|
|send_coin|string|yes|Send token name|
|send_volume|decimal|yes|Send token amount|
|sign|string|yes|Signature|

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|is_routing|bool|Routing or not|
|mid_name|string|Mid-token name|
|mid_icon|string|Mid token icon|

###

### [Pool Volume](https://github.com/rylink/doc/blob/main/open_swap.md#reg)

```plain
POST /api/open/swap/poolvolume
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format: ETH-USDT|
|sign|string|yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|coin0|string|Coin1 name|
|coin1|string|Coin2 name|
|volume0|string|Coin1 value|
|volume1|string|Coin2 value|

```plain
{
    "message":"success",
    "code":"10000",
    "data":{
        "coin0": "ETH",
        "coin1": "USDT",
        "volume0": "123",
        "volume1": "123",
    }
}
```
### [Add Liquidity](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/addliquidity
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format: ETH-USDT|
|base_volume|decimal|yes|Trading coins(eg. ETH of ETN-USDT)amount|
|quote_volume|decimal|yes|Denomination coin (eg.<br>Number of USDT in ETH-USDT)|
|sign|string|yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|symbol|string|Pair name|
|base_name|string|Coin1 name|
|token_name|string|Coin2 name|
|lp_amount|string|LP amount|
|lp_name|string|LP name: eg,"BTC-USDT LP"|

```plain
{
    "message":"success",
    "code":"10000",
    "data":{
        "symbol": "ETH-USDT",
        "base_name": "ETH",
        "token_name": "USDT",
        "lp_amount": "123",
        "lp_name": "ETH-USDT LP"
    }
}
```
### [Remove Liquidity](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/removeliquidity
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format: ETH-USDT|
|percent|int|yes|Retrieved lp percentage, integer: 0 < percent <= 100|
|sign|string|yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|ts|string|Operation date|


### [Access to withdrawable assets (My Liquidity)](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/theoryassets
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format: ETH-USDT|
|sign|string|yes|Signature|
|    |    |yes|    |

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|symbol|string|Pair name|
|base_name|string|Coin1 name|
|token_name|string|Coin2 name|
|base_amount|string|Coin1 value|
|token_amount|string|Coin2 value|
|percent|string|LP Percentage（%）|
|lp_amount|string|Number of available liquidity tokens LP|
|freeze|string|Number of HooPool staking liquidity token LPs|
|lp_name|string|Liquidity token LP name, e.g.: "BTC-USDT LP"|
|is_stablecoin|string|Whether it is a stable coin pool or not|

```plain
{
    "message":"success",
    "code":"10000",
    "data": {
        "records": [
            {
                "symbol": "EOS-USDT"         # Pair name
                "base_name": "EOS",          # base currency，coin0
                "token_name": "USDT",        # token currency,coin1
                "base_amount": "123",        # base currency amount（Current market）
                "token_amount": "12345",     # tokencurrency amount（Current market）
                "percent": "1.2",            # LP percentage（%）
                "lp_amount": "123",          # Number of available liquidity tokens LP
                "lp_freeze": "123",          # Number of HooPool staking liquidity token LPs
                "lp_name": "EOS-USDT LP"     # Liquidity token LP name
                "is_stablecoin": false       # Whether it is a stable coin pool or not
            }
        ]
    }
}
```
###
### [Number of LPs Accessed](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
GET /api/open/swap/lp
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|sign|string|yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|pledged|string|lp amount|

```plain
{
  "code": "10000",
  "message": "success",
  "data": {
    "pledged": "0"
  }
}
```
###
### [Pairs Price History](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/price/history
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|yes|Access ID provided to the user by Hoo|
|symbol|string|yes|Pair: Format: ETH-USDT|
|date_begin|string|yes|Start date: Format: YYYYMMDD, e.g. 20201028|
|date_end|string|yes|Start date: Format: YYYYMMDD, such as 20201230, and date_end must be larger than date_begin|
|page|string|No|Page number, start from 1|
|page_size|string|No|Number of data items per page, default value is 100|
|sign|string|yes|Signature<br>|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|data|[]item|Data List|

**Item Data Structure**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|coin0|string|Coin1 name|
|coin1|string|Coin2 name|
|price1|string|Coin1 value|
|price2|string|Coin2 value|
|date|int|Date, format: YYMMDD|

```plain
{
  "code": "10000",
  "message": "success",
  "data": [
    {
      "coin1": "BTC",
      "coin2": "EOS",
      "price1": 0.99933163,
      "price2": 1.0006688,
      "date": 20201030
    },
    {
      "coin1": "BTC",
      "coin2": "EOS",
      "price1": 1.09484061,
      "price2": 0.91337495,
      "date": 20201106
    }
  ]
}
```
###
### [Pairs Price](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

GET /api/open/swap/price******Request parameters**

|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|Yes|Access ID provided to the user by Hoo|
|symbol|string|Yes|Pair: Format: ETH-USDT|
|sign|string|Yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|symbol|string|Yes|
|base_name|string|Yes|
|token_name|string|Yes|
|base_price|string|Coin1 Price|
|token_price|string|Coin1 Price|
|base_pool|string|Coin1 Balance|
|token_pool|string|Coin2 Balance|
|total_share|string|Total liquidity share|

```plain
{
  "code": "10000",
  "message": "success",
  "data": {
    "symbol": "BTC-EOS",
    "base_name": "BTC",
    "token_name": "EOS",
    "base_price": "0.08867639",
    "token_price": "11.27695844",
    "base_pool": "11260.24565172",
    "token_pool": "81.69276981",
    "total_share": "1317.63190305"
  }
}
```
###
### [Liquidity Pool 24-hour Trading Volume](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/trade/volume
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|Yes|Access ID provided to the user by Hoo|
|sign|string|Yes|Signature|
|coin_name|string|No|Pair: Format: ETH-USDT|
|symbol|string|No|Pair: Format: ETH－HOO|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|symbol|string|Yes|
|base_name|string|Yes|
|token_name|string|Yes|
|base_icon|string|Coin1icon|
|token_icon|string|Coin2icon|
|total_pool|string|Total pool share|
|volume_24h|string|Last 24h trading volume|
|volume_7d|string|Total shares traded in the past 7 days|
|fee_24h|string|Last 24h fees|
|v|float64|Total pool share|

```plain
response:
{
  "code": "10000",
  "message": "success",
  "data": {
    "records": [
      {
        "symbol": "BTC-EOS",
        "base_name": "BTC",
        "token_name": "EOS",
        "base_icon": "https://wallet.hoogeek.com/static_pc/icons/btc.png?v=1",
        "token_icon": "https://wallet.hoogeek.com/static_pc/icons/eos.png?v=1",
        "total_pool": "284105549.36390177",
        "volume_24h": "0",
        "volume_7d": "0",
        "fee_24h": "0",
        "v": 284105549.3639018
      }
    ],
    "pagenum": 1,
    "count": 1
  }
}
```
###
### [Trading Records](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/swap/trade/records
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|Yes|Access ID provided to the user by Hoo|
|sign|string|Yes|Signature|
|symbol|string|No|Pair: Format: ETH－HOO|
|coin_name|string|No|Token: Format: ETH|
|pagenum|string|No|Page|
|pagesize|string|No|Amount of data per page, range [5:100]|
|trade_type|string|No|1.deposit 2.withdraw 3.swap|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|data|[]item|Data list|

**Item Data Structure**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|title|string|Title: Deposit, Withdrawal|
|symbol|string|Pair: Format: ETH-HOO|
|wallet_id|string|Wallet ID|
|base_name|string|Coin 1 Name|
|token_name|string|Coin 2 Name|
|base_amount|string|Coin1 Value|
|token_amount|string|Coin2 Value|
|value|string|Transaction amount, if trade_type=3,then base_value+token_value|
|create_at|string|Date|

```plain
{
  "code": "10000",
  "data": {
    "records": [
      {
        "token_name": "HOO",
        "base_amount": "1",
        "fee": "0.003",
        "create_at": 1608277262,
        "symbol": "USDT-HOO",
        "base_name": "USDT",
        "title": "\u5151\u6362 USDT \u4e3a HOO",
        "wallet_id": "6****6",
        "token_amount": "2.62592727",
        "value": "1"
      },
      {
        "token_name": "USDT",
        "base_amount": "0.000001",
        "fee": "0",
        "create_at": 1608276982,
        "symbol": "USDT-HOO",
        "base_name": "HOO",
        "title": "\u5151\u6362 HOO \u4e3a USDT",
        "wallet_id": "6****6",
        "token_amount": "0.00000037",
        "value": "0.00000037"
      }
    ],
    "count": 11,
    "pagenum": 2
  },
  "message": "success"
}
```
###
### [Pool Info](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/pool/pools
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|
|client_id|string|Yes|Access ID provided to the user by Hoo|
|sign|string|Yes|Signature|
|name|string|No|If you do not specify the name, the data of all the tokens registered by the user will be responsed.|
|pagenum|string|No|Page number|
|pagesize|string|No|Data per page amount, in the range [5:100].|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|data|[]item|data list|

**Item Data Structure**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|pool_id|string|Poolid|
|name|string|Token name|
|logo1|string|Token1 logo|
|logo2|string|Token2 logo|
|multiple|string|Mining multiplier|
|pledge_type|string|Staking type|
|year_rate|string|APY|
|is_swap|string|Swap market making or not|
|symbol|string|Pair: Format:ETH－HOO|
|start_at|string|Start time|
|stop_at|string|End time|
|mining_coins|[]string|Mining tokens|
|mined_name|string|Name of mined tokens|
|today_supply|string|Number of mined tokens today|
|stop_at|string|End time|

```plain
{
  "code": "10000",
  "data": {
    "records": [
      {
        "mining_coins": [
          "HC"
        ],
        "name": "BTC",
        "pledge_type": 2,
        "stop_at": 1608868461,
        "multiple": "1",
        "start_at": 1606482243,
        "logo2": "",
        "logo1": "https://wallet.hoogeek.com/media/news_thumb/16064823003736422_5.png",
        "symbol": "BTC",
        "year_rate": "0",
        "is_swap": false,
        "pool_id": 28,
        "mined_name": "HOO",
        "today_supply": "10000",
      },
      {
        "mining_coins": [
        "EOS",
        "USDT"
        ],
        "name": "EOS-USDT",
        "pledge_type": 1,
        "stop_at": 1608775015,
        "multiple": "10",
        "start_at": 1606406400,
        "logo2": "https://wallet.hoogeek.com/media/news_thumb/16064826670180166_17.png",
        "logo1": "https://wallet.hoogeek.com/media/news_thumb/16064826636837983_12.png",
        "symbol": "EOS-USDT",
        "year_rate": "0",
        "is_swap": true,
        "pool_id": 29
      }
  ],
  "ts": 1609404934,
  "count": 18,
  "pagenum": 1
  },
"message": "success"
}
```
###
### [Get Pool Info Based on Pool ID](https://github.com/rylink/doc/blob/main/open_swap.md#ex)

```plain
POST /api/open/pool
```
**Request parameters**
|**Field name**|**Data Type**|**Required**|**Description**|
|:----|:----|:----|:----|:----|
|client_id|string|Yes|Access ID provided to the user by Hoo<br>|
|pool_id|string|Yes|PoolID|
|sign|string|Yes|Signature|

**Response Content**

|**Field name**|**Data Type**|**Description**|
|:----|:----|:----|
|pool_id|string|Poolid|
|name|string|币种名称|
|logo1|string|Token1 logo|
|logo2|string|Token2 logo|
|multiple|string|Mining multiplier|
|pledge_type|string|Staking type|
|year_rate|string|APY|
|is_swap|string|Swap market making or not|
|symbol|string|Pair: Format: ETH－HOO|
|start_at|string|Start time|
|stop_at|string|End time|
|mining_coins|[]string|Mining tokens|

```plain
{
  "data": {
    "name": "EOS",
    "symbol": "EOS",
    "multiple": "3",
    "year_rate": "0",
    "pool_id": 39,
    "stop_at": 1608868739,
    "logo1": "https://wallet.hoogeek.com/media/news_thumb/16088677054857416_19.png",
    "logo2": "",
    "mining_coins": [
      "EOS"
    ],
    "start_at": 1608867680,
    "pledge_type": 2,
    "is_swap": false
  },
  "code": "10000",
  "message": "success"
}
```
