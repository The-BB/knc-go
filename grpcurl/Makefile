# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024-2025 Entware

include $(TOPDIR)/rules.mk

PKG_NAME:=grpcurl
PKG_VERSION:=1.9.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/fullstorydev/grpcurl
PKG_MIRROR_HASH:=4c8922fd1bc07e7b93bc68f34744ed47cc48754e488a24fd42542393a43b1091

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Entware team, https://entware.net

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/grpcurl/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=CLI tool to interact with gRPC servers
  URL:=https://github.com/fullstorydev/grpcurl
  PROVIDES:=grpcurl
endef

define Package/grpcurl
  $(call Package/grpcurl/Default)
  VARIANT:=default
  DEFAULT_VARIANT:=1
endef

define Package/grpcurl-hf
  $(call Package/grpcurl/Default)
  VARIANT:=hf
  DEPENDS:=@(mips||mipsel)
endef

define Package/grpcurl-sf
  $(call Package/grpcurl/Default)
  VARIANT:=sf
  DEPENDS:=@arm
endef

define Package/grpcurl/description
  grpcurl is a command-line tool that lets you interact with gRPC servers.
  It's basically curl for gRPC servers.
endef

Package/grpcurl-hf/description=$(Package/grpcurl/description)
Package/grpcurl-sf/description=$(Package/grpcurl/description)

GO_LDFLAGS += \
	-X 'main.version=$(PKG_VERSION)'

GO_TARGET:=./cmd/grpcurl

define Package/grpcurl/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/grpcurl $(1)/opt/sbin/grpcurl
endef

Package/grpcurl-hf/install=$(Package/grpcurl/install)
Package/grpcurl-sf/install=$(Package/grpcurl/install)

$(eval $(call BuildPackage,grpcurl))
$(eval $(call BuildPackage,grpcurl-hf))
$(eval $(call BuildPackage,grpcurl-sf))
