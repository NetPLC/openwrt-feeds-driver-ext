#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rtl8723du
PKG_VERSION:=1.0.0
PKG_RELEASE:=1
PKG_LICENSE:=GPLv2

PKG_SOURCE_URL:=https://github.com/thirdyouth/rtl8723du.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2019-03-20
PKG_SOURCE_VERSION:=a094122491f7a2380f574908751676f27bed7cd9

PKG_MAINTAINER:=Han Pengfei <pengphei@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8723du
  SUBMENU:=Wireless Drivers
  TITLE:=rtl8723du Support
  DEPENDS:=+kmod-mac80211
  FILES:=$(PKG_BUILD_DIR)/rtl8723du.ko
  AUTOLOAD:=$(call AutoProbe,rtl8723du)
  PROVIDES:=kmod-rtl8723du
endef

define KernelPackage/rtl8723du/description
 Kernel module for rtl8723 wireless module.
endef

EXTRA_KCONFIG:=\
	CONFIG_RTL8723DU=m \
	CONFIG_IOCTL_CFG80211=y \
	CONFIG_CONCURRENT_MODE=y \
	CONFIG_LITTLE_ENDIAN=y \
	USER_MODULE_NAME=rtl8723du 

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \
	-DRTW_USE_CFG80211_STA_EVENT

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR)/include \
	-I$(PKG_BUILD_DIR)/hal/phydm \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	USER_EXTRA_CFLAGS="$(EXTRA_CFLAGS) $(NOSTDINC_FLAGS)" \
	$(EXTRA_KCONFIG) \
	PROJECT_DIR="$(PKG_BUILD_DIR)"
	

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,rtl8723du))
