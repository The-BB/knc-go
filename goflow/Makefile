# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

# XXX Project archived by the owner on Feb 19, 2025.

include $(TOPDIR)/rules.mk

PKG_NAME:=goflow
PKG_VERSION:=3.5.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-01-21
PKG_SOURCE_VERSION:=8607c5af48d09b2caeb3add8eb69f7bcea9f5506
PKG_SOURCE_URL:=https://github.com/cloudflare/goflow
PKG_MIRROR_HASH:=73cc75f17aba13ada29440c79edb7840513719141e3cd770fcc521b3576b047d

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/goflow/Default
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=prometheus tools
  TITLE:=a NetFlow/IPFIX/sFlow collector
  URL:=https://github.com/cloudflare/goflow
  CONFLICTS:=netflow-exporter
endef

define Package/goflow
  $(call Package/goflow/Default)
  TITLE+=(full)
  VARIANT:=default
  DEFAULT_VARIANT:=1
  PROVIDES:=goflow
endef

define Package/goflow-hf
  $(call Package/goflow/Default)
  TITLE+=(full)
  VARIANT:=hf
  PROVIDES:=goflow
  DEPENDS:=@(mips||mipsel)
endef

define Package/goflow-sf
  $(call Package/goflow/Default)
  TITLE+=(full)
  VARIANT:=sf
  PROVIDES:=goflow
  DEPENDS:=@arm
endef

define Package/goflow/description
  The high-scalability sFlow/NetFlow/IPFIX collector used
  internally at Cloudflare.
  (full)
endef

Package/goflow-hf/description=$(Package/goflow/description)
Package/goflow-sf/description=$(Package/goflow/description)

define Package/cnetflow
  $(call Package/goflow/Default)
  TITLE+=(IPFIX/NetFlow v9)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/cnetflow-hf
  $(call Package/goflow/Default)
  TITLE+=(IPFIX/NetFlow v9)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/cnetflow-sf
  $(call Package/goflow/Default)
  TITLE+=(IPFIX/NetFlow v9)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/cnetflow/description
  The high-scalability sFlow/NetFlow/IPFIX collector used
  internally at Cloudflare.
  (IPFIX/NetFlow v9)
endef

Package/cnetflow-hf/description=$(Package/cnetflow/description)
Package/cnetflow-sf/description=$(Package/cnetflow/description)

define Package/cnflegacy
  $(call Package/goflow/Default)
  TITLE+=(NetFlow v5)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/cnflegacy-hf
  $(call Package/goflow/Default)
  TITLE+=(NetFlow v5)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/cnflegacy-sf
  $(call Package/goflow/Default)
  TITLE+=(NetFlow v5)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/cnflegacy/description
  The high-scalability sFlow/NetFlow/IPFIX collector used
  internally at Cloudflare.
  (NetFlow v5)
endef

Package/cnflegacy-hf/description=$(Package/cnflegacy/description)
Package/cnflegacy-sf/description=$(Package/cnflegacy/description)

define Package/csflow
  $(call Package/goflow/Default)
  TITLE+=(sFlow v5)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/csflow-hf
  $(call Package/goflow/Default)
  TITLE+=(sFlow v5)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/csflow-sf
  $(call Package/goflow/Default)
  TITLE+=(sFlow v5)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/csflow/description
  The high-scalability sFlow/NetFlow/IPFIX collector used
  internally at Cloudflare.
  (sFlow v5)
endef

Package/csflow-hf/description=$(Package/csflow/description)
Package/csflow-sf/description=$(Package/csflow/description)

GO_LDFLAGS += \
	-X 'main.version=$(PKG_VERSION)' \
	-X 'main.buildinfos=$(PKG_SOURCE_DATE)'

GO_TARGET:=./cmd/...

define Package/cnetflow/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/cnetflow $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99cnetflow $(1)/opt/etc/init.d
endef

Package/cnetflow-hf/install=$(Package/cnetflow/install)
Package/cnetflow-sf/install=$(Package/cnetflow/install)

define Package/cnflegacy/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/cnflegacy $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99cnflegacy $(1)/opt/etc/init.d
endef

Package/cnflegacy-hf/install=$(Package/cnflegacy/install)
Package/cnflegacy-sf/install=$(Package/cnflegacy/install)

define Package/csflow/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/csflow $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99csflow $(1)/opt/etc/init.d
endef

Package/csflow-hf/install=$(Package/csflow/install)
Package/csflow-sf/install=$(Package/csflow/install)

define Package/goflow/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/goflow $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S99goflow $(1)/opt/etc/init.d
endef

Package/goflow-hf/install=$(Package/goflow/install)
Package/goflow-sf/install=$(Package/goflow/install)

$(eval $(call BuildPackage,cnetflow))
$(eval $(call BuildPackage,cnetflow-hf))
$(eval $(call BuildPackage,cnetflow-sf))
$(eval $(call BuildPackage,cnflegacy))
$(eval $(call BuildPackage,cnflegacy-hf))
$(eval $(call BuildPackage,cnflegacy-sf))
$(eval $(call BuildPackage,csflow))
$(eval $(call BuildPackage,csflow-hf))
$(eval $(call BuildPackage,csflow-sf))
$(eval $(call BuildPackage,goflow))
$(eval $(call BuildPackage,goflow-hf))
$(eval $(call BuildPackage,goflow-sf))
