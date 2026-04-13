# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=fzf
PKG_VERSION:=0.71.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=62899fd74eb4f5a12ca973d30fb356431d68f9c2
PKG_SOURCE_URL:=https://github.com/junegunn/fzf
PKG_MIRROR_HASH:=16b1ea1effce1ee5e9fbc46b963e62396525e6cbf89b25f043ba57d00a8c48cc

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/fzf/Default
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=A command-line fuzzy finder
  URL:=https://github.com/junegunn/fzf
  PROVIDES:=fzf
endef

define Package/fzf
  $(call Package/fzf/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/fzf-hf
  $(call Package/fzf/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/fzf-sf
  $(call Package/fzf/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/fzf/description
  fzf is a general-purpose command-line fuzzy finder.
endef

Package/fzf-hf/description=$(Package/fzf/description)
Package/fzf-sf/description=$(Package/fzf/description)

GO_LDFLAGS += \
	-X 'main.version=$(PKG_VERSION)' \
	-X 'main.revision=$(COMMIT_SHORT)'

define Package/fzf/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/fzf $(1)/opt/sbin
endef

Package/fzf-hf/install=$(Package/fzf/install)
Package/fzf-sf/install=$(Package/fzf/install)

$(eval $(call BuildPackage,fzf))
$(eval $(call BuildPackage,fzf-hf))
$(eval $(call BuildPackage,fzf-sf))
