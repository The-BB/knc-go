# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2020-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=nebula
PKG_VERSION:=1.9.7
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/slackhq/nebula
PKG_MIRROR_HASH:=e213664b857bb3be216c6f34faf3022e4e49ca1d6d14c8b514e3398b9daed1f3

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/nebula/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=a scalable overlay networking tool 
  URL:=https://github.com/slackhq/nebula
  PROVIDES:=nebula
endef

define Package/nebula
  $(call Package/nebula/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/nebula-hf
  $(call Package/nebula/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/nebula-sf
  $(call Package/nebula/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/nebula/description
 Nebula is a scalable overlay networking tool with a focus on performance,
 simplicity and security. It lets you seamlessly connect computers anywhere
 in the world. Nebula is portable, and runs on Linux, OSX, and Windows. It
 can be used to connect a small number of computers, but is also able to
 connect tens of thousands of computers.
endef

Package/nebula-hf/description=$(Package/nebula/description)
Package/nebula-sf/description=$(Package/nebula/description)

GO_LDFLAGS += \
	-X 'main.Build=$(PKG_VERSION)'

GO_VARS += GO111MODULE=on

GO_TARGET:=./cmd/nebula ./cmd/nebula-cert

define Package/nebula/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/nebula{,-cert} $(1)/opt/bin
endef

Package/nebula-hf/install=$(Package/nebula/install)
Package/nebula-sf/install=$(Package/nebula/install)

$(eval $(call BuildPackage,nebula))
$(eval $(call BuildPackage,nebula-hf))
$(eval $(call BuildPackage,nebula-sf))
