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

# The pinned upstream commit — the whole point of this fork. Keep this in
# sync with the safety notes above; it is the only thing that decides which
# sources get compiled.
QFIREHOSE_COMMIT:=fbbb4feeea19c265a1ed01d5eedf72e33974bac5

PKG_LICENSE:=Proprietary
PKG_LICENSE_FILES:=NOTICE

PKG_BUILD_PARALLEL:=1

# Extract the pinned sources from this repo's own git history instead of
# cloning them from github at build time.
#
# Two reasons. GitHub refuses anonymous git-over-https from datacenter IPs
# (it answers 401), so SDK builds on rented build hosts died in the
# download phase while codeload and the REST API kept working from the very
# same address. And the checkout is already on the build host — the package
# dir is symlinked into <sdk>/package/, so the repo root is two levels up,
# .git included — which makes the download pure waste even when it works.
#
# Deliberately NOT the working tree: QFIREHOSE_COMMIT is what keeps this
# package on the known-safe sources, whatever else lands on the branch.
# `git archive` fails loudly if that commit is missing (shallow clone),
# which is the right outcome — building something else would be worse.
#
# -o resolves relative to the -C directory, hence the absolute paths.
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	git -C $(CURDIR)/../.. archive --format=tar $(QFIREHOSE_COMMIT) \
		-o $(PKG_BUILD_DIR)/.pinned-src.tar
	tar -xf $(PKG_BUILD_DIR)/.pinned-src.tar -C $(PKG_BUILD_DIR)
	rm -f $(PKG_BUILD_DIR)/.pinned-src.tar
endef

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

  Tested only on RM520N-GL. Other Quectel modules may work but are
  unverified by this package's maintainer.

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
