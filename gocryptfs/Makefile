# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=gocryptfs
PKG_VERSION:=2.6.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-07-14
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/rfjakob/gocryptfs
PKG_MIRROR_HASH:=f3469ac6b8f1019c745d47dbc3e8cfbca158eaa3f055412c9270ec012366d372

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1
PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/gocryptfs/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Encryption
  TITLE:=Encrypted overlay filesystem written in Go
  URL:=https://nuetzlich.net/gocryptfs
  DEPENDS:=+fuse-utils +libopenssl
endef

define Package/gocryptfs
  $(call Package/gocryptfs/Default)
  VARIANT:=hf
endef

define Package/gocryptfs_nohf
  $(call Package/gocryptfs/Default)
  VARIANT:=nohf
  DEPENDS+=@arm
endef

define Package/gocryptfs/description
  An encrypted overlay filesystem written in Go.
endef

Package/gocryptfs_nohf/description=$(Package/gocryptfs/description)

# go list -m github.com/hanwen/go-fuse/v2 | cut -d' ' -f2-

GO_LDFLAGS += \
	-X 'main.GitVersion=$(PKG_VERSION)' \
	-X 'main.GitVersionFuse=v2.8.0' \
	-X 'main.BuildDate=$(PKG_SOURCE_DATE)'

#GO_TARGET:=./gocryptfs-xray ./contrib/statfs ./contrib/findholes ./contrib/atomicrename

define Package/gocryptfs/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/gocryptfs $(1)/opt/sbin
endef

Package/gocryptfs_nohf/install=$(Package/gocryptfs/install)

$(eval $(call BuildPackage,gocryptfs))
$(eval $(call BuildPackage,gocryptfs_nohf))
