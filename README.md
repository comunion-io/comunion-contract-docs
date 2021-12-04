# read the docs
Read the Docs 是一个基于 Sphinx 的免费文档托管项目。该项目在 2010 年由 Eric Holscher、Bobby Grace 和 Charles Leifer 共同发起。2011年3月，Python 软件基金会曾给 Read the Docs 项目资助 840 美元，作为一年的服务器托管费用。


Read the Docs 网站：https://readthedocs.org/

本文是仿照[solidity](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html) 官网搭建


## Sphinx
- Sphinx 网站：http://sphinx-doc.org/
- Sphinx 使用手册：https://zh-sphinx-doc.readthedocs.io/en/latest/index.html

## 环境搭建
环境准备
```
sudo apt install git
 sudo apt install make
 sudo apt install python3
 sudo apt install python3-pip

```
然后安装最新版本的 Sphinx 及依赖。
```
 pip3 install -U Sphinx
```

为了方便编译， 还需要安装一下软件包
```
 pip3 install sphinx-autobuild
 pip3 install sphinx_rtd_theme
 pip3 install recommonmark
 pip3 install sphinx_markdown_tables
```


## 如何添加文件
1. 在markdown 下 添加markdown 文件
2. 在index.rst 下， 添加

```
.. toctree::
   :maxdepth: 2
   :caption: BASIC

   markdown/你的文件名
```
3. 在控制台下输入
```
.\make html
```

4. 升级
为了方便阅读， 在控制台下输入
```
 sphinx-autobuild source build/html
```

默认启动 8000 端口，在浏览器输入 http://127.0.0.1:8000

5. 如何贡献：
    - 合约的文档放在 contract 下
    - 后端的文档放在backend 下

# 文档的托管
1. 上传github
2. 网页托管：在 Read the Docs 网站 https://readthedocs.org/ 注册，并绑定 GitHub 账户。点击“Import a Project”导入项目，输入项目名称和仓库地址即可。（注意， 主分支是master, 不是）