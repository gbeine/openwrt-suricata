#
# Copyright (C) 2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=suricata
PKG_VERSION:=4.1.8
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://www.openinfosecfoundation.org/download/
PKG_HASH:=c8a83a05f57cedc0ef81d833ddcfdbbfdcdb6f459a91b1b15dc2d5671f1aecbb

PKG_MAINTAINER:=Gerrit Beine <gb@simonwunderlich.de>
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk
include ../../lang/python/python-package.mk

CONFIGURE_ARGS+= \
	--prefix=/usr \
	--sysconfdir=/etc \
	--localstatedir=/var \
	--enable-nflog \
	--enable-nfqueue \
	--enable-gccprotect \
	--enable-pie \
	--enable-non-bundled-htp \
	--disable-suricata-update \
	--host=$(ARCH)

define Package/suricata/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Network IDS, IPS and NSM engine
  URL:=https://suricata-ids.org/
endef

define Package/suricata
  $(call Package/suricata/Default)
  DEPENDS:=+libpcap +libpcre +libmagic +zlib +libyaml +jansson +libnfnetlink +libnetfilter-log +libhtp +file
endef

define Package/suricata-scripts
  $(call Package/suricata/Default)
  TITLE:=Suricata-IDS Python scripts
  DEPENDS:=+suricata +python
endef

define Package/suricata/Default/description
  Suricata is a free and open source, mature, fast and robust network
  threat detection engine.
endef

define Package/suricata/description
  $(call Package/suricata/Default/description)
endef

define Package/suricata-scripts/description
  $(call Package/suricata/Default/description)
  This package contains python scripts for suricata.
endef

define Package/suricata/install 
	$(INSTALL_DIR) $(1)/etc/suricata/rules
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/suricata $(1)/usr/bin
	$(CP) \
		$(PKG_BUILD_DIR)/suricata.yaml \
		$(PKG_BUILD_DIR)/classification.config \
		$(PKG_BUILD_DIR)/reference.config \
		$(PKG_BUILD_DIR)/threshold.config \
		$(1)/etc/suricata
	$(CP) $(PKG_BUILD_DIR)/rules/*.rules $(1)/etc/suricata/rules
endef

define Package/suricata-scripts/install 
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/{suricata,suricatasc}
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/{suricatactl,suricatasc} $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/suricata/* \
		$(1)$(PYTHON_PKG_DIR)/suricata
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/suricatasc/* \
		$(1)$(PYTHON_PKG_DIR)/suricatasc
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/suricata*egg-info \
		$(1)$(PYTHON_PKG_DIR)
	# fix python path in scripts
	$(SED) 's/#!.*/#!\/usr\/bin\/python/g' \
		$(1)/usr/bin/{suricatactl,suricatasc}
endef

$(eval $(call BuildPackage,suricata))
$(eval $(call BuildPackage,suricata-scripts))
