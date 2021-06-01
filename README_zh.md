# Config组件<a name="ZH-CN_TOPIC_0000001102487950"></a>

-   [简介](#section11660541593)
-   [内核的Config组成模块](#section28381947133910)
-   [目录](#section161941989596)
-   [使用](#section1393789267)
    -   [使用说明](#section1352114469620)
    -   [构建指导](#section72118467716)

-   [以hi3516dv300开源开发板+ubuntu x86主机开发环境为例](#section19369206113115)
    -   [场景1：版本级编译原生方式](#section1025111193220)
    -   [场景2：单独编译修改后的内核](#section17446652173211)

-   [相关仓](#section1371113476307)

## 简介<a name="section11660541593"></a>

OpenHarmony的Linux内核基于开源Linux内核LTS 4.19.y分支演进，为满足不同的内核场景诉求，针对性地合入CVE补丁 + OpenHarmony 特性 + vendor厂商具体的板级芯片驱动补丁从而构成完整的内核基线。

Linux社区LTS 4.19.y分支信息请查看[kernel官网](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-4.19.y)。

## 内核的Config组成模块<a name="section28381947133910"></a>

1. 通用配置文件

针对标准系统给出对应的参考通用配置文件：standard\_common\_defconfig，以便于各开发者参考。

2. 开源开发板配置文件

针对于标准系统开源开发板Hi3516DV300，给出对应的配置文件。

## 目录<a name="section161941989596"></a>

```
kernel/linux/config/linux-4.19         # 内核config
    └── standard_common_defconfig                         # 标准系统的内核的common defconfig
    └── hi3516dv300_emmc_smp_hos_l2_defconfig             # 厂商Hisilicon对应的开源开发板Hi3516dv300标准系统的defconfig
```

## 使用<a name="section1393789267"></a>

### 使用说明<a name="section1352114469620"></a>

如需使用上述config，以Hi3516DV300开发板调试为例，需要在内核代码完成对应芯片驱动patch的合入。

1. 合入芯片平台驱动补丁

针对不同芯片平台合入对应的patch，以Hi3516DV300为例：

```
patch -p1 < device/hisilicon/hi3516dv300/sdk_linux/open_source/linux/hisi_linux-4.19_hos_l2.patch
```

>![](public_sys-resources/icon-notice.gif) **须知：** 
>由于OpenHarmony工程的编译构建流程中会拷贝kernel/linux-4.19的代码环境后进行打补丁动作，在使用OpenHarmony的版本级编译命令前，需要kernel/linux-4.19保持原代码环境。

2. 复制defconfig到kernel/linux-4.19/arch/arm/configs下

### 构建指导<a name="section72118467716"></a>

## 以hi3516dv300开源开发板+ubuntu x86主机开发环境为例<a name="section19369206113115"></a>

### 场景1：版本级编译原生方式<a name="section1025111193220"></a>

使用工程的全量编译命令，编译生成uImage内核镜像

```
./build.sh --product-name Hi3516DV300 # 编译hi3516dv300的uImage内核镜像
```

### 场景2：单独编译修改后的内核<a name="section17446652173211"></a>

1.  准备工作

    准备编译环境，可以使用开源arm clang/gcc编译器，或者使用工程自带编译器。

    进入工程主目录配置环境变量：

    ```
    export PATH=`pwd`/prebuilts/clang/host/linux-x86/clang-r353983c/bin:`pwd`/prebuilts/gcc/linux-x86/arm/gcc-linaro-7.5.0-arm-linux-gnueabi/bin/:$PATH # 配置编译环境
    MAKE_OPTIONES="ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- CC=clang HOSTCC=clang" # 使用工程项目自带的clang环境
    ```

2.  修改内核代码或内核config （OpenHarmony提供对应平台的defconfig供参考）。
3.  创建编译目录及生成内核.config。

    ```
    make ${MAKE_OPTIONES} hi3516dv300_emmc_smp_hos_l2_defconfig # 使用自带的默认config 构建内核
    ```

4.  编译生成对应的内核Image。

    ```
    make ${MAKE_OPTIONES} -j32 uImage # 编译uImage内核镜像
    ```


## 相关仓<a name="section1371113476307"></a>

<u>kernel\_linux\_config</u>

