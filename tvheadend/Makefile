#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tvheadend
PKG_VERSION:=2018-04-22
PKG_RELEASE:=$(SHORT_GIT_VER)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=595fd174fa6512efc220e7dcd82b0f9b129f14b0
SHORT_GIT_VER:=$(shell echo $(PKG_SOURCE_VERSION) | cut -b -7)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(SHORT_GIT_VER).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/tvheadend/tvheadend.git
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE.md
PKG_FIXUP:=autoreconf
PKG_USE_MIPS16:=0


include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/tvheadend
  SECTION:=DVB-EXTRAS
  CATEGORY:=DVB-EXTRAS
  TITLE:=Tvheadend is a TV streaming server for Linux
  DEPENDS:=+libopenssl +librt +zlib +libpcre +libpcre2 +libdvbcsa $(ICONV_DEPENDS)
  URL:=https://tvheadend.org
endef

define Package/tvheadend/description
  Tvheadend is a TV streaming server and recorder for Linux, FreeBSD and Android
  supporting DVB-S, DVB-S2, DVB-C, DVB-T, ATSC, IPTV, SAT>IP and HDHomeRun as input sources.

  Tvheadend offers the HTTP (VLC, MPlayer), HTSP (Kodi, Movian) and SAT>IP streaming.
endef

CONFIGURE_ARGS += \
	--arch=$(ARCH) \
    --disable-libav \
	--disable-dbus_1 \
    --disable-libx265 \
	--disable-ffmpeg_static \
	--enable-bundle

define Build/Prepare
	$(call Build/Prepare/Default)
	echo 'Tvheadend $(shell echo $(PKG_VERSION)~$(SHORT_GIT_VER))' \
	> $(PKG_BUILD_DIR)/debian/changelog
endef

define Package/conffiles
/etc/config/tvheadend
endef

define Package/tvheadend/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/tvheadend.init $(1)/etc/init.d/tvheadend
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/tvheadend.config $(1)/etc/config/tvheadend

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build.linux/tvheadend $(1)/usr/bin/
endef

$(eval $(call BuildPackage,tvheadend))
