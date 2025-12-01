# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=kopia
PKG_VERSION:=0.22.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=e456f78fa2d15b102988eee1025a2451eeaa3ebf
PKG_SOURCE_URL:=https://github.com/kopia/kopia
PKG_MIRROR_HASH:=df537f56502143aea3449fa3d4311f6e923c8f9435f840e881d1a4235b10274d

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/kopia/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Backup
  TITLE:=backup/restore tool
  URL:=https://kopia.io/
  PROVIDES:=kopia
endef

define Package/kopia
  $(call Package/kopia/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/kopia-hf
  $(call Package/kopia/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/kopia-sf
  $(call Package/kopia/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/kopia/description
  Kopia is a fast and secure open-source backup/restore tool that allows
  you to create encrypted snapshots of your data and save the snapshots
  to remote or cloud storage of your choice, to network-attached storage
  or server, or locally on your machine.
endef

Package/kopia-hf/description=$(Package/kopia/description)
Package/kopia-sf/description=$(Package/kopia/description)

define Package/kopia/conffiles
/opt/etc/kopia/kopia.conf
endef

Package/kopia-hf/conffiles=$(Package/kopia/conffiles)
Package/kopia-sf/conffiles=$(Package/kopia/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/repo.BuildVersion=$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/repo.BuildInfo=$(COMMIT_SHORT)' \
	-X '$(XIMPORTPATH)/repo.BuildGitHubRepo=$(PKG_NAME)'

define Package/kopia/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/kopia $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/kopia
	$(INSTALL_DATA) ./files/kopia.conf $(1)/opt/etc/kopia
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99kopia $(1)/opt/etc/init.d
endef

Package/kopia-hf/install=$(Package/kopia/install)
Package/kopia-sf/install=$(Package/kopia/install)

$(eval $(call BuildPackage,kopia))
$(eval $(call BuildPackage,kopia-hf))
$(eval $(call BuildPackage,kopia-sf))
