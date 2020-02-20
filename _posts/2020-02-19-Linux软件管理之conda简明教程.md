---
layout: post
title: Linux软件管理之conda简明教程
categories: [Linux]
tags: [Linux, conda]
fullview: false
---

## conda 简介

- 有很多的生信软件都可以通过conda安装，省去了很多的安装、修bug的烦恼。可以在https://bioconda.github.io/recipes查看是否包含了目标软件。只要一行命令conda install xxx就行了。

- Conda 是一个开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换。 Conda 是为 Python 程序创建的，适用于 Linux，OS X 和Windows，也可以打包和分发其他软件。

- conda分为anaconda和miniconda，anaconda是包含一些常用包的版本，miniconda则是精简版，只包含 conda 和其依赖，所以推荐使用miniconda。

<br>

## 安装配置conda

> conda官网：https://conda.io/miniconda.html

- 安装conda
    ```bash
    wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
    chmod 777 Miniconda3-latest-Linux-x86_64.sh #给执行权限
    bash Miniconda3-latest-Linux-x86_64.sh #运行
    ```

    最后询问是否初始化Miniconda3时，选“no”
    
    安装过程会列出conda安装的具体路径

- 配置conda

    编辑~/.bashrc文件

    在最下行输入miniconda3的安装目录作为环境变量，与上面保存的安装目录相同
    ```bash
    export  PATH="/home/username/miniconda3/bin:"$PATH
    ```
    输入命令使.bashrc文件生效
    ```bash
    source ~/.bashrc
    ```
    输入conda命令，如正常返回，说明conda安装成功

- 添加清华大学的镜像源

    这样安装其他包的时候，下载速度会很快
    ```bash
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
    conda config --set show_channel_urls yes 
    conda config --get channels
    ```
    查看已经添加的channels
    ```bash
    conda config --get channels
    vim ~/.condarc
    ```
<br>

## 使用conda

- 查看已有环境列表
    ```bash
    conda env list
    ```
- 创建新conda环境
    ```bash
    conda create -n 环境名称 python=版本号
    ```
- 激活环境
    ```bash
    source activate 环境名称
    ```
- 退出当前环境
    ```bash
    source deactivate
    ```
- 删除环境
    ```bash
    conda remove -n 环境名称 --all
    ```
<br>

## 利用conda安装生物信息软件

- 安装软件
    ```bash
    conda search 软件名
    conda install 软件名=版本号
    ```
    这时conda会先卸载已安装版本，然后重新安装指定版本。

- 查看已安装软件:
    ```bash
    conda list
    ```
- 更新指定软件:
    ```bash
    conda update 软件名
    ```
- 卸载指定软件:
    ```bash
    conda remove 软件名
    ```
