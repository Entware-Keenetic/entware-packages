#
# Copyright (C) 2015, 2016 gxcreator
# Copyright (C) 2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=i2pd
PKG_VERSION:=2.18.0
PKG_RELEASE:=1
PKG_BUILD_PARALLEL:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_MIRROR_HASH:=1a04826b54e649ebb341d3feacdd05c0415335679d1aa787a74ea5995d419378
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/PurpleI2P/i2pd.git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_LICENSE:=BSD-3-clause

include $(INCLUDE_DIR)/package.mk

define Package/i2pd
	SECTION:=net
	CATEGORY:=Network
	DEPENDS:=+libopenssl +boost +boost-system +boost-filesystem \
	         +boost-program_options +boost-date_time +libatomic +zlib
	TITLE:=full-featured C++ implementation of I2P client
	URL:=https://github.com/PurpleI2P/i2pd
#	USERID:=i2pd:i2pd
	MAINTAINER:=David Yang <mmyangfl@gmail.com>
endef

define Package/i2pd/description
	I2P (Invisible Internet Protocol) is a universal anonymous network layer. All
	communications over I2P are anonymous and end-to-end encrypted, participants
	don't reveal their real IP addresses.
endef

define Package/i2pd/conffiles
	/opt/etc/i2pd/i2pd.conf
	/opt/etc/i2pd/subscriptions.txt
	/opt/etc/i2pd/tunnels.conf
endef

TARGET_LDFLAGS+=-latomic
MAKE_FLAGS+=USE_AESNI=no USE_AVX=no

define Package/i2pd/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/i2pd $(1)/opt/sbin
	$(INSTALL_DIR) $(1)/opt/share/i2pd
	$(CP) $(PKG_BUILD_DIR)/contrib/certificates  $(1)/opt/share/i2pd
	$(INSTALL_DIR) $(1)/opt/etc/i2pd
	$(LN) /opt/share/i2pd/certificates $(1)/opt/etc/i2pd/certificates
	$(LN) /opt/var/lib/i2pd/peerProfiles $(1)/opt/etc/i2pd/peerProfiles
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/contrib/i2pd.conf $(1)/opt/etc/i2pd
	$(SED) ' \
		s/127.0.0.1/192.168.1.1/g; \
		s/datadir = \/var\/lib/datadir = \/opt\/etc/ \
	' $(1)/opt/etc/i2pd/i2pd.conf
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/contrib/subscriptions.txt $(1)/opt/etc/i2pd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/contrib/tunnels.conf $(1)/opt/etc/i2pd
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S30i2p $(1)/opt/etc/init.d
endef

define Package/i2pd/postinst
#!/bin/sh

[ -f /opt/etc/i2pd/i2p.conf ] || exit 0
echo 'Please note: new path of configuration file is /opt/etc/i2pd/i2pd.conf'
echo 'You may check /opt/etc/i2pd/i2p.conf for custom options and delete it as obsolete.'
endef

$(eval $(call BuildPackage,i2pd))
