# Ruyi SDK文档调研报告

## 开发板：Orange Pi RV2

### 镜像：Orangepirv2_1.0.0_ubuntu_noble_desktop_gnome_lin。

## Ruyi 包管理器

### 安装：

#### 	使用预编译的二进制安装：

​		在文档的开始，列出了x86_64、aarch64、riscv64三种架构的下载方法，但是文档中对于二进制文件添加权限与安装二进制到PATH目录下，只以X86架构为例，没有列出aarch与riscv64架构的指令方法。而对于切换三种不同架构下的安装方法，被写在了“使用案例----使用Ruyi编译环境构建（以Licheepi 4A为例）----ruyi包管理器的安装”此目录下面。如果用户开始只为了完成下载任务，那么很有可能不会进入此目录下，并无法发现此处可以解决不同架构下的安装问题。

#### 	使用python包管理器安装：

​		对于RISC-V架构，在使用此方法时，会遇到缺少python.h、libgit2库的开发文件（git2.h）等问题，会导致编译失败无法构建成功。

### 使用案例：

#### 使用Ruyi编译环境构建（以Licheepi 4A）为例：

```
# 使用帮助命令了解使用
$ ruyi venv -h

# 创建虚拟环境 venv-sipeed
$ ruyi venv -t gnu-plct-xthead sipeed-lpi4a venv-sipeed 

# 查看编译环境中的工具
$ ls venv-sipeed/bin/ 

# 激活虚拟环境（虚拟环境可以理解成一个容器，实现运行环境隔离的设计，激活后，在 venv-sipeed 这个环境中，使用的就是 gnu-plct-xthead 版本工具链。不创建虚拟环境也可以为 /home/sipeed/.local/share/ruyi/binaries/riscv64/gnu-plct-xthead-2.8.0-ruyi.20240222/bin 配置环境变量，直接使用环境变量指定的gcc编译）
$ . venv-sipeed/bin/ruyi-activate 

# 查看当前虚拟环境下的 gcc 是否可用
«Ruyi venv-sipeed» sipeed@lpi4a1590:~$ riscv64-plctxthead-linux-gnu-gcc --version 
```

​	最后一条指令，“# 查看当前虚拟环境下的 gcc 是否可用”显示出了用户名，对比前几条指令，应该是文档编写者在编写时的复制黏贴多复制了用户名。

#### 使用CMake和Meson集成：

​	下载zlib、安装并激活gnu-plct工具链编译、解压zlib三个步骤存在逻辑上的错误。官网文档的指令顺序为

```
#下载zlib
$ wget https://github.com/zlib-ng/zlib-ng/archive/refs/tags/2.2.2.tar.gz
$ mkdir zlib-ng
$ cd zlib-ng

安装并激活gnu-plct工具链编译
$ ruyi install gnu-plct
$ ruyi venv -t gnu-plct generic venv
$ . venv/bin/ruyi-activate

解压zlib
$ tar -xf ./zlib-ng-2.2.2.tar.gz
$ cd zlib-ng-2.2.2
```

​	示例中首先下载了2.2.2.tar.gz的压缩包，后续创建并进入了文件夹zlib-ng。但是在解压文件夹zlib中，直接在当前位置下执行，而zlib-ng-2.2.2.tar.gz压缩包在上一级目录，所以执行解压命令会提示无法找到相应的文件。

​	且激活虚拟环境与解压文件夹并未存在很强的关联性，最好不要穿插在下载与解压文件之间，文档可以调整指令的顺序。

#### 使用QEMU和LLVM：

​	文档的示例指令也出现了粘贴出了虚拟环境的失误，此处应该只给出指令会更好。

```
«Ruyi milkv-venv» $ mkdir coremark
«Ruyi milkv-venv» $ cd coremark
«Ruyi milkv-venv» $ ruyi extract coremark

«Ruyi milkv-venv» $ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi milkv-venv» $ make PORT_DIR=linux64 link
```



## RuyiSDK IDE：

​	RuyiSDk IDE的下载地址https://mirror.iscas.ac.cn/ruyisdk/ide/ 已经失效无法打开。

## 附加

​	新手刚开始接触搭载 Ruyi 系统时，通常会通过 MobaXterm 以 SSH 命令行的方式远程操作开发板；但目前针对如何用命令行，完成不同开发板基础配置，如环境搭建、文件传输、程序编译的示例教程较少，建议补充这类 “远程命令行操作开发板” 的基础教程，帮助新手快速上手。



