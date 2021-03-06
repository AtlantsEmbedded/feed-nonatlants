#
# Copyright (C) 2007-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=i2c-tools-clone
PKG_VERSION:=2013-12-15
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/groeck/i2c-tools
PKG_SOURCE_VERSION:=9726bed900c1e4a46b2c024a43dec987215570be
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=PACKAGE_python-smbus:python

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/i2c-tools-clone/Default
  URL:=http://lm-sensors.org/wiki/I2CTools
  TITLE:=i2c-tools-clone
endef

define Package/libi2c
  $(call Package/i2c-tools-clone/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE+=library
endef

define Package/i2c-tools-clone
  $(call Package/i2c-tools-clone/Default)
  SECTION:=utils
  CATEGORY:=Atlants-GPL
  SUBMENU:=Utilities
  TITLE+=tools for Linux
  DEPENDS:=+libi2c
endef

define Package/python-smbus
  $(call Package/i2c-tools-clone/Default)
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Python bindings for the SMBUS
  DEPENDS:=+PACKAGE_python-smbus:python-mini +i2c-tools-clone
endef

define Package/i2c-tools-clone/description
 This package contains an heterogeneous set of I2C tools for Linux. These tools
 were originally part of the lm-sensors package.  It is a clone of the lm-sensor's I2C package
 as the website and GIT dissappeared.
endef

define Package/python-smbus/description
 This package contain the python bindings for Linux SMBus access through i2c-dev.
endef

TARGET_CPPFLAGS += -I$(PKG_BUILD_DIR)/include

ifdef CONFIG_PACKAGE_python-smbus
  define Build/Compile/python-smbus
	$(if $(Build/Compile/PyMod),,@echo Python packaging code not found.; false)
	$(call Build/Compile/PyMod,./py-smbus/, \
		install --prefix="$(PKG_INSTALL_DIR)/usr", \
	)
  endef
endif

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		LINUX="$(LINUX_DIR)" \
		CC="$(TARGET_CC)" \
		STAGING_DIR="$(STAGING_DIR)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		CFLAGS="$(TARGET_CFLAGS)"
	$(Build/Compile/python-smbus)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/lib/libi2c.so* $(1)/usr/lib/
endef

define Package/libi2c/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/lib/libi2c.so* $(1)/usr/lib/
endef

define Package/i2c-tools-clone/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tools/i2cdetect $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tools/i2cdump $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tools/i2cset $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tools/i2cget $(1)/usr/sbin/
endef

define PyPackage/python-smbus/filespec
+|$(PYTHON_PKG_DIR)/smbus.so
endef

$(eval $(call BuildPackage,libi2c))
$(eval $(call BuildPackage,i2c-tools-clone))
$(eval $(call PyPackage,python-smbus))
$(eval $(call BuildPackage,python-smbus))
