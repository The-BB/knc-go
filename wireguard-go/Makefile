# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2011-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=wireguard-go
PKG_VERSION:=0.0.20250522
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_URL:=https://git.zx2c4.com/wireguard-go
PKG_MIRROR_HASH:=3d5a2710f6f139f9b96030b9db5c531321eaddfe37dd95ec764d7587110bb69a

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/wireguard-go/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=This is an implementation of WireGuard in Go
  URL:=https://git.zx2c4.com/wireguard-go/about/
  PROVIDES:=wireguard-go
  EXTRA_DEPENDS:=wg-quick, wireguard-tools
endef

define Package/wireguard-go
  $(call Package/wireguard-go/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/wireguard-go-hf
  $(call Package/wireguard-go/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/wireguard-go-sf
  $(call Package/wireguard-go/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/wireguard-go/description
 This is an implementation of WireGuard in Go.
endef

Package/wireguard-go-hf/description=$(Package/wireguard-go/description)
Package/wireguard-go-sf/description=$(Package/wireguard-go/description)

define Package/wireguard-go/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/wireguard $(1)/opt/bin/wireguard-go
endef

Package/wireguard-go-hf/install=$(Package/wireguard-go/install)
Package/wireguard-go-sf/install=$(Package/wireguard-go/install)

$(eval $(call BuildPackage,wireguard-go))
$(eval $(call BuildPackage,wireguard-go-hf))
$(eval $(call BuildPackage,wireguard-go-sf))
