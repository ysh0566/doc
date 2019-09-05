## <span id="top">JSON-RPC methods</span>

- [Gzv_tx](#tx)
- [Gzv_nonce](#nonce)
- [Gzv_txReceipt](#txReceipt)
- [Gzv_balance](#balance)
- [Gzv_groupHeight](#groupHeight)
- [Gzv_getBlockByHeight](#getBlockByHeight)
- [Gzv_getBlockByHash](#getBlockByHash)
- [Gzv_getTxsByBlockHeight](#getTxsByBlockHeight)
- [Gzv_getTxsByBlockHash](#getTxsByBlockHash)
- [Gzv_minerPoolInfo](#minerPoolInfo)
- [Gzv_minerInfo](#minerInfo)
- [Gzv_transDetail](#transDetail)
- [Gzv_queryAccountData](#queryAccountData)



### <span id="tx">Gzv_tx </span> [⇧](#top)

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



### <span id="nonce">Gzv_nonce</span> [⇧](#top)

获取指定账户的nonce

#### Parameters

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



### <span id="txReceipt">Gzv_txReceipt [⇧](#top)</span>

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



### <span id="balance">Gzv_balance [⇧](#top)</span>

获取指定账户的余额

#### Parameters

1. `Address`-查询的地址

#### Example Parameters

```json
"params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf"
 ]
```

#### Returns

`float`-指定账户的余额(单位：ZVC)

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_balance",
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
    "result": 1.1 
}
```

### <span id="blockHeight">Gzv_blockHeight [⇧](#top)</span>

获取节点当前同步块高

#### Parameters

`none`

#### Example Parameters

```json
"params": [
 ]
```

#### Returns

`uint`-节点当前同步块高

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_blockHeight",
    "params": [
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
    "result": 1024 
}
```

### <span id="groupHeight">Gzv_groupHeight [⇧](#top)</span>

获取节点当前同步组高

#### Parameters

`none`

#### Example Parameters

```json
"params": [
 ]
```

#### Returns

`uint`-节点当前同步组高

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_groupHeight",
    "params": [
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
    "result": 1024
}
```

### <span id="getBlockByHeight">Gzv_getBlockByHeight [⇧](#top)</span>

获取指定块高查询块信息

#### Parameters

1. `uint`-查询的块高

#### Example Parameters

```json
"params": [
        1024
 ]
```

#### Returns

`Obejct`-块信息

- `height`
- `hash`
- `pre_hash`
- `cur_time`
- `pre_time`
- `castor`
- `group_id`
- `prove`
- `total_qn`
- `qn`
- `txs`
- `state_root`
- `tx_root`
- `receipt_root`
- `prove_root`
- `random`

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_getBlockByHeight",
    "params": [
        1024
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
    "result": {
        "height": 1024,
        "hash": "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d",
        "pre_hash": "0x63effc19ac273e3cccfd84e26cae531eddf0197258f37d25ee3e9d92ff979899",
        "cur_time": "2019-09-03T16:25:10.763+08:00",
        "pre_time": "2019-09-03T16:25:07.763+08:00",
        "castor": "zv938407bee35f42fe07bca44a0463088bbfbf19d821212ad4b2a1da284127d79a",
        "group_id": "0x6861736820666f72207a76636861696e27732067656e657369732067726f7570",
        "prove": "0x03c4d77a9976bbcfc6af62e5abd2d129c8ca643611359795ecad3127486c1927dd5fa7498f434d3262253961df4a5845670321f18a1bc68709182b28054034bf201d635177777fd78db80fec91aeb22527",
        "total_qn": 4984,
        "qn": 5,
        "txs": 0,
        "state_root": "0xb4b4ed55abcb67ac96b5e051575a5e038ab6db16ef0a2ed9f425f716b025acb3",
        "tx_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "receipt_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "prove_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "random": "0x08fe8d72bfddd1a52173671c3a0c95e8c93564e694723a48fd0004046866d61400"
    }
}
```

### <span id="getBlockByHash">Gzv_getBlockByHash [⇧](#top)</span>

获取指定账户的余额

#### Parameters

1. `Hash`-查询的块的hash值

#### Example Parameters

```json
"params": [
        "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
 ]
```

#### Returns

`Obejct`-块信息

- `height`
- `hash`
- `pre_hash`
- `cur_time`
- `pre_time`
- `castor`
- `group_id`
- `prove`
- `total_qn`
- `qn`
- `txs`
- `state_root`
- `tx_root`
- `receipt_root`
- `prove_root`
- `random`

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_getBlockByHash",
    "params": [
        "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
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
    "result": {
        "height": 1024,
        "hash": "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d",
        "pre_hash": "0x63effc19ac273e3cccfd84e26cae531eddf0197258f37d25ee3e9d92ff979899",
        "cur_time": "2019-09-03T16:25:10.763+08:00",
        "pre_time": "2019-09-03T16:25:07.763+08:00",
        "castor": "zv938407bee35f42fe07bca44a0463088bbfbf19d821212ad4b2a1da284127d79a",
        "group_id": "0x6861736820666f72207a76636861696e27732067656e657369732067726f7570",
        "prove": "0x03c4d77a9976bbcfc6af62e5abd2d129c8ca643611359795ecad3127486c1927dd5fa7498f434d3262253961df4a5845670321f18a1bc68709182b28054034bf201d635177777fd78db80fec91aeb22527",
        "total_qn": 4984,
        "qn": 5,
        "txs": 0,
        "state_root": "0xb4b4ed55abcb67ac96b5e051575a5e038ab6db16ef0a2ed9f425f716b025acb3",
        "tx_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "receipt_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "prove_root": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "random": "0x08fe8d72bfddd1a52173671c3a0c95e8c93564e694723a48fd0004046866d61400"
    }
}
```

### <span id="getTxsByBlockHash">Gzv_getTxsByBlockHash [⇧](#top)</span>

获取指定块的所有交易Hash

#### Parameters

1. `Hash`-查询的块的hash值

#### Example Parameters

```json
"params": [
        "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
 ]
```

#### Returns

`Array<Hash>`-交易Hash

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_getTxsByBlockHash",
    "params": [
        "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
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
    "result": [
      "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
    ]
}
```

### <span id="getTxsByBlockHeight">Gzv_getTxsByBlockHeight [⇧](#top)</span>

获取指定块的所有交易Hash

#### Parameters

1. `uint`-查询的块的高度

#### Example Parameters

```json
"params": [
        1024
 ]
```

#### Returns

`Array<Hash>`-交易Hash

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_getTxsByBlockHeight",
    "params": [
        1024
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
    "result": [
      "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
    ]
}
```

### <span id="minerPoolInfo">Gzv_minerPoolInfo [⇧](#top)</span>

查询某高度的矿工信息

#### Parameters

1. `Address`-查询的地址
2. `uint`-查询高度

#### Example Parameters

```json
"params": [
        "zv61dab936abb18d68c32fab6ed9b0eeb42d80d3e2b982a2e765864e73588f7805", 100
 ]
```

#### Returns

`Object`-矿工信息

- `current_stake`
- `full_stake`
- `tickets`
- `identity`
- `valid_tickets`

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_minerPoolInfo",
    "params": [
        "zv61dab936abb18d68c32fab6ed9b0eeb42d80d3e2b982a2e765864e73588f7805", 100
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
    "result": {
        "current_stake": 1000000000000,
        "full_stake": 0,
        "tickets": 0,
        "identity": 0,
        "valid_tickets": 8
    }
}
```

### <span id="minerInfo">Gzv_minerInfo</span> [⇧](#top)

获取当前矿工质押信息

#### Parameters

1. `Address`-查询的被质押的地址
2. `Address|none`-质押的地址，空字符串表示被质押的地址

#### Example Parameters

```json
"params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf"， ""
 ]
```

#### Returns

`Object`-质押相关信息

- `overview`- 总览
  - `Array<Object>`
    - `Object`-详情
      - `stake`
      - `apply_height`
      - `type`
      - `miner_status`
      - `status_update_height`
      - `identity`
      - `identity_update_height`
- `details`- 详情
  - `Map<string, Array<Object>>`
    - Object`-详情
      - `value`
      - `update_height`
      - `m_type`
      - `stake_status`
      - `can_reduce_height`

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_minerInfo",
    "params": [
         "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf"， ""
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
    "result": {
        "overview": [
            {
                "stake": 1000,
                "apply_height": 0,
                "type": "proposal node",
                "miner_status": "normal",
                "status_update_height": 0,
                "identity": "normal node",
                "identity_update_height": 0
            },
            {
                "stake": 500,
                "apply_height": 0,
                "type": "verify node",
                "miner_status": "normal",
                "status_update_height": 0,
                "identity": "normal node",
                "identity_update_height": 0
            }
        ],
        "details": {
            "zv61dab936abb18d68c32fab6ed9b0eeb42d80d3e2b982a2e765864e73588f7805": [
                {
                    "value": 500,
                    "update_height": 0,
                    "m_type": "verifier",
                    "stake_status": "normal",
                    "can_reduce_height": 0
                },
                {
                    "value": 1000,
                    "update_height": 1,
                    "m_type": "proposal",
                    "stake_status": "normal",
                    "can_reduce_height": 0
                }
            ]
        }
    }
}
```

### <span id="transDetail">Gzv_transDetail</span> [⇧](#top)

根据交易hash查询交易详情

#### Parameters

1. `Hash`-查询的交易hash

#### Example Parameters

```json
"params": [
        "0x211e7a402a18f71bf77397db651f03b9094037de2769dce3567c991878723e4d"
 ]
```

#### Returns

`Obeject`-交易详情

- `data`- 交易的data区，base64编码
- `value`-交易金额， TAS单位
- `nonce`-交易nonce
- `source`-交易发起者
- `target`-交易接受者
- `type`-交易类型
- `gas_limit`-交易可用gas
- `gas_price`-交易gas价格
- `hash`-交易hash
- `extra_data`-交易附加内容

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_transDetail",
    "params": [
        "0x331db0f9e6ccee2dff9361cea599fbf46e528d0388c5223dc4ed7e0fd6c0323b"
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
        "data": "6sA6vI3l2d6Ymmo2rWEptqGNoJEny3D4pxmuaPNJzxc=",
        "value": 0.95706438,
        "nonce": 0,
        "source": null,
        "target": null,
        "type": 104,
        "gas_limit": 0,
        "gas_price": 0,
        "hash": "0x331db0f9e6ccee2dff9361cea599fbf46e528d0388c5223dc4ed7e0fd6c0323b",
        "extra_data": "\u0001\u007f\ufffd\u0002\u0002\ufffd/@\ufffd\u000b\ufffdaw\ufffd\u0002%\ufffd}\ufffd8\"\ufffd%\u0016\f\ufffdj\ufffd\ufffd\ufffd\ufffd\ufffd\u0019\u0000\u0000\u0000\u0001VE\ufffdm\u0000\u0000\u0000\u0001\u0000\u0003\u0000\u0005\u0000\u0007\u0000\t"
    }
```

### <span id="queryAccountData">Gzv_queryAccountData [⇧](#top)</span>

获取合约存储区的数据

#### Parameters

1. `Address`-查询的地址
2. `string`-查询的起始key
3. `uint`-查询数量

#### Example Parameters

```json
"params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf", "key", 1
 ]
```

#### Returns

`Array<any>`-获取的value值列表

#### Example

```python
# Request
import json
import requests
url = 'http://node1.zvchain.io:8101/'
payload = {
    "method": "Gzv_balance",
    "params": [
        "zvb5f5758ba45ca7db85ad405d241641da867384a3eefeb80973d3ddc51e176acf", "key", 1
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
    "result": [10] 
}
```

