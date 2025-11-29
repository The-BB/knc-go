# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2022-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=alist
PKG_VERSION:=3.53.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-09-12
PKG_SOURCE_VERSION:=e1800f18e4e3746d7792a9064fa2fc0a7fef2b8e
PKG_SOURCE_URL:=https://github.com/AlistGo/alist
PKG_MIRROR_HASH:=92da7fb1e622ca33b634b2b845207cafa5c787b91c2e7e92629a50155fdff57a

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
  HASH:=755459065b69ceb353d46c5f51fa79c3ddefacdbd3a08894311190cebe8bb99e
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
