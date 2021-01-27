Hoo开放API接口说明
---------

1. 接口仅支持GET和POST请求
2. 所有请求参数按照按字符顺序进行排序后，以「key=value&key1=value」的方式组合成字符串然后进行加密，将加密后的十六进制字符串放入参数sign中一并传入
3. 加密方式为：HMAC-SHA256，测试key：12345，加密结果以十六进制表示，参考python示例
4. 接口统一返回格式：{"code": "10000", "message": "success", "data": {}}，code=10000时接口成功，其他为接口错误
5. 接口有IP地址限制，接口接入前须提供接入的IP地址

```
    签名示例(Python)：
    import hmac
    import hashlib

    sign_str = "key=value&key1=value1"
    secret_key = "12345"
    sign = hmac.new(secret_key.encode("utf-8"), sign_str.encode("utf-8"), digestmod=hashlib.sha256).hexdigest()
```

### [<font id=ex color=blue>兑换</font>](#ex)
    POST /api/open/swap/ex

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|send_coin|string|是|发起兑换的币种名称|
|send_volume|decimal|是|发起兑换的币种数量|

返回
```
{
    "message":"success",
    "code":"10000",
    "data":{}
}
```
### [<font id=reg color=blue>获取池子数量</font>](#reg)
    POST /api/open/swap/poolvolume

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|

返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|base_volume|decimal|是|交易币种(eg. ETN-USDT中的ETH)数量|
|quote_volume|decimal|是|计价币种(eg. ETH-USDT中的USDT)数量|

返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|percent|int|是|取出的lp百分比，整数：0 < percent <= 100|

返回
```
{
    "message":"success",
    "code":"10000",
    "data":{}
}
```

### [<font id=ex color=blue>获取可提取资产（我的做市）</font>](#ex)
    POST /api/open/swap/theoryassets

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|否|交易对：格式：ETH-USDT|

返回
```
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
    GET /api/open/swap/lp

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|否|交易对：格式：ETH-USDT|
|coin_name|string|否|币种：格式：ETH|

返回
```
{
    "message":"success",
    "code":"10000",
    "data":{
        "amount":"28.2",
        "freeze":"13.02"
    }
}
```

### [<font id=ex color=blue>抵押LP</font>](#ex)
    POST /api/open/hoopool/stakelp

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|lp_amount|decimal|是|需要抵押的LP的数量|

返回
```
{
    "message":"success",
    "code":"10000",
    "data":{}
}
```

### [<font id=ex color=blue>赎回LP</font>](#ex)
    GET /api/open/hoopool/retrievelp

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|lp_amount|decimal|是|需要赎回的LP的数量|

返回
```
{
    "message":"success",
    "code":"10000",
    "data":{}
}
```

### [<font id=ex color=blue>交易对历史价格</font>](#ex)
    POST /api/open/swap/price/history

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|symbol|string|是|交易对：格式：ETH-USDT|
|date_begin|string|是|起始日期：格式：YYYYMMDD，如20201028|
|date_end|string|是|起始日期：格式：YYYYMMDD，如20201230, 且date_end须比date_begin大|
|page|string|否|页码，从0开始|
|page_size|string|否|每页数据条数,默认值为100|
|sign|string|是|签名|

返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|base_name|string|是|交易对：格式：ETH|
|token_name|string|是|交易对：格式：USDT|
|sign|string|是|签名|

返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|sign|string|是|签名|
|symbol|string|是|交易对：格式：ETH－HOO|


返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|sign|string|是|签名|
|symbol|string|是|交易对：格式：ETH－HOO|

返回
```
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

| 参数名 | 参数类型 | 是否必须 | 描述 |
|:---:|:---:|:---:|:---:|
|client_id|string|是|Hoo提供给用户的接入ID|
|sign|string|是|签名|
|pagenum|string|否|页码｜
|pagesize|string|否|每页数据量，范围[5:100]|

返回
```
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
      ...
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
