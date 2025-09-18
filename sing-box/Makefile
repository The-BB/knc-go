# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=sing-box
PKG_VERSION:=1.12.8
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/SagerNet/sing-box
PKG_MIRROR_HASH:=e21634b08f473a1d715e359745f2f778c43342b8108d9eb038c4f5195cd11c94

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/sing-box/Default
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=The universal proxy platform
	URL:=https://sing-box.sagernet.org
	MAINTAINER:=Entware team, https://entware.net
endef

define Package/sing-box
	$(call Package/sing-box/Default)
	VARIANT:=hf
	PROVIDES:=sing-box-go
endef

define Package/sing-box_nohf
	$(call Package/sing-box/Default)
	VARIANT:=nohf
	PROVIDES:=sing-box-go_nohf
	DEPENDS:=@arm
endef

define Package/sing-box/description
 Sing-box is a fast, customizable and univsersal proxy platform that
can be used to create network proxy servers, clients and transparent proxies.
endef

Package/sing-box_nohf/description=$(Package/sing-box/description)

define Package/sing-box/conffiles
/opt/etc/sing-box/config.json
endef

Package/sing-box_nohf/conffiles=$(Package/sing-box/conffiles)

XIMPORTPATH:=$(call tolower,$(XIMPORTPATH))

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/constant.Version=$(PKG_VERSION)'

GO_TARGET:=./cmd/sing-box

GO_TAGS:= \
	with_acme, \
	with_clash_api, \
	with_dhcp, \
	with_embedded_tor, \
	with_grpc, \
	with_gvisor, \
	with_quic, \
	with_tailscale, \
	with_utls, \
	with_v2ray_api, \
	with_wireguard

define Package/sing-box/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/sing-box $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/sing-box
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release/config/config.json $(1)/opt/etc/sing-box
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99sing-box $(1)/opt/etc/init.d
endef

Package/sing-box_nohf/install=$(Package/sing-box/install)

$(eval $(call BuildPackage,sing-box))
$(eval $(call BuildPackage,sing-box_nohf))
