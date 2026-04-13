# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=prometheus-node-exporter
PKG_VERSION:=1.11.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-04-07
PKG_SOURCE_VERSION:=0dd664dece3f8319f6bec5a221acd2c7ad13a23d
PKG_SOURCE_URL:=https://github.com/prometheus/node_exporter
PKG_MIRROR_HASH:=c7adaea6e3a46c36465f32742dbc1ee8a88395a736876d426f7a78cff66ce4a8

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/prometheus-node-exporter/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=prometheus tools
  TITLE:=Exporter for machine metrics
  URL:=https://prometheus.io/
  PROVIDES:=prometheus-node-exporter
endef

define Package/prometheus-node-exporter
  $(call Package/prometheus-node-exporter/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/prometheus-node-exporter-hf
  $(call Package/prometheus-node-exporter/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/prometheus-node-exporter-sf
  $(call Package/prometheus-node-exporter/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/prometheus-node-exporter/description
  Prometheus exporter for hardware and OS metrics exposed by *NIX
  kernels, written in Go with pluggable metric collectors.
endef

Package/prometheus-node-exporter-hf/description=$(Package/prometheus-node-exporter/description)
Package/prometheus-node-exporter-sf/description=$(Package/prometheus-node-exporter/description)

GO_BUILD_ARGS += -a

GO_LDFLAGS += \
	-X 'github.com/prometheus/common/version.Version=v$(PKG_VERSION)' \
	-X 'github.com/prometheus/common/version.Revision=$(COMMIT_SHORT)'  \
	-X 'github.com/prometheus/common/version.Branch="release"' \
	-X 'github.com/prometheus/common/version.BuildUser=Entware@Entware' \
	-X 'github.com/prometheus/common/version.BuildDate=$(PKG_SOURCE_DATE)'

GO_TAGS:=netgo, osusergo, static_build

define Package/prometheus-node-exporter/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/node_exporter $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99node_exporter $(1)/opt/etc/init.d
endef

Package/prometheus-node-exporter-hf/install=$(Package/prometheus-node-exporter/install)
Package/prometheus-node-exporter-sf/install=$(Package/prometheus-node-exporter/install)

$(eval $(call BuildPackage,prometheus-node-exporter))
$(eval $(call BuildPackage,prometheus-node-exporter-hf))
$(eval $(call BuildPackage,prometheus-node-exporter-sf))
