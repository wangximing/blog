title: Ruby on Rails 环境搭建
date: 2014-12-19 16:40:50
tags:
---
#### 开发环境
1. IDE RubyMine
2. 文本编辑器和命令行
3. Chrome浏览器

#### Ruby， RubyGems， Rails， Git 安装（MacOS）
1. [RVM](https://rvm.io/rvm/install)
  1. ` curl -sSL https://get.rvm.io | bash -s ` 进行安装。
  2. ` rvm get stable ` 确保是最新版本。
  3. ` rvm requirements ` 减产安装Ruby的需要条件。
    > 若提示 ` command not found `, 运行 ` source ~/.rvm/scripts/rvm`。并根据提示安装相关环境。
2. `rvm install 版本号` 安装Ruby。
3. `rvm use 版本号@别名 --create --default` 设置默认genset。
4. 安装RubyGems
  1. 检查是否安装
  2. 下载RubyGems，解压文件，并运行`ruby setup.rb`
  3. 在~/.gemrc 中配置不生成ri和rdoc文档
    ```
    install: --no-rdoc --no-ri
    update: --no-rdoc --no-ri
    ```
5. 安装Rails`gem install rails --version 4.0.4 --no-ri --no-rdoc`
> rails -v 检查安装
