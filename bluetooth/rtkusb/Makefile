#
# Copyright (C) 2019 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rtk_btusb
PKG_VERSION:=1.0.0
PKG_RELEASE:=1
PKG_LICENSE:=GPLv2

PKG_SOURCE_URL:=https://github.com/thirdyouth/rtk_btusb.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2019-03-31
PKG_SOURCE_VERSION:=d24bc925aa52536d62832241605c1f01da644069

PKG_MAINTAINER:=Han Pengfei <pengphei@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtk_btusb
  SUBMENU:=Bluetooth Drivers
  TITLE:=rtk USB Bluetooth Support
  DEPENDS:=+kmod-bluetooth
  FILES:=$(PKG_BUILD_DIR)/bluetooth_usb_driver/rtk_btusb.ko
  AUTOLOAD:=$(call AutoProbe,rtk_btusb)
  PROVIDES:=kmod-rtk_btusb
endef

define KernelPackage/rtk_btusb/description
 Kernel module for rtk USB Bluetooth module.
endef

define Package/rtk-bt-firmware-default
  SECTION:=firmware
  CATEGORY:=Firmware
  TITLE:=RTK $(1) firmware
  DEPENDS:=+kmod-rtk_btusb
endef

define Package/rtk-bt-firmware-8723du
$(call Package/rtk-bt-firmware-default,8723du)
endef

define Package/rtk-bt-firmware-8723du/install
	$(INSTALL_DIR) $(1)/lib/firmware
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/8723DU/rtl8723du_fw $(1)/lib/firmware/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/8723DU/rtl8723du_config $(1)/lib/firmware/
endef

EXTRA_KCONFIG:= \

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	SUBDIRS="$(PKG_BUILD_DIR)/bluetooth_usb_driver" \
	$(EXTRA_KCONFIG) \
	PROJECT_DIR="$(PKG_BUILD_DIR)" \

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,rtk_btusb))
$(eval $(call BuildPackage,rtk-bt-firmware-8723du))