# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=prometheus
PKG_VERSION:=3.12.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-05-28
PKG_SOURCE_VERSION:=9f27dffc1f93ca23287972f632025879f2d1c658
PKG_SOURCE_URL:=https://github.com/prometheus/prometheus
PKG_MIRROR_HASH:=26fe983695be91c6328111535172495174c490dc498b7138614ce8eceeda6ea3

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/prometheus/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=prometheus tools
  TITLE:=Monitoring system and time series database
  URL:=http://prometheus.io
  PROVIDES:=prometheus
endef

define Package/prometheus
  $(call Package/prometheus/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/prometheus-hf
  $(call Package/prometheus/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/prometheus-sf
  $(call Package/prometheus/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/prometheus/description
  Prometheus, a Cloud Native Computing Foundation project,
  is a systems and service monitoring system. It collects
  metrics from configured targets at given intervals, evaluates
  rule expressions, displays the results, and can trigger alerts
  when specified conditions are observed.
endef

Package/prometheus-hf/description=$(Package/prometheus/description)
Package/prometheus-sf/description=$(Package/prometheus/description)

define Package/prometheus/conffiles
/opt/etc/prometheus/prometheus.yml
endef

Package/prometheus-hf/conffiles=$(Package/prometheus/conffiles)
Package/prometheus-sf/conffiles=$(Package/prometheus/conffiles)

WEB_FILE:=$(PKG_NAME)-web-ui-$(PKG_VERSION).tar.gz
define Download/prometheus-web-ui
  URL:=$(PKG_SOURCE_URL)/releases/download/v$(PKG_VERSION)/
  URL_FILE:=$(WEB_FILE)
  FILE:=$(WEB_FILE)
  HASH:=f2cf6129bbbb030328587aa7e1bb17b9616d7f6f3ad8bb290a0c9aef3406b2ed
endef

GO_BUILD_ARGS += -a

GO_LDFLAGS += \
	-X 'github.com/prometheus/common/version.Version=v$(PKG_VERSION)' \
	-X 'github.com/prometheus/common/version.Revision=$(COMMIT_SHORT)'  \
	-X 'github.com/prometheus/common/version.Branch="release"' \
	-X 'github.com/prometheus/common/version.BuildUser=Entware@Entware' \
	-X 'github.com/prometheus/common/version.BuildDate=$(PKG_SOURCE_DATE)'

GO_TAGS:=builtinassets, netgo

GO_TARGET:=./cmd/...

GO_VARS += GO111MODULE=on

define Build/Web
	$(HOST_TAR) -xzf $(DL_DIR)/$(WEB_FILE) -C $(PKG_BUILD_DIR)/web/ui
	( cd $(PKG_BUILD_DIR)/web/ui; \
		$(FIND) ./static -type f -a -name "*.map" -delete; )
	( cd $(PKG_BUILD_DIR); $(SHELL) scripts/compress_assets.sh; )
	( cd $(PKG_BUILD_DIR); $(GO_BIN_GENERATE) -tags plugins ./plugins; )
endef

define Build/Compile
	$(call Build/Web)
	$(call Build/Compile/Go)
endef

define Package/prometheus/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/prom{etheus,tool} $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/prometheus
	$(INSTALL_DATA) \
		$(PKG_BUILD_DIR)/documentation/examples/prometheus.yml \
			$(1)/opt/etc/prometheus
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S70prometheus $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/var/lib/prometheus
endef

Package/prometheus-hf/install=$(Package/prometheus/install)
Package/prometheus-sf/install=$(Package/prometheus/install)

$(eval $(call Download,prometheus-web-ui))
$(eval $(call BuildPackage,prometheus))
$(eval $(call BuildPackage,prometheus-hf))
$(eval $(call BuildPackage,prometheus-sf))
