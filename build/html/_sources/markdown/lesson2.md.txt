# 第二章 环境搭建

##  安装 truffle
```
npm install -g truffle
```

## 初始化 project
```
truffle init
```


##  创建本地环境
1. 使用 remixed 工具

```
 npm install -g @remix-project/remixd
```

2. 打开 remixed 页面 ( http://remix.ethereum.org)

3. 启动， 在你的根目录下， 运行
```
remixed -s [your source code list] --remix-ide http://remix.ethereum.org
```

4. 打开 "https://remix.ethereum.org/#optimize=true&runs=200"

5. 点击 home -> file -> connect to local host;

6. 加载完本地代码之后， 就可以开始编码了

## ganache 连接本地

7. if you installed ganache, you can use ganache;