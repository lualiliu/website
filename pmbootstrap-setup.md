# pmbootstrap 搭建

pmbootstrap 是 postmarketOS 的构建工具，用于构建和开发 postmarketOS 系统。

## 安装依赖

### Linux

```bash
# Ubuntu24.04
$ sudo apt-get install git python3 python3-pip python3-setuptools python3-wheel python3-dev python3-tomli

## 安装 pmbootstrap

```bash

git clone https://gitlab.com/postmarketOS/pmbootstrap.git
#创建目录
mkdir -p ~/.local/bin
#建立符号链接
ln -s "$PWD/pmbootstrap/pmbootstrap.py" ~/.local/bin/pmbootstrap
#检查版本
pmbootstrap --version

```

[RETURN](https://lualiliu.github.io/website/)

