# Config<a name="EN-US_TOPIC_0000001102487950"></a>

-   [Introduction](#section11660541593)
-   [Composition of Config](#section28381947133910)
-   [Directory Structure](#section161941989596)
-   [Usage](#section1393789267)
-   [Build](#section19369206113115)
-   [Repositories Involved](#section1371113476307)

## Introduction<a name="section11660541593"></a>

Evolved from the open-source Linux kernel LTS 4.19.y and 5.10.y, the OpenHarmony Linux kernel has incorporated CVE patches and OpenHarmony features as the OpenHarmony common kernel baseline. Vendors can complete the kernel adaptation by applying the driver patches for boards.

For more information about Linux LTS 4.19.y, visit the [official kernel website](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-4.19.y).

For more information about Linux LTS 5.10.y, visit the [official kernel website](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-5.10.y).

## Composition of Config<a name="section28381947133910"></a>

1. Common configuration file

	Config provides common configuration files for different systems:
	
	Standard system: **standard\_common\_defconfig**
	
	Small system: **small\_common\_defconfig**

2. Configuration files for open-source development boards

	Configuration file for the open-source development board Hi3516D V300 used in the standard system 

## Directory Structure<a name="section161941989596"></a>

```
kernel/linux/config
├── linux-4.19
│   └── arch
│       └── arm
│           └── configs
│               ├── hi3516dv300_small_defconfig       # Small-system defconfig of the open-source Hi3516D V300 development board from HiSilicon
│               ├── hi3516dv300_standard_defconfig    # Standard-system defconfig of the open-source Hi3516D V300 development board from HiSilicon
│               ├── small_common_defconfig            # Common defconfig of the small-system kernel
│               └── standard_common_defconfig         # Common defconfig of the standard-system kernel
└── linux-5.10
    └── arch
        └── arm
            └── configs
                ├── hi3516dv300_small_defconfig       # Small-system defconfig of the open-source Hi3516D V300 development board from HiSilicon
                ├── hi3516dv300_standard_defconfig    # Standard-system defconfig of the open-source Hi3516D V300 development board from HiSilicon
                ├── small_common_defconfig            # Common defconfig of the small-system kernel
                └── standard_common_defconfig         # Common defconfig of the standard-system kernel
```

## Usage<a name="section1393789267"></a>

1. Apply HDF patches.

	Apply the HDF kernel patches matching your kernel version. For details, see the method in **kernel.mk** in the **kernel/linux/build** repository.
	
	```
	$(OHOS_BUILD_HOME)/drivers/hdf_core/adapter/khdf/linux/patch_hdf.sh $(OHOS_BUILD_HOME) $(KERNEL_SRC_TMP_PATH) $(KERNEL_PATCH_PATH) $(DEVICE_NAME)
	```

2. Apply the chip driver patches.

	The following uses Hi3516D V300 as an example.
	
	Place the patches for the chip component in the corresponding path based on the path and naming rules for the patches of the chip component in **kernel.mk** in the **kernel/linux/build** repository.
	
	```
	DEVICE_PATCH_DIR := $(OHOS_BUILD_HOME)/kernel/linux/patches/${KERNEL_VERSION}/$(DEVICE_NAME)_patch
	DEVICE_PATCH_FILE := $(DEVICE_PATCH_DIR)/$(DEVICE_NAME).patch
	```

3. Modify the **config** file to build.

	Place the **config** file for the chip component in the corresponding path based on the path and naming rules of the chip component in **kernel.mk** in the **kernel/linux/build** repository.
	
	```
	KERNEL_CONFIG_PATH := $(OHOS_BUILD_HOME)/kernel/linux/config/${KERNEL_VERSION}
	DEFCONFIG_FILE := $(DEVICE_NAME)_$(BUILD_TYPE)_defconfig
	```
	
	> **Note**:
	>
	>In the OpenHarmony project build process, patches are installed after **kernel/linux/linux-\*\.\*** is copied. Before using the version-level build command of OpenHarmony, ensure that the **kernel/linux/linux-\*\.\*** source code is available.
	>
	>The kernel built is generated in the **kernel** directory under the **out** directory. Modify the **config** file based on the kernel built, and copy the generated **.config** file to the corresponding path in the **config** repository. Then, the configuration takes effect.

## Build<a name="section19369206113115"></a>
The following uses the Hi3516D V300 development board and Ubuntu x86 server as an example.

Perform a full build for the project to generate the **uImage** kernel image.

```
./build.sh --product-name Hi3516DV300              # Build the Hi3516D V300 image.
    --build-target build_kernel                    # Build the uImage kernel image of Hi3516D V300.
    --gn-args linux_kernel_version=\"linux-5.10\"  # Build the specified kernel version.
```

## Repositories Involved<a name="section1371113476307"></a>
<u>kernel\_linux\_config</u>
