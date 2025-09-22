# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=frp
PKG_VERSION:=0.64.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/fatedier/frp
PKG_MIRROR_HASH:=bf63cfdc1a34aca9e24408ab7ff5ab1e0fe985334c4454ba066ec31dff286486

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/frp/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Routing and Redirection
  TITLE:=fast reverse proxy
  URL:=https://github.com/fatedier/frp
  PROVIDES:=frp
endef

define Package/frp
  $(call Package/frp/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/frp-hf
  $(call Package/frp/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/frp-sf
  $(call Package/frp/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/frp/description
 frp is a fast reverse proxy that allows you to expose
 a local server located behind a NAT or firewall to the Internet.
 It currently supports TCP and UDP, as well as HTTP and HTTPS protocols,
 enabling requests to be forwarded to internal services via domain name.
 frp also offers a P2P connect mode.
endef

Package/frp-hf/description=$(Package/frp/description)
Package/frp-sf/description=$(Package/frp/description)

define Package/frp/conffiles
/opt/etc/frp/
endef

Package/frp-hf/conffiles=$(Package/frp/conffiles)
Package/frp-sf/conffiles=$(Package/frp/conffiles)

GO_VARS += GO111MODULE=on

GO_TAGS:=frpc, frps
GO_TARGET:=./cmd/frpc ./cmd/frps

define Package/frp/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/frp{c,s} $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/frp
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/*.toml $(1)/opt/etc/frp
endef

Package/frp-hf/install=$(Package/frp/install)
Package/frp-sf/install=$(Package/frp/install)

$(eval $(call BuildPackage,frp))
$(eval $(call BuildPackage,frp-hf))
$(eval $(call BuildPackage,frp-sf))
