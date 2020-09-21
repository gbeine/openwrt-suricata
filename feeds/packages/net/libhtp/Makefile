#
# Copyright (C) 2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libhtp
PKG_VERSION:=0.5.34
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OISF/$(PKG_NAME)/archive/
PKG_HASH:=088674560082d403b1b677d6ff57caf8dab9f1f27a7539136ee604e40aa3022d

PKG_MAINTAINER:=Gerrit Beine <gb@simonwunderlich.de>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libhtp
  SECTION:=net
  CATEGORY:=Network
  TITLE:=A security-aware parser for the HTTP protocol
  URL:=https://github.com/OISF/libhtp
  DEPENDS:=+zlib
endef

define Package/libhtp/description
	LibHTP is a security-aware parser for the HTTP protocol and the
	related bits and pieces.
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib,include}
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libhtp* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
endef

define Package/libhtp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libhtp*so* $(1)/usr/lib
endef

$(eval $(call BuildPackage,libhtp))
