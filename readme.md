# 合约结构

### 合约实体：


总共1个合约文件，需要编译部署其中的MyToken。使用CFO的私钥账户部署合约，部署后记下得到的合约地址。

### 如何部署：

使用[Remix - Solidity IDE](http://sol.51xnsd.com/#optimize=false&version=soljson-v0.4.19+commit.c4cbbb05.js)在线编译，将sol文件内容拷贝到文本框中，右侧compile tab页下选择所需编译的合约，点击“Details”, 在弹出的页面中拷贝“WEB3DEPLOY”里的内容。如：

```
var supply = /* var of type uint256 here */ ;

var mytokenContract = web3.eth.contract([{"constant":false,"inputs":[{"name":"guy","type":"address"},{"name":"wad","type":"uint256"}],"name":"approve","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"src","type":"address"},{"name":"dst","type":"address"},{"name":"wad","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"src","type":"address"}],"name":"balanceOf","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"dst","type":"address"},{"name":"wad","type":"uint256"}],"name":"transfer","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"src","type":"address"},{"name":"guy","type":"address"}],"name":"allowance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[{"name":"supply","type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"name":"from","type":"address"},{"indexed":true,"name":"to","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"owner","type":"address"},{"indexed":true,"name":"spender","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Approval","type":"event"}]);

var mytoken = mytokenContract.new(
   supply,
   {
     from: web3.eth.accounts[0], 
     data: '0x6060604052341561000f57600080fd5b604051602080610a7f83398.....a7817a22cbc13a42e0c218a884b44ccd053977780029', 
     gas: '4700000'
   }, function (e, contract){
    console.log(e, contract);
    if (typeof contract.address !== 'undefined') {
         console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
    }
 })

```

将supply值设定为发行Token的总量。

进入Geth 命令行，解锁账户，开启挖矿，部署合约。如：
```
geth --datadir "~/eth/erc20" --verbosity "6" --networkid "345235" --ipcpath "~/eth/erc20/geth.ipc" --rpc --rpcapi "db,eth,net,personal,web3" --port 4146 --rpcport 8101 --gasprice "0" --nodiscover console 2>> ~/eth/erc20/01.log

personal.unlockAccount(eth.accounts[0])

miner.start(2);

```

部署合约即把上述“WEB3DEPLOY”的内容黏贴到console中并回车,部署成功后得到：

```

> null [object Object]
Contract mined! address: 0x25cbcc6852397a25d32f5c543ef8347468d9d663 transactionHash: 0x9c597cbe70adccee96e8492429347f25b7519a6a57a0f191f9f33a5f277ca075

```


# Test Plan

> 以下测试步骤基于geth console环境进行

## 部署合约并设定代币总量

![](http://chuantu.biz/t6/208/1516193324x-1404793495.jpg)
![](http://chuantu.biz/t6/208/1516193418x-1404793495.jpg)

## 查询某账户的代币余额

![](http://chuantu.biz/t6/208/1516193521x-1404793495.jpg)

## 转账给某账户100个代币

![](http://chuantu.biz/t6/208/1516193711x-1404793495.jpg)

## A抵押100个代币到某账户B（该账户B有这些代币的处置权）

![](http://chuantu.biz/t6/208/1516193902x-1404793495.jpg)

## 被抵押账户B划转100个Token到账户C

![](http://chuantu.biz/t6/208/1516194119x-1404793495.jpg)
