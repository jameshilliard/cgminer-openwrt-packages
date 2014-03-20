#
# Author: Xiangfu Liu <xiangfu@openmobilefree.net>
# Address: 12h6gdGnThW385JaX1LRMA8cXKmbYRTP8Q
#
# This is free and unencumbered software released into the public domain.
# For details see the UNLICENSE file at the root of the source tree.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cgminer

ifeq ($(CONFIG_CGMINER_ANTS1),y)
PKG_VERSION:=ants1-4.2.0-b63babba
PKG_REV:=b63babba6aaebebab2de2e056339b85a2d67ea3d
endif

PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.bz2
PKG_SOURCE_URL:=git://github.com/kanoi/cgminer.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/cgminer
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=cgminer
	URL:=https://github.com/kanoi/cgminer
ifeq ($(CONFIG_TARGET_brcm2708_RaspberryPi),y)
	DEPENDS:=+libcurl +libpthread +jansson +udev +libncurses
else
	DEPENDS:=+libcurl +libpthread +jansson +udev
endif
endef

define Package/cgminer/description
Cgminer is a multi-threaded multi-pool ASIC and FPGA miner for bitcoin coins.
endef

define Package/cgminer/config
	menu "Configuration"
	depends on PACKAGE_cgminer
	source "$(SOURCE)/Config.in"
	endmenu
endef

TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

ifeq ($(CONFIG_CGMINER_ANTS1),)
	CONFIGURE_ARGS += --without-curses --enable-ants1
else
	CONFIGURE_ARGS += --enable-ants1


endif

define Build/Compile
	$(call Build/Compile/Default)
	(cd $(PKG_BUILD_DIR) && \
	  $(TARGET_CC) -Icompat/jansson -Icompat/libusb-1.0/libusb \
	  api-example.c -o cgminer-api;)
endef

define Package/cgminer/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cgminer-api	   $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cgminer  $(1)/usr/bin
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-monitor        $(1)/usr/bin

	$(INSTALL_BIN) $(FILES_DIR)/cgminer.init           $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.config         $(1)/etc/config/cgminer

endef

$(eval $(call BuildPackage,cgminer))
