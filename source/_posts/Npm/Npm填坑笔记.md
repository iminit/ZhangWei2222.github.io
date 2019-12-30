---
title:  Npm填坑笔记
date: 2019-05-24 18:20:37
categories:
- Npm
tags:
- 填坑
comments: false
---

#### 1.node版本不对
- 问题描述
    - npm i时，发现报错primordials is not defined.
- 原因
    - node 12版本太高，须降到稳定版10
- 解决
    - 卸载node，安装nvm 
        - brew install nvm
        - mkdir ~/.nvm;
        - open ~/.zshrc
        - 复制export NVM_DIR="$HOME/.nvm" . "/usr/local/opt/nvm/nvm.sh" 到文件中，
        - source ~/.zshrc
        - nvm install 10.15.3
        - 达到降版本的作用，以及npm也回来啦~
#### 2.安装Node、Mysql报错

```shell
问题1
Error:Directory not empty @ dir_s_rmdir - /usr/local/opt/openssl

解决
sudo rm -rf /usr/local/opt/openssl
brew install openssl
```


```shell
问题2
dyld:Library not loaded:/usr/local/opt/openssl/lib/libssl.1.0.0.dylib

解决
brew remove openssl
brew install openssl
```


```shell
问题3
If you need to have mysql@5.1 first in your PATH num:
echo "export PATH='/USR/LOCAL/OPT...'" >> ~/.zshrc

解决
open ~/.zshrc
复制路径到最后
source ~/.zshrc
```