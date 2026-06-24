# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=chezmoi
PKG_VERSION:=2.70.5
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-06-03
PKG_SOURCE_VERSION:=b81bd8daa23126bd5f1b2f787141b2b69439abee
PKG_SOURCE_URL:=https://github.com/twpayne/chezmoi
PKG_MIRROR_HASH:=8006d413a8ceec126e356897aaa901d5121ca83db3127b9f64b0914e069395d3

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/chezmoi/Default
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Manage dotfiles
  URL:=https://www.chezmoi.io
  PROVIDES:=chezmoi
endef

define Package/chezmoi
  $(call Package/chezmoi/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/chezmoi-hf
  $(call Package/chezmoi/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/chezmoi-sf
  $(call Package/chezmoi/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/chezmoi/description
  Manage your dotfiles across multiple diverse machines, securely.
endef

Package/chezmoi-hf/description=$(Package/chezmoi/description)
Package/chezmoi-sf/description=$(Package/chezmoi/description)

GO_LDFLAGS += \
	-X 'main.version=v$(PKG_VERSION)' \
	-X 'main.commit=$(COMMIT_SHORT)' \
	-X 'main.date=$(PKG_SOURCE_DATE)' \
	-X 'main.builtBy=Entware'

GO_TAGS:=noupgrade

define Package/chezmoi/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/chezmoi $(1)/opt/bin
endef

Package/chezmoi-hf/install=$(Package/chezmoi/install)
Package/chezmoi-sf/install=$(Package/chezmoi/install)

$(eval $(call BuildPackage,chezmoi))
$(eval $(call BuildPackage,chezmoi-hf))
$(eval $(call BuildPackage,chezmoi-sf))
