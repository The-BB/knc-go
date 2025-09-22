# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=restic
PKG_VERSION:=0.18.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/restic/restic
PKG_MIRROR_HASH:=7808ee49c2069410fa2440d1eef7c3fd61b26bb1a332d2b793e76155153dd721

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=BSD-2-Clause
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/restic/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Backup
  TITLE:=Fast, secure, efficient backup program
  URL:=https://restic.net
  PROVIDES:=restic
endef

define Package/restic
  $(call Package/restic/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/restic-hf
  $(call Package/restic/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/restic-sf
  $(call Package/restic/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/restic/description
 restic is a backup program that is fast, efficient and secure.
 It supports the three major operating systems (Linux, macOS, Windows) and a
 few smaller ones (FreeBSD, OpenBSD).
endef

Package/restic-hf/description=$(Package/restic/description)
Package/restic-sf/description=$(Package/restic/description)

GO_TAGS:=disable_grpc_modules

GO_TARGET:=./cmd/restic

define Package/restic/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/restic $(1)/opt/bin/
endef

Package/restic-hf/install=$(Package/restic/install)
Package/restic-sf/install=$(Package/restic/install)

$(eval $(call BuildPackage,restic))
$(eval $(call BuildPackage,restic-hf))
$(eval $(call BuildPackage,restic-sf))
