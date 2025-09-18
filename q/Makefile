# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025 Entware

# https://github.com/Entware/Entware/issues/1128

include $(TOPDIR)/rules.mk

PKG_NAME:=q
PKG_VERSION:=0.19.8
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-09-07
PKG_SOURCE_VERSION:=7ddddd3c97994c59fe282f5c9dad346be34b328b
PKG_SOURCE_URL:=https://github.com/natesales/q
PKG_MIRROR_HASH:=b7e295a689c4371a6e4e9878a2260c975701a9e0408ea27e1d8e6ed3f48a35ff

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/q/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  TITLE:=command line DNS client
  URL:=https://github.com/natesales/q
endef

define Package/q
  $(call Package/q/Default)
  VARIANT:=hf
endef

define Package/q_nohf
  $(call Package/q/Default)
  VARIANT:=nohf
  DEPENDS:=@arm
endef

define Package/q/description
  A tiny CLI DNS client library with support for UDP, TCP, DoT, DoH,
 and DoQ.
endef

Package/q_nohf/description=$(Package/q/description)

GO_LDFLAGS += \
	-X 'main.version=$(PKG_VERSION)' \
	-X 'main.commit=$(COMMIT_SHORT)' \
	-X 'main.date=$(PKG_SOURCE_DATE)'

define Package/q/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/q $(1)/opt/sbin
endef

Package/q_nohf/install=$(Package/q/install)

$(eval $(call BuildPackage,q))
$(eval $(call BuildPackage,q_nohf))
