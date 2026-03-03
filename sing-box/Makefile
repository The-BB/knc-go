# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=sing-box
PKG_VERSION:=1.11.15
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/SagerNet/sing-box
PKG_MIRROR_HASH:=7416c399bd4d7ae698bd0f804cf3aec8e1792cdc0b3960e54c1c726b6754b425

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/sing-box-go/Default
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=The universal proxy platform
	URL:=https://sing-box.sagernet.org
	MAINTAINER:=Entware team, https://entware.net
endef

define Package/sing-box-go
	$(call Package/sing-box-go/Default)
	VARIANT:=hf
endef

define Package/sing-box-go_nohf
	$(call Package/sing-box-go/Default)
	VARIANT:=nohf
	DEPENDS:=@arm
endef

define Package/sing-box-go/description
 Sing-box is a fast, customizable and univsersal proxy platform that
can be used to create network proxy servers, clients and transparent proxies.
endef

Package/sing-box-go_nohf/description=$(Package/sing-box-go/description)

define Package/sing-box-go/conffiles
/opt/etc/sing-box/config.json
endef

Package/sing-box-go_nohf/conffiles=$(Package/sing-box-go/conffiles)

XIMPORTPATH:=$(call tolower,$(XIMPORTPATH))

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/constant.Version=$(PKG_VERSION)'

GO_TARGET:=./cmd/sing-box

GO_TAGS:= \
	with_acme, \
	with_clash_api, \
	with_dhcp, \
	with_ech, \
	with_embedded_tor, \
	with_grpc, \
	with_gvisor, \
	with_quic, \
	with_reality_server, \
	with_utls, \
	with_v2ray_api, \
	with_wireguard

define Package/sing-box-go/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/sing-box $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/sing-box
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release/config/config.json $(1)/opt/etc/sing-box
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99sing-box $(1)/opt/etc/init.d
endef

Package/sing-box-go_nohf/install=$(Package/sing-box-go/install)

$(eval $(call BuildPackage,sing-box-go))
$(eval $(call BuildPackage,sing-box-go_nohf))
