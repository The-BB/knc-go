# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=go2rtc
PKG_VERSION:=1.9.12
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/AlexxIT/go2rtc
PKG_MIRROR_HASH:=097ddc9a9837c2705c0a7da2ae7dd671673697048955240b82b3db9eb370deb5

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/go2rtc/Default
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=camera streaming application
  URL:=https://github.com/AlexxIT/go2rtc
  PROVIDES:=go2rtc
  EXTRA_DEPENDS:=ffmpeg
endef

define Package/go2rtc
  $(call Package/go2rtc/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/go2rtc-hf
  $(call Package/go2rtc/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/go2rtc-sf
  $(call Package/go2rtc/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/go2rtc/description
 Ultimate camera streaming application with support RTSP, RTMP,
 HTTP-FLV, WebRTC, MSE, HLS, MP4, MJPEG, HomeKit, FFmpeg, etc.
endef

Package/go2rtc-hf/description=$(Package/go2rtc/description)
Package/go2rtc-sf/description=$(Package/go2rtc/description)

define Package/go2rtc/conffiles
/opt/etc/go2rtc.yaml
endef

Package/go2rtc-hf/conffiles=$(Package/go2rtc/conffiles)
Package/go2rtc-sf/conffiles=$(Package/go2rtc/conffiles)

define Build/Prepare
	$(call Build/Prepare/Default)
  ifeq ($(CONFIG_mips),y)
	$(SED) '/v4l2/d' $(PKG_BUILD_DIR)/main.go
  endif
endef

define Package/go2rtc/install
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S98go2rtc $(1)/opt/etc/init.d
	$(INSTALL_DATA) ./files/go2rtc.yaml $(1)/opt/etc
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/go2rtc $(1)/opt/sbin
endef

Package/go2rtc-hf/install=$(Package/go2rtc/install)
Package/go2rtc-sf/install=$(Package/go2rtc/install)

$(eval $(call BuildPackage,go2rtc))
$(eval $(call BuildPackage,go2rtc-hf))
$(eval $(call BuildPackage,go2rtc-sf))
