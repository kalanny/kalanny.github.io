---
layout: post
title: Linux Screen超简明教程
categories: [Linux]
tags: [Linux, Screen]
fullview: false
---
1. screen是什么

    Screen是Linux下的一款远程会话管理工具，可以在多个进程之间多路复用一个物理终端的全屏窗口管理器。它可以创建多个会话（Session），每个会话中可以创建多个窗口（Window），每个窗口中可以运行单独的任务，并且互相之间不受影响，还可以方便快速的在不同的窗口和会话之间切换。

2. screen有什么用

    在服务器中执行一些非常耗时的任务时（如下载，压缩，解压缩，编译，安装等），我们通常是单独开一个远程终端窗口来执行这个任务，且在任务执行过程中不能关闭这个窗口或者中断连接，否则正在执行的任务会被终止掉。而有了screen，我们可以在一个窗口中安装程序，然后在另一个窗口中下载文件，再在第三个窗口中编译程序，只需要一个SSH连接就可以同时执行这三个任务，还可以方便的在不同会话或窗口中切换，即使因为意外导致窗口关闭或者连接中断，也不会影响这三个任务的执行。

3. screen的使用说明

    - 安装Screen
        ```
        yum install screen
        ```
    - 常用命令
        ```
        screen -S test    #创建一个名为test的会话
        screen -d test    #退出名为test的会话，但会话中的任务会继续执行。
        screen -ls        #列出所有会话
        screen -r test    #恢复名为test的会话
        ```
    - 常用快捷键
        ```
        Ctrl+a d  :  效果与screen -d相同，退出当前会话
        Ctrl+a w ：  显示当前会话中的窗口列表，显示在标题栏中
        Ctrl+a n ：  切换到下一个窗口
        Ctrl+a p ：  切换到上一个窗口
        Ctrl+a 0-9 ：在第0个窗口和第9个窗口之间切换
        ```
