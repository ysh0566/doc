## JSON-RPC methods

-  [Gzv_tx](#tx)
- [Gzv_nonce](#nonce)



### <span id="tx">Gzv_tx</span>

发送交易

#### Parameters

1. ` Object ` -transaction交易体，json格式

   - `source`: 发送交易源地址

   - `target`: 接收地址
   - `value`:发送的金额, Ra单位的数量
   - `gaslimit`: 该交易可消耗gas
   - `gas_price`: gas价格
   - `type`: 交易类型，普通交易为0
   - `nonce`: source 账户的nonce
   - `data`: 普通交易可不传该参数，参数类型为base64格式的字符串
   - `sign`: 签名, hex格式
   - `extra_data`: 可选备注， 参数类型为base64格式的字符串

#### Example Parameters

```json
params: [{
    "source": "zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e",
    "target": "zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e", 
    "value": 1000000000, 
    "gas_limit": 1000,  
    "gas_price": 500,
    "type": 0,  
    "nonce": 10, 
    "data": "", 
    "sign": "0x32f4b12bfba23fbe64043becc239184f7aeccbc815f4771058907ab01379062f51f580ea494d5d70e3ae3326fc5dc90946e9629689dddab6ced86deaf3b911ea02", 
    "extra_data": ""  
}]
```

#### Returns

`Hash` - 所发送交易的hash

#### Example

```python
# Request
import json
import requests

url = 'http://node1.zvchain.io:8101/'

transaction = {
    "source": "zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e", 
    "target": "zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e", 
    "value": 1000000000,  
    "gas_limit": 1000,  
    "gas_price": 500, 
    "type": 0,  
    "nonce": 10, 
    "data": "",  
    "sign": "0x32f4b12bfba23fbe04043becc339184f7aeccbc805f4771058907ab01379062f51f580ea494d5d70e3ae3326fc5dc90946e9d29689dddab6ced76deaf3b911ea02",  # 签名, hex格式
    "extra_data": "" 
}

payload = {
    "method": "Gzv_tx",
    "params": [
        transaction
    ],
    "jsonrpc": "2.0",
    "id": 1
}
headers = {
  'Content-Type': 'application/json'
}
response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

```json
// Response
{
    "jsonrpc":"2.0",
    "id":"1",
    "result":"0xf86fcfd8c1fb2572d831fc50e88da87311d1a197c6eb6133e7eddcc788f06e0d"
}
```



### <span id="nonce">Gzv_nonce</span>

获取指定账户的nonce

####Parameters

1. `Address`-查询的地址

#### Example Parameters

```json
"params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf"
 ]
```

#### Returns

`uint`-指定账户的nonce值

 #### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_nonce",
    "params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf"
    ],
    "jsonrpc": "2.0",
    "id": 1
}
headers = {
  'Content-Type': 'application/json'
}
response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())

```

```json
// Response
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": 1 
}
```



### Gzv_txReceipt

查询交易回执

#### Parameters

`Hash`: 交易hash

#### Example Parameters

```
"params": [
        "0xf86fcfd8c1fb2572d831fc50e88da87311d1a197c6eb6133e7eddcc788f06e0d"
 ]
```

#### Returns

`Object`-回执和交易数据

- `Receipt`
  - `status`-回执状态码
  - `cumulativeGasUsed`-交易gas消耗
  - `logs`-合约产生的log
  - `transactionHash`-交易hash
  - `contractAddress`-如果交易类型为创建合约，显示创建的合约地址
  - `height`-交易所在块高
  - `tx_index`-交易在块中的索引
- `Transaction`
  - `data`-交易data数据
  - `value`-交易的转账数额
  - `nonce`-交易的nonce
  - `source`-发送交易源地址
  - `target`-交易接收地址
  - `type`-交易类型
  - `gas_limit`-交易可消耗gas
  - `gas_price`-gas价格
  - `hash`-交易hash
  - `extra_data`-备注信息

#### Example

```python
# Request
import json
import requests

url = 'http://120.77.155.204:8102/'

payload = {
    "method": "Gzv_txReceipt",
    "params": [
        "0xf86fcfd8c1fb2572d831fc50e88da87311d1a197c6eb6133e7eddcc788f06e0d"
    ],
    "jsonrpc": "2.0",
    "id": 1
}
headers = {
  'Content-Type': 'application/json'
}
response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

```json

// Response
/*
回执状态码
RSSuccess ReceiptStatus = 0 // 交易成功
RSFail= 1                   // 交易失败
RSBalanceNotEnough= 2				// 余额不足
RSAbiError= 3								// 执行合约abi校验失败
RSTvmError= 4								// 执行合约失败
RSGasNotEnoughError= 5			// gas不足
RSNoCodeError= 6						// 请求地址无合约代码
RSParseFail= 7							// 解析错误
*/
{
    "jsonrpc":"2.0",
    "id":"1",
    "result":{
        "Receipt":{
            "status":0, 
            "cumulativeGasUsed":400, 
            "logs":null,  
            "transactionHash":"0xf86fcfd8c1fb2572d831fc50e88da87311d1a197c6eb6133e7eddcc788f06e0d",
            "contractAddress":"zv0000000000000000000000000000000000000000000000000000000000000000",
            "height":6830, 
            "tx_index":1
        },
        "Transaction":{
            "data":null,
            "value":1,
            "nonce":7,
            "source":"zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e",
            "target":"zvd4d108ca92871ab1115439db841553a4ec3c8eddd955ea6df299467fbfd0415e",
            "type":0,
            "gas_limit":400,
            "gas_price":500,
            "hash":"0xf86fcfd8c1fb2572d831fc50e88da87311d1a197c6eb6133e7eddcc788f06e0d",
            "extra_data":""
        }
    }
}
```



