#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=mt7610u
PKG_VERSION:=1.0.1
PKG_RELEASE:=2
PKG_LICENSE:=GPLv2

# PKG_SOURCE_URL:=https://github.com/thirdyouth/mt7610u.git
# PKG_SOURCE_PROTO:=git
# PKG_SOURCE_DATE:=2019-03-20
# PKG_SOURCE_VERSION:=c64bf83db2be3b3fb18de9f28a7f665f272d5fe5

PKG_MAINTAINER:=Han Pengfei <pengphei@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/mt7610u
  SUBMENU:=Wireless Drivers
  TITLE:=mt7610u Support
  DEPENDS:=+kmod-cfg80211
  FILES:=$(PKG_BUILD_DIR)/os/linux/mt7610u_sta.ko
  AUTOLOAD:=$(call AutoProbe,mt7610u_sta)
  PROVIDES:=kmod-mt7610u
endef

define KernelPackage/mt7610u/description
 Kernel module for mt7610u wireless module.
endef

EXTRA_KCONFIG:=\
	WIFI_MODE=STA \
	CHIPSET=mt7610u \
	HAS_WOW_SUPPORT=n 

 NOSTDINC_FLAGS = \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include generated/autoconf.h \
	-include backport/backport.h

EXTRA_CFLAGS = \
	-I$(PKG_BUILD_DIR)/include \
	$(NOSTDINC_FLAGS) \
	-DLINUX \
	-D__KERNEL__ \
	-DCONFIG_STA_SUPPORT \
	-DSCAN_SUPPORT \
	-DDBG \
	-DMODULE=1 \
	-DMT76x0 \
	-DRT65xx \
	-DRLT_MAC \
	-DRLT_RF \
	-DRTMP_MAC_USB \
	-DRTMP_USB_SUPPORT \
	-DRTMP_TIMER_TASK_SUPPORT \
	-DA_BAND_SUPPORT \
	-DRTMP_EFUSE_SUPPORT \
	-DNEW_MBSSID_MODE \
	-DCONFIG_ANDES_SUPPORT \
	-DDOT11_N_SUPPORT \
	-DDOT11_VHT_AC \
	-DNEW_RATE_ADAPT_SUPPORT \
	-DSTA_DRIVER_BUILD="20141022-1327"


MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	$(EXTRA_KCONFIG) \
	PROJECT_DIR="$(PKG_BUILD_DIR)" \
	NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
	LINUX_SRC="$(LINUX_DIR)" \
	LINUX_SRC_MODULE="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)"

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(PKG_BUILD_DIR)" \
		$(MAKE_OPTS)
endef

$(eval $(call KernelPackage,mt7610u))