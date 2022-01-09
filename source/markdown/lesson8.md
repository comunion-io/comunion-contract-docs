# 实战（一） 合约的编写
## ganache 的安装

开发前， 我们首先本地安装[ganache](https://www.npmjs.com/package/ganache-cli)
```
npm install -g ganache-cli
```
然后运行
```
ganache-cli
```
系统会帮我们生成10个账号和10个对应的秘钥key
## remix 的安装

1. 运行命令
```
npm install -g @remix-project/remixd
```
2. 打开 remix (https://remix.ethereum.org)

3. 启动,

   ```
    remixd -s  [你的源码目录] --remix-ide  https://remix.ethereum.org
   ```

## 合约的编写
我们假设前端提交了一个表单， 表单中包含id, name, mission, description 这些字段， 而这些字段都要上链：

```
pragma solidity >=0.4.21 <0.7.0;
pragma experimental ABIEncoderV2;

contract Startup
{
    address private _owner;
    address payable private _coinbase;

    struct Profile {
        string id;
        string name;
        string mission;
        string description;
    }
    event createdStartup(string startupId, Profile startUp);

    mapping(string => Profile) startups;

    modifier isOwner() {
        require(msg.sender == _owner);
        _;
    }

    constructor()
    public {
        _owner = msg.sender;
    }

    function setCoinBase(address payable addr)
    isOwner
    public {
        _coinbase = addr;
    }

    /*
    * tuple param, as (id, name, categoryId...)
    */
    function newStartup(Profile memory p) public payable {
        require(bytes(p.id).length != 0, "id can not be null");
        startups[p.id] = p;
        emit createdStartup(p.id, p);
    }

    function getStartup(string calldata id)
    external
    view
    returns (Profile memory p){
        return startups[id];
    }
}
```

以上代码我们是书写了一个合约创建 startup 的合约， 这个合约主要包括两个函数， 一个是创建startup , 第二个是获取startup

## 运行发布合约
1. 把以上合约贴入 remix 中， 编译
2. 我们再web3 provider 中 选择本地启动的gananche， 讲remix 与 ganache 连接
3. 点击部署， 就可以看到我们书写的函数
4. 在createStartup 中输入： [ "1", "start", "seddd", "sedcription"]
5. 点击transcat, 即可看到执行成功， 展开信息， 可以看到上链的详情
6. 本地合约填写完成后， 我们开始部署到测试网络
7. 断开与本地的ganache 的连接， 选择 injected web3
8. 安装[metamask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=zh-CN)
9. 安装后记住秘钥（很重要，丢失后， 资产无法找回）
10. metamask 连接generli
11. 点击购买， 即可获取到， 打开页面后， 贴入自己的钱包地址， 获取ether
12. 回到remix 页面， 然后点击部署， 即可吊起metamask， 点击确定交易后， 等待上链结果， 不出意外， 等待1分钟， 就可以看到上链成功的反馈消息，
13. 我们打开上链的详情， 复制上链地址
15. 复制 solidity 的合约abi保存在本地