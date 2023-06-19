# Config组件<a name="ZH-CN_TOPIC_0000001102487950"></a>

-   [简介](#section11660541593)
-   [内核的Config组成模块](#section28381947133910)
-   [目录](#section161941989596)
-   [使用说明](#section1393789267)
-   [构建说明](#section19369206113115)
-   [相关仓](#section1371113476307)

## 简介<a name="section11660541593"></a>

OpenHarmony的Linux内核基于开源Linux内核LTS **4.19.y / 5.10.y** 分支演进，在此基线基础上，回合CVE补丁及OpenHarmony特性，作为OpenHarmony Common Kernel基线。针对不同的芯片，各厂商合入对应的板级驱动补丁，完成对OpenHarmony的基线适配。

Linux社区LTS 4.19.y分支信息请查看[kernel官网](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-4.19.y)；

Linux社区LTS 5.10.y分支信息请查看[kernel官网](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-5.10.y)。

## 目录<a name="section161941989596"></a>

针对标准系统开源开发板rk3568，给出对应的config分层配置目录结构、文件示例：
```
kernel/linux/config
└── linux-5.10
    ├── base_defconfig                                # 内核必选模块或安全红线特性配置
    ├── type                                          # 形态配置目录
    │   ├── small_defconfig                           # 小型系统常用配置文件
    │   └── standard_defconfig                        # 标准系统常用配置文件
    ├── form                                          # 版本配置目录
    │   └── debug_defconfig                           # 调试版本配置文件（示例目前无配置需求）
    ├── rk3568                                        # 厂商平台配置目录
    │   └── arch
    │       └── arm64_defconfig                       # 芯片单板64位版本相关配置文件
    └── product                                       # 相关产品类型配置目录
        └── phone_defconfig                           # 手机类型产品相关配置文件
```

## 内核的Config组成模块<a name="section28381947133910"></a>

1. 基础通用配置文件（base）：其中包含OpenHarmony特性依赖的内核必选模块以及安全红线特性等必须开启的配置项，其中配置不可被覆盖。如需新增、调整其中配置需上内核-sig例会进行评审。默认必选配置文件。

2. 系统形态配置文件（type）：针对不同的内核版本，config将给出不同内核版本的对应不同的系统的参考通用配置文件。必选配置文件，通过build\_type编译参数确定文件。
	
3. 版本形态配置文件（form）：用于区分版本类型的可选配置文件，通过build\_variant编译参数确定文件，如果文件不存在，则忽略该层配置文件。

4. 芯片平台配置文件（chip）：由具体厂商提供的和芯片、单板相关的内核模块、驱动等配置，为必选配置文件，通过target\_cpu编译参数确定文件。

5. 产品类型配置文件（product）：如同一芯片平台同时又多种产品形态时候，需要做区分配置可以配置使用该层配置。为可选配置层，通过device\_type编译参数确定文件


## 使用说明<a name="section1393789267"></a>

1. 合入HDF补丁

	在kernel/linux/build仓中，按照kernel.mk中HDF的补丁合入方法，合入不同内核版本对应的HDF内核补丁：
	
	```
	$(OHOS_BUILD_HOME)/drivers/hdf_core/adapter/khdf/linux/patch_hdf.sh $(OHOS_BUILD_HOME) $(KERNEL_SRC_TMP_PATH) $(KERNEL_PATCH_PATH) $(DEVICE_NAME)
	```

2. 合入芯片平台驱动补丁

	以Hi3516DV300为例：
	
	在kernel/linux/build仓中，按照kernel.mk中的芯片组件所对应的patch路径规则及命名规则，将对应的芯片组件patch放到对应路径下：
	
	```
	DEVICE_PATCH_DIR := $(OHOS_BUILD_HOME)/kernel/linux/patches/${KERNEL_VERSION}/$(DEVICE_NAME)_patch
	DEVICE_PATCH_FILE := $(DEVICE_PATCH_DIR)/$(DEVICE_NAME).patch
	```

3. 修改自己所需要编译的config

	在kernel/linux/build仓中，按照kernel.mk中的芯片组件所对应的patch路径规则及命名规则，将对应的芯片组件config放到对应路径下：
	
	```
	KERNEL_CONFIG_PATH := $(OHOS_BUILD_HOME)/kernel/linux/config/${KERNEL_VERSION}
	DEFCONFIG_FILE := $(DEVICE_NAME)_$(BUILD_TYPE)_defconfig
	```
	
	> **须知：** 
	>
	>由于OpenHarmony工程的编译构建流程中会拷贝kernel/linux/linux-\*\.\*的代码环境后进行打补丁动作，在使用OpenHarmony的版本级编译命令前，需要kernel/linux/linux-\*\.\*原代码环境。
	>
	>根据不同系统工程，编译完成后会在out目录下的kernel目录中生成对应实际编译的内核，基于此目录的内核，进行对应的config修改，将最后生成的\.config文件cp到config仓对应的路径文件里，即可生效。

## 构建说明<a name="section19369206113115"></a>
以hi3516dv300开源开发板+ubuntu x86主机开发环境为例

使用工程的全量编译命令，编译生成uImage内核镜像

```
./build.sh --product-name Hi3516DV300              # 编译hi3516dv300镜像
    --build-target build_kernel                    # 编译hi3516dv300的uImage内核镜像
    --gn-args linux_kernel_version=\"linux-5.10\"  # 编译指定内核版本
```

## 相关仓<a name="section1371113476307"></a>
<u>kernel\_linux\_config</u>

