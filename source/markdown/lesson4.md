# Solidity 介绍（一）

前面三章，我们介绍了区块链和以太坊等。 本章开始， 我们正式开始学习solidity 语言。

## Solidity  与 EVM

Solidity 是针对以太坊虚拟机EVM 的编程语言， 以太坊通过编写智能合约的代码来扩展其功能。EVM 执行智能合约的代码， 智能合约是由Solidity语言编写的。 可是 EVM并不能直接理解solidity, 智能理解字节码。因此， solidity 到 EVM 可以识别的字节码， 需要经过solidity 附带的编译器， 称为sol, 它负责完成转换。

solidity 是一种非常接近javaScript 的语言， 只支持有限的面向对象的特征， solidity 的文件结构主要由四个部分组成：

1. 预编译指令
2. 注释
3. 导入
4. 合约

## 预编译指令

预编译指令是sol 文件的第一行， pragma 是告诉编译器， 用哪个编译器版本来编译。 主要原因是因为solidity 作为一门新的语言， 一直在持续的改进中， 每当引入新的特性的时候， 就会推出新版本的编译器

```solidity
pragma solidity >=0.8.x <0.9.0;
```

虽然编译器的版本指定不是强制的， 但是 pragma 作为第一行是值得鼓励的一种做法。

在指定版本的过程中， 我们经常会看到 ^ 来指定版本， 例如

```solidity
pragma solidity ^0.8.x
```

这种做法会导致当编译器版本有新的改动的时候， 可能编译器就会弃用你的代码。从而导致在某一天早上当你上班编译solidity 的时候， 发现跟昨天的结果不一样，从而调试一上午， 才发现是版本问题。

## Solidity 合约

### 注释

solidity 的注释包括单行或者多行， 单行使用双斜线

```solidity
// 创建staretup
function newStartup(Profile memory p) public payable {}
```

多行使用 /*  */

```solidity
/* 创建
   staretup
*/
function newStartup(Profile memory p) public payable {}
```

### 合约文件引用

合约中， 与其它编程语言一样， 文件间或库的引用。

文件间的引用一般使用import, 文件路径是完全显示或者隐式的路径，与linux 的文件引入方式完全一致，

```
import filename；
```



### 合约的结构

合约的常规结构主要包括 函数和变量。以下是一个完整的base 合约,  大家暂时不必关注具体的逻辑

```solidity
// write by ys 神 from  comunion contract team
pragma solidity >=0.8.x <0.9.0;

contract Base
{
    address internal  _owner;
    address payable internal  _coinbase;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier isOwner() {
        assert(msg.sender == _owner);
        _;
    }

    constructor()
    {
        _owner = msg.sender;
    }

    fallback() external virtual payable {
        revert();
    }

    receive() external payable {
        revert();
    }

    function setCoinBase(address payable cb) internal isOwner {
        _coinbase = cb;
    }

    function transferOwnership(address newOwner) public virtual isOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function suicide0(address payable receiver)
    public
    isOwner {
        selfdestruct(receiver);
    }
```

合约结构主要有

1. 编译器声明
2. contract 声明合约
3. 状态变量： 合约中的状态变量例如 _owner， 没有在任何函数内声明的变量都是状态变量
4. 修改器
5. 函数

### 合约的类型

任何语言中都有数据类型， solidity 也不例外， 主要有：

1. bool

2. uint/int

3. bytes

4. address

5. mapping

6. enum

7. struct

   struct 主要是方便用户实现自定义数据或符合类型数据，但是不包含任何代码， 只包含变量。你想要将不同的数据类型关联到一起， 便可以使用struct, 例如：

   ```
    struct Profile {
           string id;
           string name;
           Mode mode;
           string hashtag;
           bytes logo;
           string mission;
           string overview;
       }
   ```



8. bytes/String

数据类型的主要修饰符有四种

1. internal: 被修饰的变量或者函数只能在当前合约或者集成的子类合约中使用
2. private：被修饰的变量或者函数 只能在其声明的合约中使用
3. public ：被修饰的变量或者函数可以公开访问
4. constant：状态变量不可变， 且必须赋初值

### 合约的修改器

在以上的合约中， 我们可以可以看到

```
modifier isOwner() {
        assert(msg.sender == _owner);
        _;
    }
```

用modifier修饰的函数， 修改器只与函数关联， 用于改变执行代码的行为。就写我们银行转账，转账前我们需要判断当前账户是否是有效的， 那么就可以使用 moddifier 修饰 转账函数。

我们看到 修改器修饰的函数中有一个 “_“, 其含义是经过了 assert 的判断之后， 继续执行目标函数的逻辑。











