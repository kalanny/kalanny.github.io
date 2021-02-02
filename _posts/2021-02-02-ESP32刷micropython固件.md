---
layout: post
title: ESP32刷micropython固件
categories: [ESP32]
tags: [ESP32, micropython]
fullview: true
---

## 1.esptool安装（连接esp32和刷固件使用）

    pip install esptool

## 2.adafruit-ampy 安装 （发送文件到ESP32）

    pip install adafruit-ampy

## 3.擦除原ESP32固件的命令

    esptool.py --chip esp32 --port COM5 erase_flash #修改COM端口号，在设备管理器端口中查看

## 4.下载ESP32的micropython固件并烧录

下载地址：https://micropython.org/download/esp32/ 目前最新版本 [esp32-idf3-20200902-v1.13.bin](https://micropython.org/resources/firmware/esp32-idf3-20200902-v1.13.bin)

    esptool.py --chip esp32 --port COM5 --baud 115200 write_flash -z 0x1000 esp32-idf3-20200902-v1.13.bin

## 5.向ESP32上传文件

    ampy --port COM5 put test.py

## 6.使用putty连接ESP32

 ![putty_ESP32](../images/putty_esp32.jpg)

 ![putty_ESP32](../images/putty_esp32_1.jpg)





