# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=chezmoi
PKG_VERSION:=2.67.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-11-27
PKG_SOURCE_VERSION:=5cda5a8a118f2eff0c786c3d45c82669735a346b
PKG_SOURCE_URL:=https://github.com/twpayne/chezmoi
PKG_MIRROR_HASH:=00e996a9181b82ae87cedd43f9abf16ff50b98099f2f206ae0782c866402829a

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
