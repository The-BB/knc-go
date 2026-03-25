# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=dnslookup
PKG_VERSION:=1.11.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/ameshkov/dnslookup
PKG_MIRROR_HASH:=bca76de79c50d689b576e8d43e87d828a98bd8a64f23af43a588fe824b0d39e0

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/dnslookup/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  TITLE:=Simple command line utility to make DNS lookups
  URL:=https://github.com/ameshkov/dnslookup
  PROVIDES:=dnslookup
  EXTRA_DEPENDS:=ca-bundle
endef

define Package/dnslookup
  $(call Package/dnslookup/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/dnslookup-hf
  $(call Package/dnslookup/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/dnslookup-sf
  $(call Package/dnslookup/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/dnslookup/description
 Simple command line utility to make DNS lookups.
 Supports all known DNS protocols: plain DNS, plain DNS-over-TCP, DoH, DoT, DoQ,
 DNSCrypt.
endef

Package/dnslookup-hf/description=$(Package/dnslookup/description)
Package/dnslookup-sf/description=$(Package/dnslookup/description)

GO_LDFLAGS += \
	-X 'main.VersionString=$(PKG_SOURCE_VERSION)'

define Package/dnslookup/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/dnslookup $(1)/opt/sbin
endef

Package/dnslookup-hf/install=$(Package/dnslookup/install)
Package/dnslookup-sf/install=$(Package/dnslookup/install)

$(eval $(call BuildPackage,dnslookup))
$(eval $(call BuildPackage,dnslookup-hf))
$(eval $(call BuildPackage,dnslookup-sf))
