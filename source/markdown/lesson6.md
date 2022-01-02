# Solidity 介绍（三）

上一章节， 我们了解solidity 的数据结构类型， 本章我们的重点用于介绍变量和函数。

## var
var 是一种只能在函数中声明的特殊类型， 状态变量不能是var 类型。 var 代表类型没有任何类型， 在编译器编译的时候， 确定变量的类型， 确定之后，无法更改。

```

function newStartUp(){
  var uint = 100;
  var isCreated = true;
  var address = "0x0";

}

```

solidity 中， 与javascript 类似， 都会有变量声明提前， 变量声明可以再函数内的任意地方， 及时是在使用之后声明也可以。 solidity 的编译器会提取函数内变量声明的位置， 然后讲他们放置的函数的顶部。

```

function testVar() {
  startUpName = "test";
  startupDesc = "test desc"

  String startUpName;
  String startUpDesc;
}

```

## 作用域
变量作用域有两种：
1. 合约的全局变量（状态变量）
2. 函数的局部变量

全局变量可以用于所有函数中，状态变量的修改只能通过函数
1. public 修饰的状态变量，可以直接从外部调用
2. internal 修饰的状态变量， 不能直接从外部访问， 但是能在当前合约或者子合约中访问
3. private 只能在当前合约中访问

```
contract Startup {
  String public startUpName = "";
  String private  DiscoName = "";
  String internal IROName = "";

}

```

## 全局变量
Solidity 提供了很多全局变量的访问权限， 尽管变量没有在合约中声明。 但是确能真切的在合约中通过代码访问

### 全局变量的定义

```
contract StartUp {
  event logstring(string);
  event loguint(uint);

  function createStartup() payable {
    logstring(block.coinbase);
    loguint(block.gas);
  }

}
.
```

block 可以查看的数据信息(部分)


1. block.coinbase: 矿工地址
2. block.gas： 当前区块的gas费用
3. block.timeStamp： 创建区块的时间
4. msg.data： 与创建交易相关的数据
5. msg.sender： 函数的调用者
6. now： 当前时间
7. tx.origin： 交易的发起者


### 全局变量的加密

solidity 为合约提供了变量的加密， 分别为sha3和sha2, sha3是基于sha3（keccak256）函数， sha2 是基于sha256函数，

```
function getStartups() returns(bytes32 x ,bytes32 y , bytes32 z) {
  return (sha256(x), keccak256(y), sha3(z))
}

```

### 全局变量的地址
每个合约地址， 都会有五个全局函数和一个全局变量：
1. address.transfer(uint256 amount) 向address发送amount wei 的以太币， 执行失败， 返回false
2. address.sned(uint256 amount ) 向 address 发送 amount wei 的以太币
3. address.call 执行获取状态变量的函数
4. address.callcode 调用 低级别的callcode函数
5. address.delegatecall 调用低级别的delegatecall 函数

### 合约变量

每个合约都有三个全局要素
1. this 当前的合约类型
2. selfdestruct 接收一个地址， 销毁当前合约， 并且将合约中的资产， 发送到指定的地址
3. suicide: 是 selfdestruct 的别名

