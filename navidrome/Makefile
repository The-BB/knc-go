# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2022-2026 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=navidrome
PKG_VERSION:=0.60.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=34c6f12aeeb11144100d764b06248e0a40ccb126
PKG_SOURCE_URL:=https://github.com/navidrome/navidrome
PKG_MIRROR_HASH:=bfa2d6c59110677297f012277bdd55d0464e93b8bb9babf8b8361215660da46e

PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_CGO_ENABLED:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/navidrome/Default
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=Modern Music Server and Streamer
  URL:=https://www.navidrome.org/
  DEPENDS:=+libc +libgcc +libpthread +libstdcpp +taglib +zlib
  EXTRA_DEPENDS:=ffmpeg
endef

define Package/navidrome
  $(call Package/navidrome/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/dnstt-client-hf
  $(call Package/dnstt/Default)
  VARIANT:=hf
  DEPENDS+=@(mips||mipsel)
endef

define Package/navidrome-sf
  $(call Package/navidrome/Default)
  VARIANT:=sf
  DEPENDS+=@arm
endef

define Package/navidrome/description
 Navidrome is an open source web-based music collection server and streamer.
 It gives you freedom to listen to your music collection from any browser or
 mobile device.
endef

Package/navidrome-hf/description=$(Package/navidrome/description)
Package/navidrome-sf/description=$(Package/navidrome/description)

define Package/navidrome/conffiles
/opt/etc/navidrome.toml
endef

Package/navidrome-hf/conffiles=$(Package/navidrome/conffiles)
Package/navidrome-sf/conffiles=$(Package/navidrome/conffiles)

GO_LDFLAGS += \
	-X '$(XIMPORTPATH)/consts.gitSha=$(COMMIT_SHORT)' \
	-X '$(XIMPORTPATH)/consts.gitTag=v$(PKG_VERSION)'

GO_TAGS:=netgo

FRONTEND:=$(PKG_NAME)-frontend-$(PKG_VERSION).tar.gz

define Build/Frontend
	( cd $(PKG_BUILD_DIR)/ui; \
		if [ ! -f "$(DL_DIR)/$(FRONTEND)" ]; then \
			npm ci; npm run build; \
			$(FIND) build -type f -iname "*.map" -delete; \
			cd $(PKG_BUILD_DIR) && \
			$(TAR) -cf $(DL_DIR)/$(FRONTEND) ui/build; \
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

define Package/navidrome/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/navidrome $(1)/opt/sbin
	$(INSTALL_DIR) $(1)/opt/etc
	$(INSTALL_DATA) ./files/navidrome.toml $(1)/opt/etc
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S91navidrome $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/data/music
endef

Package/navidrome-hf/install=$(Package/navidrome/install)
Package/navidrome-sf/install=$(Package/navidrome/install)

$(eval $(call BuildPackage,navidrome))
#$(eval $(call BuildPackage,navidrome-hf))
$(eval $(call BuildPackage,navidrome-sf))
