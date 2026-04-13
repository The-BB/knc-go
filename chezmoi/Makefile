# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=chezmoi
PKG_VERSION:=2.70.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-04-08
PKG_SOURCE_VERSION:=420cfdc935590cf62626d347a2a9ec39b2691fc1
PKG_SOURCE_URL:=https://github.com/twpayne/chezmoi
PKG_MIRROR_HASH:=7b769ef642919217ebcab40195c5d2e17fc2651e85b3a8d0490532f27fe2030f

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
