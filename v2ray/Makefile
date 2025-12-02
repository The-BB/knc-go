# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2021-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=v2ray
PKG_VERSION:=5.42.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-11-22
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/v2fly/v2ray-core
PKG_MIRROR_HASH:=ea062109d0db278ad6f871dc30e7de04e5d798c96943c8cbf86adea62e51f2d6

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/v2ray/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=proxy building platform
  URL:=https://www.v2fly.org
endef

define Package/v2ray-core
  $(call Package/v2ray/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
  EXTRA_DEPENDS:=ca-bundle, v2ray-geoip, v2ray-geosite
endef

define Package/v2ray-core-hf
  $(call Package/v2ray/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
  EXTRA_DEPENDS:=ca-bundle, v2ray-geoip, v2ray-geosite
endef

define Package/v2ray-core-sf
  $(call Package/v2ray/Default)
  VARIANT:=sf
  DEPENDS:=@arm
  EXTRA_DEPENDS:=ca-bundle, v2ray-geoip, v2ray-geosite
endef

define Package/v2ray-core/description
 A platform for building proxies to bypass network restrictions.
endef

Package/v2ray-core-hf/description=$(Package/v2ray-core/description)
Package/v2ray-core-sf/description=$(Package/v2ray-core/description)

define Package/v2ray
  $(call Package/v2ray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=default
  DEFAULT_VARIANT:=1
  DEPENDS:=+v2ray-core
endef

define Package/v2ray-hf
  $(call Package/v2ray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=hf
  DEPENDS:=+v2ray-core-hf
endef

define Package/v2ray-sf
  $(call Package/v2ray/Default)
  TITLE+= (scripts & configs)
  VARIANT:=sf
  DEPENDS:=+v2ray-core-sf
endef

define Package/v2ray/description
  $(call Package/v2ray-core/description)
  (scripts & configs)
endef

Package/v2ray-hf/description=$(Package/v2ray/description)
Package/v2ray-sf/description=$(Package/v2ray/description)

define Package/v2ray/conffiles
/opt/etc/v2ray/config.json
/opt/etc/v2ray/vpoint_socks_vmess.json
/opt/etc/v2ray/vpoint_vmess_freedom.json
endef

Package/v2ray-hf/conffiles=$(Package/v2ray/conffiles)
Package/v2ray-sf/conffiles=$(Package/v2ray/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/v5.codename=Entware' \
	-X '$(XIMPORTPATH)/v5.build=$(PKG_SOURCE_DATE)' \
	-X '$(XIMPORTPATH)/v5.version=$(PKG_VERSION)'

GO_TARGET:=./main

GO_VARS += GO111MODULE=on

define Package/v2ray-core/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/main $(1)/opt/sbin/v2ray
endef

Package/v2ray-core-hf/install=$(Package/v2ray-core/install)
Package/v2ray-core-sf/install=$(Package/v2ray-core/install)

define Package/v2ray/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S24v2ray $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/v2ray
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/release/config/*.json $(1)/opt/etc/v2ray
endef

Package/v2ray-hf/install=$(Package/v2ray/install)
Package/v2ray-sf/install=$(Package/v2ray/install)

$(eval $(call BuildPackage,v2ray-core))
$(eval $(call BuildPackage,v2ray-core-hf))
$(eval $(call BuildPackage,v2ray-core-sf))
$(eval $(call BuildPackage,v2ray))
$(eval $(call BuildPackage,v2ray-hf))
$(eval $(call BuildPackage,v2ray-sf))
