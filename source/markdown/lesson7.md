# 智能合约编写前的准备

从第二章之后， 我们经过了这么久， 才开始书写合约， 但是在书写合约前， 我们需要了解一些前置知识。

## 智能合约

智能合约的本质是EVM中部署和执行一段代码程序。 合约是借用了法律界的术语， 但是Solidity 写一份智能合约， 并不代表这段代码就是一份合同。 solidity 是实现合约的一种方式， 就像 java 和 C一样， 合约也包含状态变量和函数。

## 创建合约

一个简单的合约用contract 来声明
```
contract Startup {
  int public startUpId;
  string startUpAddress;
  struct startStyle {
    string name;
    unt age;
    bool isFinished;
  }

  modifier onlyBy(){
    if(msg.sender === _owner) {
      _
    }
  }

  enum gender {
    male,
    female
  }
}


```

这是合约的基础声明， 在这个结构体中包含状态变量， 结构和枚举， 函数， 修改器和事件.

创建合约的方式也有两种：
1. 使用new
2. 使用已经部署的合约地址， 所以合约地址不止有合约地址的功能， 还是一份合约实例

以下定义了两份智能合约， disco 和 fund, 在disco 中创建 fund 合约， 必须使用关键字

```

pragma solidity 0.5.18

contract Disco {
  function newDisco(DiscoInfo memory d) public payable {
    Fund addr = new Fund();

  }
}

contract Fund {

}


```

如果想要使用合约地址类创建合约， 这种是必须有现有的合约地址。没有创建新的实例， 只是合约的实例被复用
```

pragma solidity 0.5.18

contract Fund {

}

```


部署完fund 后，我们会获取到fund的合约地址
```
pragma solidity 0.5.18

contract Disco {
  function newDisco(DiscoInfo memory d) public payable {
    Fund addr =  Fund(funcAddress);

  }
}
```

disco 和 fund 是一个组合合约。 fund 是一个独立合约， disco 是一个依赖合约， 完全依赖于 fund。 组合合约是讲多个合约或者数据类型组合在一起构成复杂的数据结构和合约， 来解决更加复杂的问题


## 合约的继承

Solidity 也支持面向对象， 因此也提供了继承。 被继承的合约是父合约或者基合约， 继承的合约是子合约或者派生合约。 合约的派生存在 is-a 关系， is 关键字用于在派生合约中继承合约。

### 单继承
下面是两份合约， parent 和 child， child 继承parent 的所有公共变量和函数。

```
// parent

contract Parent {
  function getParentName() returns(string) {
    return "parent";
  }
}
```

```
// child
contract Child is Parent {
  function getChildName returns(string) {
    return "child";
  }
}

```

```
contract Client {
  Child c =  new Child()

  function getNames() public returns(string, string) {
    return c.getParentName, c.getChildName;
  }
}

```


### 多继承

多继承有多种方式
1. 一个派生合约， 有多个基合约， 这种行为是多继承， 例如 A 继承了 B ， B 继承了 C
2. 分层继承： 基合约被 两个子合约同时继承
3. 多重继承： 基合约被两个子合约继承， 两个子合约有作为基合约被子合约继承


