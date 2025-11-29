# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2022-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=alist
PKG_VERSION:=3.55.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-11-11
PKG_SOURCE_VERSION:=3cddb6b7edd77b422b5c9c66462b58538d6c206b
PKG_SOURCE_URL:=https://github.com/AlistGo/alist
PKG_MIRROR_HASH:=74aff41b61c4d9d96ccdc14c7b4e62aa718e7cffff06e484ee3d3a3724f3318c

PKG_LICENSE:=AGPL-3.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1
PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/alist/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Filesystem
  TITLE:=A file list program that supports multiple storage
  URL:=https://alistgo.com
  PROVIDES:=alist
  DEPENDS:=+libc +libgcc +libpthread
endef

define Package/alist
  $(call Package/alist/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/alist-hf
  $(call Package/alist/Default)
  VARIANT:=hf
  DEPENDS+=@(mips||mipsel)
endef

define Package/alist-sf
  $(call Package/alist/Default)
  VARIANT:=sf
  DEPENDS+=@arm
endef

define Package/alist/description
  A file list program that supports multiple storage.
endef

Package/alist-hf/description=$(Package/alist/description)
Package/alist-sf/description=$(Package/alist/description)

WEB_VERSION:=$(PKG_VERSION)
WEB_FILE:=$(PKG_NAME)-web-$(WEB_VERSION).tar.gz
define Download/alist-web
  FILE:=$(WEB_FILE)
  URL_FILE:=dist.tar.gz
  URL:=https://github.com/AlistGo/alist-web/releases/download/$(WEB_VERSION)
  HASH:=1857caa4e64c0c9c2d92daea5953e8c5f2a5576e33e799b172a51e974f16fd6c
endef

GIT_TAG:=v$(PKG_VERSION)-$(COMMIT_SHORT)
GO_VER:=$(shell $(GO_BIN) version | sed 's/go version //;s,/.*,/$(GOARCH),')

XIMPORTPATH:=$(subst AlistGo,alist-org,$(XIMPORTPATH))

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/v3/internal/conf.BuiltAt=$(PKG_SOURCE_DATE)' \
	-X '$(XIMPORTPATH)/v3/internal/conf.GoVersion=$(GO_VER)' \
	-X '$(XIMPORTPATH)/v3/internal/conf.GitAuthor=Entware' \
	-X '$(XIMPORTPATH)/v3/internal/conf.GitCommit=$(COMMIT_SHORT)' \
	-X '$(XIMPORTPATH)/v3/internal/conf.Version=$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/v3/internal/conf.WebVersion=$(WEB_VERSION)'

GO_TAGS:=jsoniter

define Build/Prepare
	$(call Build/Prepare/Default)
	$(HOST_TAR) -xf $(DL_DIR)/$(WEB_FILE) -C $(PKG_BUILD_DIR)/public
endef

define Package/alist/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99alist $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/alist $(1)/opt/sbin
endef

Package/alist-hf/install=$(Package/alist/install)
Package/alist-sf/install=$(Package/alist/install)

$(eval $(call Download,alist-web))
$(eval $(call BuildPackage,alist))
#$(eval $(call BuildPackage,alist-hf))
$(eval $(call BuildPackage,alist-sf))
