# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=gost-go
PKG_VERSION:=3.2.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/go-gost/gost
PKG_MIRROR_HASH:=51ca6595f05626dccfcd893ee18cc083639381d6d8de66bd7f7eb1903ad57435

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/gost-go/Default
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=GO Simple Tunnel
	URL:=https://gost.run
	MAINTAINER:=Entware team, https://entware.net
endef

define Package/gost-go
	$(call Package/gost-go/Default)
	VARIANT:=hf
endef

define Package/gost-go_nohf
	$(call Package/gost-go/Default)
	VARIANT:=nohf
	DEPENDS:=@arm
endef

define Package/gost-go/description
  GO Simple Tunnel - a simple tunnel written in golang.
endef

Package/gost-go_nohf/description=$(Package/gost-go/description)

GO_TARGET:=./cmd/gost

define Package/gost-go/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/gost $(1)/opt/bin/
endef

Package/gost-go_nohf/install=$(Package/gost-go/install)

$(eval $(call BuildPackage,gost-go))
$(eval $(call BuildPackage,gost-go_nohf))
