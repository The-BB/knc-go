# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=git-lfs
PKG_VERSION:=3.7.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=92dddf560e62ef7dd25877d87ce072f7595aa52d
PKG_SOURCE_URL:=https://github.com/git-lfs/git-lfs
PKG_MIRROR_HASH:=14774b0b3b7dc7dfcd380575c12799ce315c1a1d9f3c7f37b9c32aa688fbf928

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.md

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/git-lfs/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Version Control Systems
  TITLE:=Git Large File Storage
  URL:=https://git-lfs.github.com
  MAINTAINER:=Entware team, https://entware.net
  PROVIDES:=git-lfs
  EXTRA_DEPENDS:=git
endef

define Package/git-lfs
  $(call Package/git-lfs/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/git-lfs-hf
  $(call Package/git-lfs/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/git-lfs-sf
  $(call Package/git-lfs/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/git-lfs/description
  Git Large File Storage (LFS) replaces large files such as audio samples,
  videos, datasets, and graphics with text pointers inside Git, while storing
  the file contents on a remote server like GitHub.com or GitHub Enterprise.
endef

Package/git-lfs-hf/description=$(Package/git-lfs/description)
Package/git-lfs-sf/description=$(Package/git-lfs/description)

define Build/Compile
	( cd $(PKG_BUILD_DIR); $(GO_BIN_GENERATE) ./commands; )
	$(call Build/Compile/Go)
endef

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/v3/config.Vendor=Entware' \
	-X '$(XIMPORTPATH)/v3/config.GitCommit=$(COMMIT_SHORT)'

define Package/git-lfs/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/git-lfs $(1)/opt/bin
endef

Package/git-lfs-hf/install=$(Package/git-lfs/install)
Package/git-lfs-sf/install=$(Package/git-lfs/install)

$(eval $(call BuildPackage,git-lfs))
$(eval $(call BuildPackage,git-lfs-hf))
$(eval $(call BuildPackage,git-lfs-sf))
