---
title: linux服务器常用配置
date: 2022-01-01 18:00:00
updated: 2022-04-24 10:25:00
tag:
- note
- linux
---

仅记录不常用的命令，可能会不全哦👐🏻

<!-- more -->

1. 服务器`conda`切换北外镜像源

```shell
$ conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/free/ 
$ conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/pkgs/main/ 
$ conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/conda-forge 
$ conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/msys2/
$ conda config --set show_channel_urls yes 
$ conda config --add channels https://mirrors.bfsu.edu.cn/anaconda/cloud/pytorch/
```


2. `kill -STOP 1234` 将进程暂停，如果要让它恢复到后台，用`kill -CONT 1234`。[ref1](https://www.cnblogs.com/kexinxin/p/9939119.html), [ref2](https://www.jianshu.com/p/d4190447736e) 

3. linux解压、安装rar文件

```shell
$ wget http://rarsoft.com/rar/rarlinux-x64-5.5.0.tar.gz
$ sudo tar -zxvf rarlinux-x64-5.5.0.tar.gz
# 编译
$ cd rar
$ sudo make
# 测试
$ rar
# 解压, 按压缩包内的文件结构解压
$ rar x <file_path>
```

4. 查看目录的文件大小使用`du -h <dir_path>`，`-h`代表human，将输出人类易读的文件单位（MB/GB/...）

    

