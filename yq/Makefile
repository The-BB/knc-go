# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=yq
PKG_VERSION:=4.52.5
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/mikefarah/yq
PKG_MIRROR_HASH:=2a098c4546861b95b6d2cd96dc63c1db66eb5db9a8ac128af3fa7fa674a34650

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/yq/Default
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=a portable command-line data file processor
  URL:=https://mikefarah.gitbook.io/yq
  PROVIDES:=yq
endef

define Package/yq
  $(call Package/yq/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/yq-hf
  $(call Package/yq/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/yq-sf
  $(call Package/yq/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/yq/description
 yq is a portable command-line YAML, JSON, XML, CSV, TOML, HCL and properties
 processor.
endef

Package/yq-hf/description=$(Package/yq/description)
Package/yq-sf/description=$(Package/yq/description)

define Package/yq/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/yq $(1)/opt/sbin
endef

Package/yq-hf/install=$(Package/yq/install)
Package/yq-sf/install=$(Package/yq/install)

$(eval $(call BuildPackage,yq))
$(eval $(call BuildPackage,yq-hf))
$(eval $(call BuildPackage,yq-sf))
