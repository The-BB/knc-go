# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2021-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=xray
PKG_VERSION:=26.2.6
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=12ee51e4bb1d02ece4ef4b7114efa2bcdc130995
PKG_SOURCE_URL:=https://github.com/XTLS/Xray-core
PKG_MIRROR_HASH:=816abc353f1b9597f34b02e006377a0935bea3d6cee358ffbd59c91df5ab1935

PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/xray/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=a personal vpn server
  URL:=https://github.com/XTLS/Xray-core
  EXTRA_DEPENDS:=ca-bundle
endef

define Package/xray-core
  $(call Package/xray/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/xray-core-hf
  $(call Package/xray/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/xray-core-sf
  $(call Package/xray/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/xray-core/description
 Project X originates from XTLS protocol, providing a set of network tools.
endef

Package/xray-core-hf/description=$(Package/xray-core/description)
Package/xray-core-sf/description=$(Package/xray-core/description)

define Package/xray
  $(call Package/xray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=default
  DEFAULT_VARIANT:=1
  DEPENDS:=+xray-core
endef

define Package/xray-hf
  $(call Package/xray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=hf
  DEPENDS:=+xray-core-hf
endef

define Package/xray-sf
  $(call Package/xray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=sf
  DEPENDS:=+xray-core-sf
endef

define Package/xray/description
  $(call Package/xray-core/description)
  (scripts & configs)
endef

Package/xray-hf/description=$(Package/xray/description)
Package/xray-sf/description=$(Package/xray/description)

define Package/xray/conffiles
/opt/etc/xray/config.json
endef

Package/xray-hf/conffiles=$(Package/xray/conffiles)
Package/xray-sf/conffiles=$(Package/xray/conffiles)

XIMPORTPATH:=$(call tolower,$(XIMPORTPATH))

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/core.build=$(COMMIT_SHORT)'

GO_TARGET:=./main

define Package/xray-core/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/main $(1)/opt/sbin/xray
endef

Package/xray-core-hf/install=$(Package/xray-core/install)
Package/xray-core-sf/install=$(Package/xray-core/install)

define Package/xray/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S24xray $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/xray
	$(INSTALL_CONF) ./files/config.json.example $(1)/opt/etc/xray
endef

Package/xray-hf/install=$(Package/xray/install)
Package/xray-sf/install=$(Package/xray/install)

$(eval $(call BuildPackage,xray-core))
$(eval $(call BuildPackage,xray-core-hf))
$(eval $(call BuildPackage,xray-core-sf))
$(eval $(call BuildPackage,xray))
$(eval $(call BuildPackage,xray-hf))
$(eval $(call BuildPackage,xray-sf))
