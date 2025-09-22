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

PKG_MAINTAINER:=Entware team, https://entware.net
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
  PROVIDES:=gost-go
endef

define Package/gost-go
  $(call Package/gost-go/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/gost-go-hf
  $(call Package/gost-go/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/gost-go-sf
  $(call Package/gost-go/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/gost-go/description
  GO Simple Tunnel - a simple tunnel written in golang.
endef

Package/gost-go-hf/description=$(Package/gost-go/description)
Package/gost-go-sf/description=$(Package/gost-go/description)

GO_TARGET:=./cmd/gost

define Package/gost-go/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/gost $(1)/opt/bin/
endef

Package/gost-go-hf/install=$(Package/gost-go/install)
Package/gost-go-sf/install=$(Package/gost-go/install)

$(eval $(call BuildPackage,gost-go))
$(eval $(call BuildPackage,gost-go-hf))
$(eval $(call BuildPackage,gost-go-sf))
