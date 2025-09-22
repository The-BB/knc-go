# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2016-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=syncthing
PKG_VERSION:=2.0.9
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-09-13
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/syncthing/syncthing
PKG_MIRROR_HASH:=273ab18f14630848a367a01f917e2f538e930a110bf2e19a4fa715324d46e094

PKG_MAINTAINER:=Entware team, https://entware.net
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/syncthing/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Utility for synchronization
  URL:=https://syncthing.net/
  PROVIDES:=syncthing
  DEPENDS:=+libc +libgcc +libpthread
endef

define Package/syncthing
  $(call Package/syncthing/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/syncthing-hf
  $(call Package/syncthing/Default)
  VARIANT:=hf
  DEPENDS+=@(mips||mipsel)
endef

define Package/syncthing-sf
  $(call Package/syncthing/Default)
  VARIANT:=sf
  DEPENDS+=@arm
endef

define Package/syncthing/description
 Syncthing replaces proprietary sync and cloud services with something open,
 trustworthy and decentralized. Your data is your data alone and you deserve
 to choose where it is stored, if it is shared with some third party and how
 it's transmitted over the Internet.
endef

Package/syncthing-hf/description=$(Package/syncthing/description)
Package/syncthing-sf/description=$(Package/syncthing/description)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/lib/build.Version=v$(PKG_VERSION)' \
	-X '$(XIMPORTPATH)/lib/build.Stamp=$(PKG_SOURCE_DATE)' \
	-X '$(XIMPORTPATH)/lib/build.User=Entware' \
	-X '$(XIMPORTPATH)/lib/build.Host=Entware' \
	-X '$(XIMPORTPATH)/lib/build.Tags=noupgrade'

GO_VARS += GO111MODULE=on

GO_TAGS:=noupgrade

GO_TARGET:=./cmd/syncthing

define Build/Web
	( cd $(PKG_BUILD_DIR); \
		PATH="$(shell $(GO_BIN) env GOROOT)/bin:$(PATH)" \
		$(GO_ENV_COMMON) $(GO_BIN) run \
		$(if $(findstring s,$(OPENWRT_VERBOSE)),-v -x) \
		build.go assets; \
	)
endef

define Build/Compile
	$(call Build/Web)
	$(call Build/Compile/Go)
endef

define Package/syncthing/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/syncthing $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S92syncthing $(1)/opt/etc/init.d
endef

Package/syncthing-hf/install=$(Package/syncthing/install)
Package/syncthing-sf/install=$(Package/syncthing/install)

$(eval $(call BuildPackage,syncthing))
#$(eval $(call BuildPackage,syncthing-hf))
$(eval $(call BuildPackage,syncthing-sf))
