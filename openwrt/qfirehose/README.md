# qfirehose OpenWrt package

Package recipe for building `qfirehose` as an OpenWrt package.

Pinned commit: `fbbb4fe` (nippynetworks 1.4.17 + CMakeLists fixes).

**Tested only on Quectel RM520N-GL.** Other Quectel modems unverified.

## Why a fork

- **Version safety**: Quectel upstream v1.4.11 is known to brick modems
  (Quectel official bulletin). This fork pins to [1.4.17 via nippynetworks](https://github.com/nippynetworks/qfirehose/releases/tag/1.4.17).
  See also: [Quectel forum thread on qfirehose upgrades](https://forums.quectel.com/t/how-to-upgrade-module-thru-linux-qfirehose/15556).
- **Reproducible builds**: PKG_SOURCE_VERSION locks to an exact commit SHA.
- **Feed-friendly layout**: `openwrt/qfirehose/` is a drop-in package
  directory — wire it into your buildroot as a local feed, no file
  copying required.

## Usage in an OpenWrt build

Clone this repo somewhere persistent on the build host, then register
it as a local feed in `<openwrt>/feeds.conf`:

```
src-link custom /path/to/feeds-local
```

Symlink the package dir into `feeds-local/`:

```sh
ln -s /path/to/qfirehose/openwrt/qfirehose <openwrt>/feeds-local/qfirehose
```

Then update + install + build:

```sh
cd <openwrt>
./scripts/feeds update custom
./scripts/feeds install -a -p custom
make menuconfig       # Utilities -> qfirehose
make package/qfirehose/compile V=s
```

Resulting `.apk` (OpenWrt ≥ 24.10) or `.ipk` lands in
`bin/packages/<arch>/custom/`.

## Runtime usage

```sh
qfirehose -f /tmp/RM520NGLAAR03A03M4G/
```

Directory must contain `update/firehose/` (standard Quectel firmware layout).

**Never** flash while on battery or unstable power. Expect ~2–5 minutes.
Modem reboots on completion; verify with `AT+QGMR` afterwards.
