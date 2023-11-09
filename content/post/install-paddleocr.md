+++
title = 'Install Paddleocr'
date = 2023-11-09T18:42:36+08:00
draft = true
+++
# 安装paddleocr

## 安装需要的包
```shell
apt install swig libpython3-dev python3-dev libgl1-mesa-dev
```
## 创建虚拟环境
```shell
apt install python3-venv
python3 -m venv .env
source .env/bin/active
```

## 安装paddlepaddle 和 paddleocr
```shell
python -m pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple
python -m pip install "paddleocr>=2.0.1"
```