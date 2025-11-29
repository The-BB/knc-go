# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2019-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=adguardhome
PKG_VERSION:=0.107.69
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-10-30
PKG_SOURCE_VERSION:=ee23c0805af699f1ab8268ec87d96a9f3783f4fa
PKG_SOURCE_URL:=https://github.com/AdguardTeam/AdGuardHome
PKG_MIRROR_HASH:=f08cf74a890c471163c06bdd22b257b5bc4a9078147f04292db9afdc6f774b28

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.txt

PKG_BUILD_DEPENDS:=node-yarn/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/adguardhome/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Network-wide ads & trackers blocking DNS server
  URL:=https://adguard.com/adguard-home.html
  MAINTAINER:=Entware team, https://entware.net
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

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/internal/version.channel=release' \
	-X '$(XIMPORTPATH)/internal/version.version=v$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/internal/version.committime=$(PKG_SOURCE_DATE)'

define Build/Compile
	( cd $(PKG_BUILD_DIR); \
		NODE_OPTIONS=--openssl-legacy-provider \
		$(MAKE) js-deps js-build; \
	)
	( cd $(PKG_BUILD_DIR); $(GO_BIN_MOD_TIDY); )
	$(call Build/Compile/Go)
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

$(eval $(call BuildPackage,adguardhome))
$(eval $(call BuildPackage,adguardhome-hf))
$(eval $(call BuildPackage,adguardhome-sf))
