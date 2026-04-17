# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=sing-box
PKG_VERSION:=1.13.8
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/SagerNet/sing-box
PKG_MIRROR_HASH:=bebc9be8791eaafe9857c869f7e78244bfefb2ccfd92c2189cf6210c414825e6

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1
PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/sing-box/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=The universal proxy platform
  URL:=https://sing-box.sagernet.org
  PROVIDES:=sing-box
  DEPENDS:=+libatomic +libc +libgcc +libpthread +librt
endef

define Package/sing-box
  $(call Package/sing-box/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/sing-box-hf
  $(call Package/sing-box/Default)
  VARIANT:=hf
  DEPENDS+=@(mips||mipsel)
endef

define Package/sing-box-sf
  $(call Package/sing-box/Default)
  VARIANT:=sf
  DEPENDS+=@arm
endef

define Package/sing-box/description
 Sing-box is a fast, customizable and univsersal proxy platform that
can be used to create network proxy servers, clients and transparent proxies.
endef

Package/sing-box-hf/description=$(Package/sing-box/description)
Package/sing-box-sf/description=$(Package/sing-box/description)

define Package/sing-box/conffiles
/opt/etc/sing-box/config.json
endef

Package/sing-box-hf/conffiles=$(Package/sing-box/conffiles)
Package/sing-box-sf/conffiles=$(Package/sing-box/conffiles)

XIMPORTPATH:=$(call tolower,$(XIMPORTPATH))

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/constant.Version=$(PKG_VERSION)' \
	-X 'internal/godebug.defaultGODEBUG=multipathtcp=0' \
	-checklinkname=0

GO_TARGET:=./cmd/sing-box

ifneq ($(or $(CONFIG_mips),$(CONFIG_mipsel)),y)
  GO_TAGS_EXT:=,with_naive_outbound,with_purego
endif

GO_TAGS:= \
	badlinkname, \
	tfogo_checklinkname0, \
	with_acme, \
	with_ccm, \
	with_clash_api, \
	with_dhcp, \
	with_embedded_tor, \
	with_grpc, \
	with_gvisor, \
	with_ocm, \
	with_quic, \
	with_tailscale, \
	with_utls, \
	with_v2ray_api, \
	with_wireguard \
	$(GO_TAGS_EXT)

define Package/sing-box/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/sing-box $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/sing-box
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release/config/config.json $(1)/opt/etc/sing-box
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99sing-box $(1)/opt/etc/init.d
endef

Package/sing-box-hf/install=$(Package/sing-box/install)
Package/sing-box-sf/install=$(Package/sing-box/install)

$(eval $(call BuildPackage,sing-box))
#$(eval $(call BuildPackage,sing-box-hf))
$(eval $(call BuildPackage,sing-box-sf))
