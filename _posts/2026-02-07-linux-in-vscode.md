---
layout: post
title:  "Setup env for linux kernel 2.6.14 source code"
date: 2026-02-07 01:12:13
categories: linux kernel
---

从0开始搭建环境，可以学习linux kernel。
注意，本文基于的kernel版本为2.6.14. 

# 源代码下载 

```zsh
mkdir -p ~/src && cd ~/src
git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
cd linux
git tag -l "v2.6*"
git checkout v2.6.14

```

Dockerfile
```
FROM --platform=linux/amd64 debian:7

# 1. 直接使用官方 archive 源，这是最准确的路径
RUN echo "deb http://archive.debian.org/debian/ wheezy main" > /etc/apt/sources.list && \
    echo "deb http://archive.debian.org/debian-security/ wheezy/updates main" >> /etc/apt/sources.list && \
    echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99ignore-expiry && \
    echo 'Acquire::Retries "10";' > /etc/apt/apt.conf.d/80-retries

# 2. 关键：更新索引。如果官方源太慢，我们通过 AllowInsecure 强制跳过安全校验加速
RUN apt-get update -o Acquire::AllowInsecureRepositories=true || true

# 3. 核心：安装编译工具。这里把所有包放在一起，如果还是下载失败，
# 建议在 Docker Desktop 开启代理，或者多次尝试，因为官方 Archive 确实不稳定。
RUN apt-get install -y --force-yes \
    make \
    bin86 \
    bc \
    gcc \
    libc6-dev-i386 \
    libncurses5-dev \
    build-essential
```

docker交叉编译，笔者平台为Mac
```sh
cd workspace/linux
docker buildx build --platform linux/amd64 -t kernel-study:2.6.14 .
```

创建docker-compose.yml
```yaml
version: '3.8'

services:
  kernel-dev:
    image: kernel-study:2.6.14
    container_name: kernel_26_build
    platform: linux/amd64
    volumes:
      # 挂载当前目录到容器的 /workspace
      - .:/workspace
    working_dir: /workspace/linux
    # 保持容器运行，方便随时 exec 进入
    stdin_open: true
    tty: true
    privileged: true
```



在workspace目录下执行如下sh脚本
```bash
#!/bin/bash
# 进入源码目录
cd linux-2.6.14

echo "Applying patches for 2.6.14 on amd64..."

# 1. 修复 PATH_MAX 缺失 (scripts/mod/sumversion.c)
if ! grep -q "limits.h" scripts/mod/sumversion.c; then
    sed -i '1i#include <limits.h>' scripts/mod/sumversion.c
fi

# 2. 修复 __always_inline 冲突 (compiler-gcc4.h)
# GCC 4 会把 inline 解释得更严格，导致重复定义
if [ -f include/linux/compiler-gcc4.h ]; then
    sed -i 's/__always_inline inline/__always_inline/g' include/linux/compiler-gcc4.h
fi

# 3. 修复汇编对齐问题 (部分旧版 amd64 汇编格式)
sed -i 's/p2align 4//g' arch/x86_64/kernel/entry.S 2>/dev/null || true

echo "Done. Now you can run 'make x86_64_defconfig'"
```

在linux的上层目录workspace下，运行docker-compose

```sh
docker-compose up -d
docker-compose exec kernel-dev bash

# 停止 docker容器
docker-compose stop
```
在容器内执行
```sh
# 配置内核
# make x86_64_defconfig
# 或者，如果上面命令有问题，则执行
# 你运行 make x86_64_defconfig 时，内核去 arch/x86_64/configs/ 目录下找这个文件，
# 但实际上那个年代这个文件的名字并不叫 x86_64_defconfig，而是直接叫 defconfig。
# 直到后来的 2.6.2x 版本，内核才把 i386 和 x86_64 合并成统一的 arch/x86，
# 并规范了 defconfig 的命名。
make ARCH=x86_64 defconfig
make ARCH=x86_64 menuconfig
# 如果你想精简内核或开启调试符号，可以用 menuconfig (需要调整终端大小)
# make menuconfig

# 开始全量编译
make -j$(nproc)
```

在mac上用qemu运行编译出来的内核
```sh
qemu-system-x86_64 \
  -kernel ./linux-2.6.14/arch/x86_64/boot/bzImage \
  -append "console=ttyS0" \
  -nographic \
  -serial mon:stdio
```

# Docker 环境配置
设置Docker daemon的代理，通过使用本机系统代理。 
![Docker setup diagram]({{ site.baseurl }}/assets/images/docker-conf-1.jpg){: width="80%" }

# IDE环境搭建
VS code的配置
在kernel的根目录下，创建.vscode目录
为VS Code 生成一个 .vscode/c_cpp_properties.json 文件。
```json
{
    "configurations": [
        {
            "name": "Linux-2.6.14-AMD64",
            "includePath": [
                "${workspaceFolder}/include",
                "${workspaceFolder}/include/asm-x86_64",
                "${workspaceFolder}/include/asm-generic",
                // "${workspaceFolder}/arch/x86_64/include",
                "${workspaceFolder}/drivers/char",
                // 强制包含生成的头文件目录
                "${workspaceFolder}/include/linux"
            ],
            "defines": [
                "__KERNEL__",
                "MODULE",
                "KBUILD_STR(s)=#s",
                "KBUILD_MODNAME=KBUILD_STR(kernel)",
                "CONFIG_X86_64",
                "ARCH=x86_64",
            ],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu89",
            "cppStandard": "gnu++98",
            "intelliSenseMode": "linux-gcc-x64",
            "browse": {
                "path": [
                    "${workspaceFolder}/include",
                    "${workspaceFolder}/arch/x86_64",
                    "${workspaceFolder}/kernel",
                    "${workspaceFolder}/mm",
                    "${workspaceFolder}/fs",
                    "${workspaceFolder}/net",
                    "${workspaceFolder}/drivers"
                ],
                "limitSymbolsToIncludedHeaders": false,
                "databaseFilename": ""
            }
        }
    ],
    "version": 4
}
```

## clangd
clangd 根本不会理会 .vscode/c_cpp_properties.json。

你这是在用 A 系统的说明书去修 B 系统的引擎。.vscode/c_cpp_properties.json 是 Microsoft C/C++ 扩展（也就是带有“小火焰”图标的那个）专属的配置文件；而 clangd 是一个完全独立的语言服务器。

## 在禁用clang之后，vscode依然不跳转

### 确认 Microsoft C/C++ 扩展已激活
在 VS Code 1.109 中，插件不会无缘无故运行。
看右下角状态栏：
你是否看到了一个类似 “C/C++” 的字样，或者一个小圆柱/火焰图标？
如果没有，点击左侧扩展栏，确保 C/C++ (ms-vscode.cpptools) 已经安装并且是 Enabled 状态。
强制触发：在 linux-2.6.14 目录下随便点开一个 .c 文件（比如 kernel/sched.c），这通常会触发扩展加载。
### 解决“不跳转”的头号嫌疑人：IntelliSense 引擎选择
有时 VS Code 会因为之前装过 clangd 而把 IntelliSense 引擎禁用了。
按下 Cmd + , (打开设置)。
搜索 C_Cpp: Intelli Sense Engine。
确保它被设置为 "default"（如果被设置成了 "Disabled"，跳转就废了）。

![Intelli Sense Engine 配置]({{ site.baseurl }}/assets/images/vscode-sense-engine.jpg){: width="80%" }




