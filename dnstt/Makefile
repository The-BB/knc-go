# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=dnstt
PKG_VERSION:=1.20241021.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://repo.or.cz/dnstt
PKG_MIRROR_HASH:=a8af62cbcf01a85e0d40c550b28fca8a680ba2058e9db1cec9c1632e5f97b173

PKG_LICENSE:=CC0-1.0
PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/dnstt/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  TITLE:=Userspace DNS tunnel with support for DoH and DoT
  URL:=https://www.bamsoftware.com/software/dnstt/
endef

define Package/dnstt-client
  $(call Package/dnstt/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/dnstt-client-hf
  $(call Package/dnstt/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/dnstt-client-sf
  $(call Package/dnstt/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/dnstt-server
  $(call Package/dnstt/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/dnstt-server-hf
  $(call Package/dnstt/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/dnstt-server-sf
  $(call Package/dnstt/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/dnstt/description
 dnstt is a DNS tunnel that can use DNS over HTTPS (DoH) and DNS
 over TLS (DoT) resolvers.
endef

Package/dnstt-client/description=$(Package/dnstt/description)
Package/dnstt-client-hf/description=$(Package/dnstt/description)
Package/dnstt-client-sf/description=$(Package/dnstt/description)
Package/dnstt-server/description=$(Package/dnstt/description)
Package/dnstt-server-hf/description=$(Package/dnstt/description)
Package/dnstt-server-sf/description=$(Package/dnstt/description)

GO_TARGET:=./dnstt-client ./dnstt-server

define Package/dnstt-client/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/dnstt-client $(1)/opt/sbin
endef

Package/dnstt-client-hf/install=$(Package/dnstt-client/install)
Package/dnstt-client-sf/install=$(Package/dnstt-client/install)

define Package/dnstt-server/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/dnstt-server $(1)/opt/sbin
endef

Package/dnstt-server-hf/install=$(Package/dnstt-server/install)
Package/dnstt-server-sf/install=$(Package/dnstt-server/install)

$(eval $(call BuildPackage,dnstt-client))
$(eval $(call BuildPackage,dnstt-client-hf))
$(eval $(call BuildPackage,dnstt-client-sf))
$(eval $(call BuildPackage,dnstt-server))
$(eval $(call BuildPackage,dnstt-server-hf))
$(eval $(call BuildPackage,dnstt-server-sf))
