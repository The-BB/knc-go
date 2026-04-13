# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=snowflake
PKG_VERSION:=2.13.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/snowflake
PKG_MIRROR_HASH:=a993a8fae3ce8ff545615c21f95e134c77b2904c2ff062bd33a9a4683810da18

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/snowflake/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Snowflake
  URL:=https://snowflake.torproject.org/
  PROVIDES:=snowflake
  EXTRA_DEPENDS:=tor-geoip
endef

define Package/snowflake
  $(call Package/snowflake/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/snowflake-hf
  $(call Package/snowflake/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/snowflake-sf
  $(call Package/snowflake/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/snowflake/description
 Snowflake is a system that allows people from all over the world to
 access censored websites and applications. Similar to how VPNs assist
 users in getting around Internet censorship, Snowflake helps you avoid
 being noticed by Internet censors by making your Internet activity appear
 as though you're using the Internet for a regular video or voice call.
endef

Package/snowflake-hf/description=$(Package/snowflake/description)
Package/snowflake-sf/description=$(Package/snowflake/description)

GO_VARS += GO111MODULE=on
GO_TARGET:=./broker ./client ./probetest ./proxy ./server

BINS:=$(subst ./,,$(GO_TARGET))

define Package/snowflake/install
	$(INSTALL_DIR) $(1)/opt/bin
	for bin in $(BINS); do \
		$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/$$$$bin \
			$(1)/opt/bin/snowflake-$$$$bin; \
	done
endef

Package/snowflake-hf/install=$(Package/snowflake/install)
Package/snowflake-sf/install=$(Package/snowflake/install)

$(eval $(call BuildPackage,snowflake))
$(eval $(call BuildPackage,snowflake-hf))
$(eval $(call BuildPackage,snowflake-sf))
