# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=headscale
PKG_VERSION:=0.28.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/juanfont/headscale
PKG_MIRROR_HASH:=44e337fe5af175e8b941c2bb605009ed29946caceaff0237ca3e50f94872ad08

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/headscale/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=Tailscale control server
  URL:=https://github.com/juanfont/headscale
  PROVIDES:=headscale
  DEPENDS:=@!(mips||mipsel)
  EXTRA_DEPENDS:=ca-bundle
endef

define Package/headscale
  $(call Package/headscale/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/headscale-hf
  $(call Package/headscale/Default)
  VARIANT:=hf
endef

define Package/headscale-sf
  $(call Package/headscale/Default)
  VARIANT:=sf
  DEPENDS+=@arm
endef

define Package/headscale/description
 Headscale aims to implement a self-hosted, open source alternative
 to the Tailscale control server. Headscale's goal is to provide self-hosters
 and hobbyists with an open-source server they can use for their projects and
 labs. It implements a narrow scope, a single Tailnet, suitable for a personal
 use, or a small open-source organisation.
endef

Package/headscale-hf/description=$(Package/headscale/description)
Package/headscale-sf/description=$(Package/headscale/description)

define Package/headscale/conffiles
/opt/etc/headscale/config.yaml
endef

Package/headscale-hf/conffiles=$(Package/headscale/conffiles)
Package/headscale-sf/conffiles=$(Package/headscale/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/cmd/headscale/cli.Version=v$(PKG_VERSION)'

GO_BUILD_ARGS += -a

GO_TARGET:=./cmd/headscale

define Package/headscale/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/headscale $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/headscale
	$(CP) $(PKG_BUILD_DIR)/config-example.yaml $(1)/opt/etc/headscale/config.yaml
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S07headscale $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/var/{lib,run}/headscale
endef

Package/headscale-hf/install=$(Package/headscale/install)
Package/headscale-sf/install=$(Package/headscale/install)

$(eval $(call BuildPackage,headscale))
#$(eval $(call BuildPackage,headscale-hf))
$(eval $(call BuildPackage,headscale-sf))
