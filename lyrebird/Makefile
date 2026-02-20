# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=lyrebird
PKG_VERSION:=0.8.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/lyrebird
PKG_MIRROR_HASH:=1f8a79e5cee1c2be402e36a07fa2222675b56fbda2151a4f1cc0a2f7ac4a3580

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE LICENSE-GPL3.txt
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/lyrebird/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=pluggable transport proxy for Tor, implementing obfs4
  URL:=https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/lyrebird
  PROVIDES:=lyrebird
endef

define Package/lyrebird
  $(call Package/lyrebird/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/lyrebird-hf
  $(call Package/lyrebird/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/lyrebird-sf
  $(call Package/lyrebird/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/lyrebird/description
  This is a look-like nothing obfuscation protocol that
  incorporates ideas and concepts from Philipp Winter's ScrambleSuit protocol.
  The obfs naming was chosen primarily because it was shorter, in terms of
  protocol ancestery obfs4 is much closer to ScrambleSuit than obfs2/obfs3.
endef

Package/lyrebird-hf/description=$(Package/lyrebird/description)
Package/lyrebird-sf/description=$(Package/lyrebird/description)

GO_LDFLAGS += -X 'main.lyrebirdVersion=$(PKG_VERSION)'

GO_TARGET:=./cmd/lyrebird

define Package/lyrebird/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/lyrebird $(1)/opt/sbin
endef

Package/lyrebird-hf/install=$(Package/lyrebird/install)
Package/lyrebird-sf/install=$(Package/lyrebird/install)

$(eval $(call BuildPackage,lyrebird))
$(eval $(call BuildPackage,lyrebird-hf))
$(eval $(call BuildPackage,lyrebird-sf))
