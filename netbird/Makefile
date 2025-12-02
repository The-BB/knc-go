# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=netbird
PKG_VERSION:=0.60.4
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-11-28
PKG_SOURCE_VERSION:=cb83b7c0d3ddbacdd6ff391bdcc405f8b2ad00f0
PKG_SOURCE_URL:=https://github.com/netbirdio/netbird
PKG_MIRROR_HASH:=6129f188753a98afdc2dcb1901800a1c25671b83e960c3d3700188f34fe92bb7

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1
# for management (and signal?)
#PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/netbird/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=A WireGuard-based mesh network (client)
  URL:=https://netbird.io/
  PROVIDES:=netbird
endef

define Package/netbird
  $(call Package/netbird/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/netbird-hf
  $(call Package/netbird/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/netbird-sf
  $(call Package/netbird/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/netbird/description
  Connect your devices into a single secure private WireGuard®-based
  mesh network with SSO/MFA and simple access controls.
endef

Package/netbird-hf/description=$(Package/netbird/description)
Package/netbird-sf/description=$(Package/netbird/description)

define Package/netbird/conffiles
/opt/etc/netbird/config.json
/opt/etc/netbird/env
endef

Package/netbird-hf/conffiles=$(Package/netbird/conffiles)
Package/netbird-sf/conffiles=$(Package/netbird/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/version.version=$(PKG_VERSION)' \
	-X 'main.commit=$(COMMIT_SHORT)' \
	-X 'main.date=$(PKG_SOURCE_DATE)' \
	-X 'main.builtBy=Entware'

GO_TARGET:=./client ./management ./signal

define Package/netbird/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99netbird $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/netbird
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release_files/systemd/env \
		 $(1)/opt/etc/netbird
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/client $(1)/opt/sbin/netbird
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/management $(1)/opt/sbin/netbird-mgmt
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/signal $(1)/opt/sbin/netbird-signal
endef

Package/netbird-hf/install=$(Package/netbird/install)
Package/netbird-sf/install=$(Package/netbird/install)

$(eval $(call BuildPackage,netbird))
$(eval $(call BuildPackage,netbird-hf))
$(eval $(call BuildPackage,netbird-sf))
