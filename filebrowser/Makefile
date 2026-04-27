# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=filebrowser
PKG_VERSION:=2.63.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=7970c26cbcb8c7dc8abf12f95fe2b97d6c89d577
PKG_SOURCE_URL:=https://github.com/filebrowser/filebrowser
PKG_MIRROR_HASH:=fa4fe4b0de515d49a015eac7fa6245fa411d975ba44a8bfd39f56be1e0bbbed2

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_DEPENDS:=node-pnpm/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/filebrowser/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Filesystem
  TITLE:=Web File Browser
  URL:=https://filebrowser.org
  PROVIDES:=filebrowser
endef

define Package/filebrowser
  $(call Package/filebrowser/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/filebrowser-hf
  $(call Package/filebrowser/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/filebrowser-sf
  $(call Package/filebrowser/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/filebrowser/description
  filebrowser provides a file managing interface within a specified
  directory and it can be used to upload, delete, preview, rename and edit your
  files. It allows the creation of multiple users and each user can have its own
  directory. It can be used as a standalone app or as a middleware.
endef

Package/filebrowser-hf/description=$(Package/filebrowser/description)
Package/filebrowser-sf/description=$(Package/filebrowser/description)

define Package/filebrowser/conffiles
/opt/etc/filebrowser/filebrowser.conf
endef

Package/filebrowser-hf/conffiles=$(Package/filebrowser/conffiles)
Package/filebrowser-sf/conffiles=$(Package/filebrowser/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/v2/version.Version=$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/v2/version.CommitSHA=$(COMMIT_SHORT)'

GO_VARS += \
	GOGC=off \
	GO111MODULE=on

FRONTEND:=$(PKG_NAME)-frontend-$(PKG_VERSION).tar.gz

define Build/Frontend
	( cd $(PKG_BUILD_DIR)/frontend; \
		if [ ! -f "$(DL_DIR)/$(FRONTEND)" ]; then \
			XDG_DATA_HOME=$(TMP_DIR)/pnpm-cache \
				pnpm install --frozen-lockfile; \
			XDG_DATA_HOME=$(TMP_DIR)/pnpm-cache \
				pnpm run build; \
			$(FIND) dist -type f -name "*.br" -o -name "*.gz" | \
				$(XARGS) rm -rf; \
			cd $(PKG_BUILD_DIR) && \
			$(TAR) -cf $(DL_DIR)/$(FRONTEND) frontend/dist; \
		else \
			cd $(PKG_BUILD_DIR) && \
			$(TAR) -xf $(DL_DIR)/$(FRONTEND); \
		fi; \
	)
endef

define Build/Compile
	$(call Build/Frontend)
	$(call Build/Compile/Go)
endef

define Package/filebrowser/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/filebrowser $(1)/opt/sbin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99filebrowser $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/filebrowser
	$(INSTALL_DATA) ./files/filebrowser.conf $(1)/opt/etc/filebrowser
endef

Package/filebrowser-hf/install=$(Package/filebrowser/install)
Package/filebrowser-sf/install=$(Package/filebrowser/install)

$(eval $(call BuildPackage,filebrowser))
$(eval $(call BuildPackage,filebrowser-hf))
$(eval $(call BuildPackage,filebrowser-sf))
