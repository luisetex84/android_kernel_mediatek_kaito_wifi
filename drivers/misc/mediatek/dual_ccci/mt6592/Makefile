ifeq ($(CONFIG_MTK_CCCI_EXT), y)

include $(srctree)/drivers/misc/mediatek/Makefile.custom
#CCCI_CORE_DRIVER_DIR :=  $(wildcard $(obj)/$(call to-root,$(obj))mediatek/source/kernel/drivers/ccci)
CCCI_CORE_DRIVER_DIR := $(srctree)/mediatek/kernel/drivers/dual_ccci
MASP_CORE_DRIVER_DIR := $(MTK_PATH_PLATFORM)/drivers/masp

ifeq ($(CCCI_CORE_DRIVER_DIR),)
$(error Invalid ccci source path, please check)
endif

obj-$(CONFIG_MTK_CCCI_DRIVER) += ccci_plat.o
ccci_plat-y :=src/ccci_platform.o
ccci_plat-y +=src/ccci_settings.o

ifeq ($(CONFIG_MTK_SECURITY_SW_SUPPORT), y)
include $(MASP_CORE_DRIVER_DIR)/asf/Makefile.core
include $(MASP_CORE_DRIVER_DIR)/mach/Makefile.mach

ifeq ($(CUSTOM_SEC_AUTH_SUPPORT),y)
CCCI_CUSTOM_DRIVER_DIR := $(MTK_PATH_CUSTOM)/$(call lc,$(MTK_PROJECT))/kernel/ccci
ccci_plat-y += $(CCCI_CUSTOM_DRIVER_DIR)/cust_auth.o
else
include $(MASP_CORE_DRIVER_DIR)/asf/Makefile.crypto
sec_obj := $(crypto_obj)
sec_obj += asf/auth/sec_wrapper.o
endif

ifeq ($(CONFIG_MTK_CCCI_DRIVER),m)
ccci_plat-y += $(core_obj) $(mach_obj) $(sec_obj)
endif
endif

#ldflags-y += --whole-archive $(ext-libs)  --no-whole-archive

#ccflags-y  += -I$(CCCI_CORE_DRIVER_DIR)/include -I$(obj)/include
ccflags-y  += -I$(CCCI_CORE_DRIVER_DIR)/include -I$(srctree)/$(obj)/include

clean-files += $(patsubst $(obj)/%,%,$(wildcard *.o)\
	 $(wildcard .*cmd) $(wildcard *~))

clean-files += $(patsubst $(obj)/%,%,$(wildcard src/*.o)\
	 $(wildcard src/.*cmd) $(wildcard src/*~))

ifeq ($(CONFIG_MTK_SECURITY_SW_SUPPORT), y)
ifeq ($(CUSTOM_SEC_AUTH_SUPPORT),y)
clean-files += $(CCCI_CUSTOM_DRIVER_DIR)/cust_auth.o
else
clean-files += $(patsubst $(obj)/%,%,$(wildcard auth/*.o))
endif
endif

ifeq ($(CONFIG_MTK_ENABLE_MD1), y)
ccflags-y  += -DMTK_ENABLE_MD1
endif
ifeq ($(CONFIG_MTK_ENABLE_MD2), y)
ccflags-y  += -DMTK_ENABLE_MD2
endif
ifeq ($(CONFIG_MTK_SECURITY_SW_SUPPORT), y)
ccflags-y  += -DENABLE_MD_IMG_SECURITY_FEATURE
endif
ifeq ($(CONFIG_MTK_SEC_MODEM_NVRAM_ANTI_CLONE), y)
ccflags-y  += -DMTK_SEC_MODEM_NVRAM_ANTI_CLONE
endif

else
obj-  := dummy.o
endif

#obj-  := dummy.o
#my-dir := $(dir $(lastword $(MAKEFILE_LIST)))
#
##CCCI_PLATFORM_DRIVER_FILES := $(filter-out %.h,$(wildcard $(CCCI_PLATFORM_DRIVER_DIR)/*.c)\
##			$(wildcard $(CCCI_PLATFORM_DRIVER_DIR)/prebuilt/lib*.o))
#
#my-files := $(patsubst $(obj)/%,%,$(wildcard $(my-dir)/*.c)  $(wildcard $(my-dir)/prebuilt/*.o))
#ifeq ($(CONFIG_MTK_EMMC_SUPPORT),y)
#	my-libs := $(wildcard $(my-dir)prebuilt/libasp_EMMC.a)
#else 
#	my-libs := $(wildcard $(my-dir)prebuilt/libasp_NAND.a)
#endif
##my-libs := 
##$(info  lib===$(my-libs))
#ifneq ($(my-libs),)
# 
#ldflags-y += -L$(my-dir) --whole-archive $(my-libs)  --no-whole-archive
##--start-group --end-group
#endif
#ccflags-y  +=-I$(my-dir)/include
#ccci-y +=$(my-files:.c=.o)
#
#clean-files += $(patsubst $(obj)/%,%,$(wildcard $(my-dir)/*.o)\
#	 $(wildcard $(my-dir)/.*cmd) $(wildcard $(my-dir)/*~))
