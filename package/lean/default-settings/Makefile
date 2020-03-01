#
# Copyright (C) 2016-2017 GitHub 
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk
OPENWRT_VERSION:= r190702-$(shell git log --pretty=format:'%h' -n 1|sed -e "s,\([0-9a-z]\{7\}\)[0-9a-z]*,\1,")
PKG_NAME:=default-settings
PKG_VERSION:=1.1
PKG_RELEASE:=50
PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/default-settings
  SECTION:=luci
  CATEGORY:=LuCI
  TITLE:=LuCI support for Default Settings
  PKGARCH:=all
  DEPENDS:=+luci-base +@LUCI_LANG_zh-cn
endef

define Package/default-settings/description
	Language Support Packages.
endef

define Build/Prepare
	$(CP) ./files/zzz-default-settings $(PKG_BUILD_DIR)/
	$(foreach po,$(wildcard ${CURDIR}/i18n/*.po), \
		po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)
	sed -i 's/MODIFY_VERSION/$(OPENWRT_VERSION) by Mleaf/g' $(PKG_BUILD_DIR)/zzz-default-settings
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/default-settings/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.lmo $(1)/usr/lib/lua/luci/i18n/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zzz-default-settings $(1)/etc/uci-defaults/99-default-settings
endef

$(eval $(call BuildPackage,default-settings))
