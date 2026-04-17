#
# OpenWrt package recipe for qfirehose.
# Source: fork of nippynetworks/qfirehose pinned to safe commit.
#
# Known-bad upstream version: Quectel 1.4.11 (brick risk).
# Safe: nippynetworks main @ fbbb4fe (= tag 1.4.17 + CMakeLists fixes).
#
# Refs:
#   https://github.com/nippynetworks/qfirehose (upstream fork)
#   https://github.com/nippynetworks/qfirehose/releases/tag/1.4.17
#   https://forums.quectel.com/t/how-to-upgrade-module-thru-linux-qfirehose/15556
#   https://github.com/vjt/qfirehose (this fork)
#

include $(TOPDIR)/rules.mk

PKG_NAME:=qfirehose
PKG_VERSION:=1.4.17
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/vjt/qfirehose.git
PKG_SOURCE_VERSION:=fbbb4feeea19c265a1ed01d5eedf72e33974bac5
PKG_SOURCE_DATE:=2024-12-09
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=Proprietary
PKG_LICENSE_FILES:=NOTICE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/qfirehose
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Quectel Firehose modem firmware flasher
  URL:=https://github.com/vjt/qfirehose
endef

define Package/qfirehose/description
  Utility to flash firmware on Quectel modems (EM/RM/RG series) via
  Qualcomm Firehose over EDL. Fork of nippynetworks/qfirehose.

  WARNING: flashing wrong firmware can permanently brick the modem.
  Do NOT use Quectel upstream v1.4.11 — known to brick. This package
  pins to nippynetworks 1.4.17 which is safe for RM520N-GL.

  Usage: qfirehose -f /path/to/firmware_dir
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) linux \
		CC="$(TARGET_CC)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		CFLAGS="$(TARGET_CFLAGS) -Wall" \
		LDFLAGS="$(TARGET_LDFLAGS) -lpthread -ldl"
endef

define Package/qfirehose/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/QFirehose $(1)/usr/bin/qfirehose
endef

$(eval $(call BuildPackage,qfirehose))
