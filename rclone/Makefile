# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2016-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=rclone
PKG_VERSION:=1.72.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/rclone/rclone
PKG_MIRROR_HASH:=42ea9dde88c2c0163b573efabb51c67d308ef32848387be05fa69486fd152612

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/rclone/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=File Transfer
  TITLE:=rsync for cloud storage
  URL:=https://rclone.org
  PROVIDES:=rclone
endef

define Package/rclone
  $(call Package/rclone/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/rclone-hf
  $(call Package/rclone/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/rclone-sf
  $(call Package/rclone/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/rclone/description
 Rclone is a command line program to sync files and directories to and from:
    Google Drive
    Amazon S3
    Openstack Swift / Rackspace cloud files / Memset Memstore
    Dropbox
    Google Cloud Storage
    Amazon Cloud Drive
    Microsoft One Drive
    Hubic
    Backblaze B2
    Yandex Disk
    The local filesystem
endef

Package/rclone-hf/description=$(Package/rclone/description)
Package/rclone-sf/description=$(Package/rclone/description)

define Build/Prepare
	$(call Build/Prepare/Default)
  ifeq ($(or $(CONFIG_mips),$(CONFIG_mipsel)),y)
	$(SED) '/mount2/d' $(PKG_BUILD_DIR)/cmd/all/all.go
  endif
endef

GO_LDFLAGS += \
        -X '$(XIMPORTPATH)/fs.Version=$(PKG_VERSION)'

GO_TAGS:=noselfupdate
GO_VARS += GO111MODULE=on

define Package/rclone/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/$(PKG_NAME) $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/var/cache/rclone
endef

Package/rclone-hf/install=$(Package/rclone/install)
Package/rclone-sf/install=$(Package/rclone/install)

$(eval $(call BuildPackage,rclone))
$(eval $(call BuildPackage,rclone-hf))
$(eval $(call BuildPackage,rclone-sf))
