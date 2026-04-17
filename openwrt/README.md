# qfirehose OpenWrt package

Package recipe for building `qfirehose` as an OpenWrt package.

Pinned commit: `fbbb4fe` (nippynetworks 1.4.17 + CMakeLists fixes).

## Why a fork

- **Version safety**: Quectel upstream v1.4.11 is known to brick modems
  (Quectel official bulletin). This fork pins to 1.4.17 via nippynetworks.
- **Reproducible builds**: PKG_SOURCE_VERSION locks to an exact commit SHA.
- **Feed-friendly**: recipe can be dropped into any OpenWrt package feed
  or `package/utils/qfirehose/`.

## Usage in an OpenWrt build

```sh
mkdir -p <openwrt>/package/utils/qfirehose
cp openwrt/Makefile <openwrt>/package/utils/qfirehose/Makefile
cd <openwrt>
make menuconfig   # Utilities -> qfirehose
make package/qfirehose/compile V=s
```

Resulting `.ipk` lands in `bin/packages/<arch>/base/`.

## Runtime usage

```sh
qfirehose -f /tmp/RM520NGLAAR03A03M4G/
```

Directory must contain `update/firehose/` (standard Quectel firmware layout).

**Never** flash while on battery or unstable power. Expect ~2–5 minutes.
Modem reboots on completion; verify with `AT+QGMR` afterwards.
