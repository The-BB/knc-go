# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2020-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=nextdns
PKG_VERSION:=1.47.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/nextdns/nextdns
PKG_MIRROR_HASH:=031a5e0d635bbfa3fce6409ba782ffd0bd00925dd881c8c5832bb1f78364c3d4

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/nextdns/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  TITLE:=NextDNS CLI Client
  URL:=https://nextdns.io/
  PROVIDES:=nextdns
endef

define Package/nextdns
  $(call Package/nextdns/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/nextdns-hf
  $(call Package/nextdns/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/nextdns-sf
  $(call Package/nextdns/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/nextdns/description
 NextDNS CLI is a DNS53 to DNS-over-HTTPS (DoH) proxy with
 advanced capabilities to get the most out of the NextDNS
 service. Although the most advanced features will only work
 with NextDNS, this program can work as a client for any DoH
 provider or a mix of NextDNS + another DNS (split horizon).
endef

Package/nextdns-hf/description=$(Package/nextdns/description)
Package/nextdns-sf/description=$(Package/nextdns/description)

GO_LDFLAGS += \
	-X 'main.Build=v$(PKG_VERSION)'

GO_VARS += GO111MODULE=on

define Package/nextdns/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/nextdns $(1)/opt/bin
endef

Package/nextdns-hf/install=$(Package/nextdns/install)
Package/nextdns-sf/install=$(Package/nextdns/install)

$(eval $(call BuildPackage,nextdns))
$(eval $(call BuildPackage,nextdns-hf))
$(eval $(call BuildPackage,nextdns-sf))
