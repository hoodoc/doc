Hoo开放API接口说明
---------

1. 接口仅支持GET和POST请求
2. 所有请求参数按照按字符顺序进行排序后，以「key=value&key1=value」的方式组合成字符串然后进行加密，将加密后的十六进制字符串放入参数sign中一并传入
3. 加密方式为：HMAC-SHA256，测试key：12345，加密结果以十六进制表示，参考python示例
4. 接口统一返回格式：{"code": "10000", "message": "success", "data": {}}，code=10000时接口成功，其他为接口错误
5. 接口有IP地址限制，接口接入前须提供接入的IP地址

```json
    签名示例(Python)：
    import hmac
    import hashlib

    sign_str = "key=value&key1=value1"
    secret_key = "12345"
    sign = hmac.new(secret_key.encode("utf-8"), sign_str.encode("utf-8"), digestmod=hashlib.sha256).hexdigest()
```

### [<font id=ex color=blue>兑换</font>](#ex)
    POST /api/open/swap/ex  
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|send_coin|string|是|发起兑换的币种名称|
|send_volume|decimal|是|发起兑换的币种数量|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|is_routing|bool|是否路由|
|mid_name|string|中间币名称|
|mid_icon|string|中间币icon|

### [<font id=reg color=blue>获取池子数量</font>](#reg)
    POST /api/open/swap/poolvolume

请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|coin0|string|币种1名称|
|coin1|string|币种2名称|
|volume0|string|币种1金额|
|volume1|string|币种2金额|

```json
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
### [<font id=ex color=blue>增加流动性</font>](#ex)
    POST /api/open/swap/addliquidity

请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|base_volume|decimal|是|交易币种(eg. ETN-USDT中的ETH)数量|
|quote_volume|decimal|是|计价币种(eg. ETH-USDT中的USDT)数量|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|symbol|string|币种对名称|
|base_name|string|币种1名称|
|token_name|string|币种2名称|
|lp_amount|string|流动性份数|
|lp_name|string|流动性名称：如:"BTC-USDT LP"|


```json
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

### [<font id=ex color=blue>减少流动性</font>](#ex)
    POST /api/open/swap/removeliquidity  
请求参数   
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|percent|int|是|取出的lp百分比，整数：0 < percent <= 100|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|ts|string|操作日期|



### [<font id=ex color=blue>获取可提取资产（我的做市）</font>](#ex)
    POST /api/open/swap/theoryassets  
请求参数   
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|否|交易对：格式：ETH-USDT|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|symbol|string|币种对名称|
|base_name|string|币种1名称|
|token_name|string|币种2名称|
|base_amount|string|币种1金额|
|token_amount|string|币种2金额|
|percent|string|LP占比（%）|
|lp_amount|string|可用流动性代币LP数量|
|freeze|string|HooPool质押流动性代币LP数量|
|lp_name|string|流动性代币LP名称, 如:"BTC-USDT LP"|
|is_stablecoin|string|是否稳定币池子|

```json
{
    "message":"success",
    "code":"10000",
    "data": {
        "records": [
            {
                "symbol": "EOS-USDT"         # 交易对名称
                "base_name": "EOS",          # base币种，coin0
                "token_name": "USDT",        # token币种,coin1
                "base_amount": "123",        # base币种数量（当前做市）
                "token_amount": "12345",     # token币种数量（当前做市）
                "percent": "1.2",            # LP占比（%）
                "lp_amount": "123",          # 可用流动性代币LP数量
                "lp_freeze": "123",          # HooPool质押流动性代币LP数量
                "lp_name": "EOS-USDT LP"     # 流动性代币LP名称
                "is_stablecoin": false       # 是否稳定币池子
            }
        ]
    }
}
```


### [<font id=ex color=blue>获取LP数量</font>](#ex)
    GET /api/open/pool/lp
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|pool_id|string|是|池子id｜
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|pledged|string|lp数量|

```json
{
  "code": "10000",
  "message": "success",
  "data": {
    "pledged": "0"
  }
}
```


### [<font id=ex color=blue>交易对历史价格</font>](#ex)
    POST /api/open/swap/price/history
请求参数   
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|date_begin|string|是|起始日期：格式：YYYYMMDD，如20201028|
|date_end|string|是|起始日期：格式：YYYYMMDD，如20201230, 且date_end须比date_begin大|
|page|string|否|页码，从1开始|
|page_size|string|否|每页数据条数,默认值为100|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|data|[]item|数据列表|

item数据结构  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|coin0|string|币种1名称|
|coin1|string|币种2名称|
|price1|string|币种1价格|
|price2|string|币种2价格|
|date|int|日期，格式：YYMMDD|

```json
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

### [<font id=ex color=blue>交易对价格</font>](#ex)
  GET /api/open/swap/price
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|symbol|string|是|交易对：格式：ETH-USDT|
|base_name|string|是|交易对：格式：ETH|
|token_name|string|是|交易对：格式：USDT|
|base_price|string|币种1交易价格|
|token_price|string|币种2交易价格|
|base_pool|string|币种1余额|
|token_pool|string|币种2余额|
|total_share|string|流动性总份数|

```json
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


### [<font id=ex color=blue>流动池24小时交易量</font>](#ex)
    POST /api/open/swap/trade/volume
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|否|交易对：格式：ETH－HOO|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|symbol|string|是|交易对：格式：ETH-USDT|
|base_name|string|是|交易对：格式：ETH|
|token_name|string|是|交易对：格式：USDT|
|base_icon|string|币种1 icon|
|token_icon|string|币种2 icon|
|total_pool|string|池子总份额|
|volume_24h|string|过去24小时交易量|
|volume_7d|string|过去7天交易量总份数|
|fee_24h|string|过去24小时手续费|
|v|float64|池子总份额|

```json
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
        "base_icon": "https://wallet.test.hoogeek.com/static_pc/icons/btc.png?v=1",
        "token_icon": "https://wallet.test.hoogeek.com/static_pc/icons/eos.png?v=1",
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


### [<font id=ex color=blue>交易记录</font>](#ex)
    POST /api/open/swap/trade/records
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|否|交易对：格式：ETH－HOO|
|coin_name|string|否|货币名称：格式：ETH|
|pagenum|string|否|页码｜
|pagesize|string|否|每页数据量，范围[5:100]|
|trade_type|string|否|1存入,2取出,3兑换｜
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|data|[]item|数据列表|

item数据结构  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|title|string|交易title: 存入，取出｜
|symbol|string|交易对：格式：ETH－HOO|
|wallet_id|string|钱包ID|
|base_name|string|币种1名称|
|token_name|string|币种2名称|
|base_amount|string|币种1金额|
|token_amount|string|币种2金额|
|value|string|交易金额,若trade_type=3，则为base_value+token_value|
|create_at|string|交易时间|

```json
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


### [<font id=ex color=blue>池子信息</font>](#ex)
    POST /api/open/pool/pools
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|name|string|否|币种名称｜
|pagenum|string|否|页码｜
|pagesize|string|否|每页数据量，范围[5:100]|
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|data|[]item|数据列表|

item数据结构  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|pool_id|string|池子id｜
|name|string|币种名称｜
|logo1|string|币种1 logo｜
|logo2|string|币种2 logo｜
|multiple|string|挖矿倍数｜
|pledge_type|string|质押类型|
|year_rate|string|年化利率|
|is_swap|string|是否Swap做市|
|symbol|string|交易对：格式：ETH－HOO|
|start_at|string|开始时间|
|stop_at|string|结束时间|
|mining_coins|[]string|挖矿币种|

```json
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
        "logo1": "https://wallet.test.hoogeek.com/media/news_thumb/16064823003736422_5.png",
        "symbol": "BTC",
        "year_rate": "0",
        "is_swap": false,
        "pool_id": 28
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
        "logo2": "https://wallet.test.hoogeek.com/media/news_thumb/16064826670180166_17.png",
        "logo1": "https://wallet.test.hoogeek.com/media/news_thumb/16064826636837983_12.png",
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

### [<font id=ex color=blue>根据池子ID获取池子信息</font>](#ex)
    POST /api/open/pool
请求参数  
| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|pool_id|string|是|池子ID｜
|sign|string|是|签名|

返回数据  
| 参数名 | 参数类型 | 描述 |
|:---:|:---:|:---:|
|pool_id|string|池子id｜
|name|string|币种名称｜
|logo1|string|币种1 logo｜
|logo2|string|币种2 logo｜
|multiple|string|挖矿倍数｜
|pledge_type|string|质押类型|
|year_rate|string|年化利率|
|is_swap|string|是否Swap做市|
|symbol|string|交易对：格式：ETH－HOO|
|start_at|string|开始时间|
|stop_at|string|结束时间|
|mining_coins|[]string|挖矿币种|

```json
{
  "data": {
    "name": "EOS",
    "symbol": "EOS",
    "multiple": "3",
    "year_rate": "0",
    "pool_id": 39,
    "stop_at": 1608868739,
    "logo1": "https://wallet.test.hoogeek.com/media/news_thumb/16088677054857416_19.png",
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
