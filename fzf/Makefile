# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=fzf
PKG_VERSION:=0.65.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=416aff86e9d550d0a34af9d3cc12dfede25354c3
PKG_SOURCE_URL:=https://github.com/junegunn/fzf
PKG_MIRROR_HASH:=780a8b15ae0ea1c2372b82c790ee6715871117e87ad0babace6b7f7c34eb4dd6

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
