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
	MAINTAINER:=Entware team, https://entware.net
endef

define Package/frp
	$(call Package/frp/Default)
	VARIANT:=hf
	PROVIDES:=frp-go
endef

define Package/frp_nohf
	$(call Package/frp/Default)
	VARIANT:=nohf
	PROVIDES:=frp-go_nohf
	DEPENDS:=@arm
endef

define Package/frp/description
 frp is a fast reverse proxy that allows you to expose
 a local server located behind a NAT or firewall to the Internet.
 It currently supports TCP and UDP, as well as HTTP and HTTPS protocols,
 enabling requests to be forwarded to internal services via domain name.
 frp also offers a P2P connect mode.
endef

Package/frp_nohf/description=$(Package/frp/description)

define Package/frp/conffiles
/opt/etc/frp/
endef

Package/frp_nohf/conffiles=$(Package/frp/conffiles)

GO_VARS += GO111MODULE=on

GO_TAGS:=frpc, frps
GO_TARGET:=./cmd/frpc ./cmd/frps

define Package/frp/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/frp{c,s} $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/frp
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/*.toml $(1)/opt/etc/frp
endef

Package/frp_nohf/install=$(Package/frp/install)

$(eval $(call BuildPackage,frp))
$(eval $(call BuildPackage,frp_nohf))
