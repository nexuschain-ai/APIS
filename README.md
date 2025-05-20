# APIS
Nexus_API
Nexus Chain API 文档概览
文档标题
Nexus Chain API 开发者文档 (2025年5月版)

内容简介
本文档提供了Nexus区块链平台的完整API参考，包含以下核心模块：

主要功能模块
账号管理：查询余额、交易记录、内部交易等

智能合约：获取ABI/源代码、验证合约

交易查询：检查交易状态和执行结果

区块数据：获取区块信息、奖励详情

事件日志：查询合约事件日志

关键特性
统一参数规范：所有API共享module/action/apikey等基础参数

数据单位：余额/奖励等值默认使用wei单位

错误处理：标准化错误码(isError: 0/1)

分页支持：默认返回最多10,000条记录

开发者工具
代理API：直接访问底层节点(Geth/Parity)

SDK参考：包含Solidity合约开发规范

验证服务：支持合约源代码验证(BETA)

实用信息
日期标记：本文档对应2025年5月API版本

速率限制：通过API Key进行请求管理

特殊说明：包含中英文术语对照说明

提示：各模块API的参数说明在文档"Nexus API约定"部分统一描述，具体接口文档中不再重复。

API文档
参数
参数说明：
module: 指明接口所属模块，即上面包含的模块
action: API动作，如：txlist - 表示列出交易记录；
address: 所查询交易的账号地址；
contractaddress: 合约地址
apikey: 用户API-key 根据key来统计请求限额；
startblock: 起始查询块 id，可选，默认值为 0；
endblock: 结束查询块 id，可选，默认值为最后一个区块；
tag: 状态：pending 或 latest
blocktype: 块类型：blocks（主链块） 或 uncles （叔块）
page: 页码，可选；
offset: 每页查询记录数，可选，默认是查询 10000 条记录；
sort: 排序规则，支持正序asc和倒序desc。
Nexus API 约定
Nexus API文档时间为2025年5月，因官方API没有版本号，这里用时间做一个标注。
包含模块
Nexus API主要包含模块有：
账号地址相关接口
智能合约相关接口
交易相关接口
区块相关接口
事件日志相关接口

这些模块对应着左侧的一级菜单，在接口中使用module参数指定
参数
参数说明：
module: 指明接口所属模块，即上面包含的模块
action: API动作，如：txlist - 表示列出交易记录；
address: 所查询交易的账号地址；
contractaddress: 合约地址
apikey: 用户API-key 根据key来统计请求限额；
startblock: 起始查询块 id，可选，默认值为 0；
endblock: 结束查询块 id，可选，默认值为最后一个区块；
tag: 状态：pending 或 latest
blocktype: 块类型：blocks（主链块） 或 uncles （叔块）
page: 页码，可选；
offset: 每页查询记录数，可选，默认是查询 10000 条记录；
sort: 排序规则，支持正序asc和倒序desc。
module、action、apikey是每个 API 都有的参数，其他的参数则因不同 API 而不同，这里做一个统一的介绍，后面介绍接口时不再单独说明。

账号(Account)
账号及地址相关的 API，接口的参数说明请参考Nexus API 约定, 文档中不单独说明。
获取单个账号余额
注解
译者注:
英文 balance 有人翻译为`金额`，译者习惯称为`余额`。 账号和地址大部分也是指一个意思。

接口:

https://api.nexuschain.ai/accountBalance/api?module=account&action=balance&address=0x&tag=latest&apikey=YourApiKeyToken

返回:

{
status: "1",
message: "OK",
result: "40807178566070000000000"
}

说明:
余额的单位都是最小单位wei， 更多单位换算可阅读：Nexus单位换算


获取地址(普通)交易列表
接口：

https://api.nexuschain.ai/transactionList/api?module=account&action=txlist&address=&apikey=YourApiKeyToken

可选参数：startblock 、endblock、sort

返回：

{
"status": "1",
"message": "OK",
"result": [{
"blockNumber": "47884",
"timeStamp": "1438947953",
"hash": "0xad1c27dd8d0329dbc400021d7477b34ac41e84365bd54b45a4019a15deb10c0d",
"nonce": "0",
"blockHash": "0xf2988b9870e092f2898662ccdbc06e0e320a08139e9c6be98d0ce372f8611f22",
"transactionIndex": "0",
"from": "0xddbd2b932c763ba5b1b7ae3b362eac3e8d40121a",
"to": "0x2910543af39aba0cd09dbb2d50200b3e800a63d2",
"value": "5000000000000000000",
"gas": "23000",
"gasPrice": "400000000000",
"isError": "0",
"txreceipt_status": "",
"input": "0x454e34354139455138",
"contractAddress": "",
"cumulativeGasUsed": "21612",
"gasUsed": "21612",
"confirmations": "7525550"
}]
}

说明:

isError： 0= 没错, 1=出错 最多返回最近的10000个交易Nexus Api约定
获取地址”内部”交易列表
内部交易只指引一个正常（外部）交易（连带）触发的交易，比如过一个合约调用触发的内部交易。
接口：

https://api.nexuschain.ai/Internal/transaclist/api?module=account&action=txlistinternal&address=

可选参数：startblock 、endblock、sort
返回结果和普通交易列表JSON结构一样。

获取地址转账交易列表
转账交易是通过Transfer事件来判定的。
接口：

https://api.nexuschain.ai/interna/ltransactionlist/api?module=account&action=tokentx&address=&contractaddress

说明： 可选参数：startblock 、endblock、contractaddress 、sort 可以通过参数contractaddress来指定某一个合约的交易。
返回和上面普通交易列表JSON结构一样。

获取地址所挖区块列表
接口：

https://api.nexuschain.ai/block/transactionlist/api?module=account&action=getminedblocks&address=&blocktype=blocks

说明：blocktype 值为 blocks（仅显示主链块） 或 uncles （仅显示叔块） ， blocks为默认值
返回：
{
status: "1",
message: "OK",
result: [
{
blockNumber: "3462296",
timeStamp: "1491118514",
blockReward: "5194770940000000000"
},
{
blockNumber: "2691400",
timeStamp: "1480072029",
blockReward: "5086562212310617100"
}
]
}

}

请下载Nexus_APIS.docx文档了解更多Nexus API 文档
