# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=alertmanager
PKG_VERSION:=0.32.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-04-08
PKG_SOURCE_VERSION:=685a2a1c6bb01b2c17bc1bfae995cb3416c1115e
PKG_SOURCE_URL:=https://github.com/prometheus/alertmanager
PKG_MIRROR_HASH:=663feb1001dc678c4312c0a86595a1ad570db8ca8721163b20eddae4db1c3376

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/alertmanager/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=prometheus tools
  TITLE:=Prometheus Alertmanager
  URL:=http://prometheus.io
  PROVIDES:=alertmanager
endef

define Package/alertmanager
  $(call Package/alertmanager/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/alertmanager-hf
  $(call Package/alertmanager/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/alertmanager-sf
  $(call Package/alertmanager/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/alertmanager/description
  The Alertmanager handles alerts sent by client applications such
  as the Prometheus server. It takes care of deduplicating, grouping,
  and routing them to the correct receiver integrations such as email,
  PagerDuty, OpsGenie, or many other mechanisms thanks to the webhook
  receiver. It also takes care of silencing and inhibition of alerts.
endef

Package/alertmanager-hf/description=$(Package/alertmanager/description)
Package/alertmanager-sf/description=$(Package/alertmanager/description)

define Package/alertmanager/conffiles
/opt/etc/prometheus/alertmanager.yml
endef

Package/alertmanager-hf/conffiles=$(Package/alertmanager/conffiles)
Package/alertmanager-sf/conffiles=$(Package/alertmanager/conffiles)

GO_LDFLAGS += \
	-X 'github.com/prometheus/common/version.Version=v$(PKG_VERSION)' \
	-X 'github.com/prometheus/common/version.Revision=$(COMMIT_SHORT)'  \
	-X 'github.com/prometheus/common/version.Branch="release"' \
	-X 'github.com/prometheus/common/version.BuildUser=Entware@Entware' \
	-X 'github.com/prometheus/common/version.BuildDate=$(PKG_SOURCE_DATE)'

GO_TAGS:=netgo
GO_TARGET:=./cmd/alertmanager ./cmd/amtool

FRONTEND:=$(PKG_NAME)-frontend-$(PKG_VERSION).tar.gz

ifeq (,$(wildcard $(DL_DIR)/$(FRONTEND)))
PKG_BUILD_DEPENDS:=node/host
endif

define Build/Frontend
	( cd $(PKG_BUILD_DIR)/ui/app; \
		if [ ! -f "$(DL_DIR)/$(FRONTEND)" ]; then \
			npm ci; npm run build; \
			if [ -d "$(PKG_BUILD_DIR)/ui/app/dist/assets" ]; then \
				cd $(PKG_BUILD_DIR) && \
				$(TAR) -cf $(DL_DIR)/$(FRONTEND) ui/app/dist; \
			fi; \
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

define Package/alertmanager/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/{alertmanager,amtool} $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S90alertmanager $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/prometheus
	$(CP) $(PKG_BUILD_DIR)/examples/ha/alertmanager.yml $(1)/opt/etc/prometheus
endef

Package/alertmanager-hf/install=$(Package/alertmanager/install)
Package/alertmanager-sf/install=$(Package/alertmanager/install)

$(eval $(call BuildPackage,alertmanager))
$(eval $(call BuildPackage,alertmanager-hf))
$(eval $(call BuildPackage,alertmanager-sf))
