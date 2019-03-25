#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=rtl8821cu
PKG_VERSION:=1.0.1
PKG_RELEASE:=2
PKG_LICENSE:=GPLv2

PKG_SOURCE_URL:=https://github.com/thirdyouth/rtl8821cu.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2019-03-25
PKG_SOURCE_VERSION:=eff942898ac6272bf2a61a07380d739abe0037cd

PKG_MAINTAINER:=Han Pengfei <pengphei@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8821cu
  SUBMENU:=Wireless Drivers
  TITLE:=rtl8821cu Support
  DEPENDS:=+kmod-mac80211
  FILES:=$(PKG_BUILD_DIR)/rtl8821cu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8821cu)
  PROVIDES:=kmod-
endef

define KernelPackage/rtl8821cu/description
 Kernel module for rtl8821cu wireless module.
endef

EXTRA_KCONFIG:=\
	CONFIG_RTL8821CU=m \
	CONFIG_IOCTL_CFG80211=y \
	CONFIG_CONCURRENT_MODE=y \
	CONFIG_LITTLE_ENDIAN=y \
	USER_MODULE_NAME=rtl8821cu 

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \
	-DRTW_USE_CFG80211_STA_EVENT \
	-I$(PKG_BUILD_DIR)/include \
	-I$(PKG_BUILD_DIR)/hal/phydm

NOSTDINC_FLAGS = \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	USER_EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG) \
	PROJECT_DIR="$(PKG_BUILD_DIR)" \
	NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)"
	

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,rtl8821cu))
