# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2019-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=adguardhome
PKG_VERSION:=0.107.74
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-04-16
PKG_SOURCE_VERSION:=19e30ce89c6eed7aaf2b74128e17b243325e4a8e
PKG_SOURCE_URL:=https://github.com/AdguardTeam/AdGuardHome
PKG_MIRROR_HASH:=451e3976b575d8b379323c60bda0eb290c40dbb13710e3b075b82d735ff80324

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/adguardhome/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Network-wide ads & trackers blocking DNS server
  URL:=https://adguard.com/adguard-home.html
  PROVIDES:=adguardhome
  EXTRA_DEPENDS:=ca-bundle
endef

define Package/adguardhome
  $(call Package/adguardhome/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/adguardhome-hf
  $(call Package/adguardhome/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/adguardhome-sf
  $(call Package/adguardhome/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/adguardhome/description
  AdGuard Home is a network-wide software for blocking ads and tracking.
endef

Package/adguardhome-hf/description=$(Package/adguardhome/description)
Package/adguardhome-sf/description=$(Package/adguardhome/description)

define Package/adguardhome/conffiles
/opt/etc/AdGuardHome/adguardhome.conf
endef

Package/adguardhome-hf/conffiles=$(Package/adguardhome/conffiles)
Package/adguardhome-sf/conffiles=$(Package/adguardhome/conffiles)

AGH_FRONTEND:=$(PKG_NAME)-frontend-$(PKG_VERSION).tar.gz
define Download/adguardhome-frontend
  URL:=$(PKG_SOURCE_URL)/releases/download/v$(PKG_VERSION)/
  URL_FILE:=AdGuardHome_frontend.tar.gz
  FILE:=$(AGH_FRONTEND)
  HASH:=bca5a8a4d777f984d9a6992214dc158cb35c12e6fd6fd61a30f677896aaa1758
endef

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/internal/version.channel=release' \
	-X '$(XIMPORTPATH)/internal/version.version=v$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/internal/version.committime=$(PKG_SOURCE_DATE)'

define Build/Prepare
	$(call Build/Prepare/Default)
	$(TAR) -xf $(DL_DIR)/$(AGH_FRONTEND) --strip-components=1 -C $(PKG_BUILD_DIR)
endef

define Package/adguardhome/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/AdGuardHome $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99adguardhome $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/AdGuardHome
	$(INSTALL_CONF) ./files/adguardhome.conf $(1)/opt/etc/AdGuardHome
endef

Package/adguardhome-hf/install=$(Package/adguardhome/install)
Package/adguardhome-sf/install=$(Package/adguardhome/install)

$(eval $(call Download,adguardhome-frontend))
$(eval $(call BuildPackage,adguardhome))
$(eval $(call BuildPackage,adguardhome-hf))
$(eval $(call BuildPackage,adguardhome-sf))
