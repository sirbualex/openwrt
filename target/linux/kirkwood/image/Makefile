#
# Copyright (C) 2009-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

NAND_BLOCKSIZE := 2048-128k

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

KERNEL_LOADADDR:=0x8000
TARGET_DEVICES = linksys-audi linksys-viper dockstar goflexnet goflexhome iconnect pogo_e02 ib62x0

UBI_OPTS := -m 2048 -p 128KiB -s 512
UBIFS_OPTS := -m 2048 -e 126KiB -c 4096

define Device/Default
  PROFILES := Default
  KERNEL_DEPENDS = $$(wildcard $(DTS_DIR)/$$(DEVICE_DTS).dts)
  KERNEL := kernel-bin | append-dtb | uImage none
  KERNEL_NAME := zImage
  KERNEL_SUFFIX  := -uImage
  KERNEL_INSTALL := 1

  PAGESIZE := 2048
  SUBPAGESIZE := 512
  BLOCKSIZE := 128k
  IMAGES := sysupgrade.tar
  IMAGE/sysupgrade.tar := sysupgrade-tar
endef

define Device/dockstar
  DEVICE_DTS := kirkwood-dockstar
  FILESYSTEMS := squashfs
  DEVICE_TITLE := Seagate FreeAgent Dockstar
  IMAGES += factory.bin
  IMAGE/factory.bin := append-ubi
  KERNEL_IN_UBI := 1
endef

define Device/goflexnet
$(Device/dockstar)
  DEVICE_TITLE := Seagate GoFlexNet
  DEVICE_DTS := kirkwood-goflexnet
endef

define Device/goflexhome
$(Device/dockstar)
  DEVICE_TITLE := Seagate GoFlexHome
  DEVICE_DTS := kirkwood-goflexhome
endef

define Device/linksys-audi
  DEVICE_TITLE := Linksys EA3500 (Audi)
  DEVICE_PACKAGES := kmod-mwl8k swconfig wpad-mini
  DEVICE_DTS := kirkwood-linksys-audi
  KERNEL_SIZE := 2624k
  FILESYSTEMS := squashfs
  IMAGES += factory.bin
  UBINIZE_OPTS := -E 5
  IMAGE/factory.bin := append-kernel | pad-to $$$$(KERNEL_SIZE) | append-ubi
endef

define Device/linksys-viper
  DEVICE_TITLE := Linksys E4200v2 / EA4500 (Viper)
  DEVICE_PACKAGES := kmod-mwl8k swconfig wpad-mini
  DEVICE_DTS := kirkwood-linksys-viper
  KERNEL_SIZE := 2688k
  FILESYSTEMS := squashfs
  IMAGES += factory.bin
  UBINIZE_OPTS := -E 5
  IMAGE/factory.bin := append-kernel | pad-to $$$$(KERNEL_SIZE) | append-ubi
endef

define Device/iconnect
$(Device/dockstar)
  DEVICE_TITLE := Iomega Iconnect
  DEVICE_DTS := kirkwood-iconnect
endef

define Device/iomega_ix2_200
  DEVICE_PACKAGES := kmod-mwl8k swconfig kmod-gpio-button-hotplug kmod-ata-core kmod-ata-marvell-sata kmod-usb2 kmod-usb-storage \
  		     kmod-mmc kmod-mvsdio kmod-i2c-core kmod-i2c-mv64xxx kmod-thermal-kirkwood 
  DEVICE_DTS := kirkwood-iomega_ix2_200
  FILESYSTEMS := squashfs
  IMAGE_SIZE := 29360128
  IMAGES := sysupgrade.tar rootfs.ubi
  IMAGE/sysupgrade.tar := sysupgrade-tar
  IMAGE/rootfs.ubi := append-ubi | check-size $$$$(IMAGE_SIZE)
  UBIFS_OPTS:= -m 512 -e 15872 -c 2048
  UBI_OPTS:= -m 512 -p 16384 -s 256
  PAGESIZE := 512
  SUBPAGESIZE := 256
  BLOCKSIZE := 16KiB
  KERNEL_IN_UBI := 0
  UBINIZE_OPTS := -E 5  
  DEVICE_TITLE := Iomega StorCenter ix2-200
  BOARD_NAME := iomega_ix2_200
  SUPPORTED_DEVICES += iomega_ix2_200 iom,ix2-200
endef
TARGET_DEVICES += iomega_ix2_200

define Device/pogo_e02
$(Device/dockstar)
  DEVICE_TITLE := Cloud Engines Pogoplug E02
  DEVICE_DTS := kirkwood-pogo_e02
endef

define Device/ib62x0
$(Device/dockstar)
  DEVICE_TITLE := RaidSonic ICY BOX IB-NAS62x0
  DEVICE_DTS := kirkwood-ib62x0
endef

$(eval $(call BuildImage))
