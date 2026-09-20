# POCO F7 (onyx) Wi-Fi 7 + 6 GHz Hotspot (Magisk Module)

Verified camera-safe Magisk module enabling real **Wi-Fi 7 (802.11be)**, standalone **6 GHz Wi-Fi**, **5 GHz + 6 GHz MLO**, and **6 GHz SoftAP** on the POCO F7 (`onyx`) running Project Infinity-X 3.12 with the Global `OS3.0.302.0.WOLMIXM` vendor. Camera and hotspot have been verified working together.

## Verified result

- Protocol: 802.11be (WiFi 7)
- Band: 6 GHz
- ACS hotspot channel: 37 & 133
- Observed hotspot client link speed: 5764/5764 Mbps
- Security: WPA3-Personal
- Maximum channel bandwidth: 320 MHz enabled
- Client: Intel Wi-Fi 7 BE200 320 MHz
<img width="799" height="846" alt="{Intel Wi-Fi 7 BE200 320 MHz}" src="https://github.com/user-attachments/assets/402eef24-4657-447e-97e8-7a37f6ed97f0" />

## Verified operating modes

### Wi-Fi client

- Connected using Wi-Fi 7 (`802.11be`) with WPA3-SAE.
- MLO established with active 5 GHz (channel 144) and 6 GHz (channel 85) affiliated links.
- Standalone 6 GHz SSID verified in Android scan results and Settings UI at 6375 MHz/channel 85.
- Android reports `config_wifi6ghzSupport=true`.

### Wi-Fi hotspot

- Wi-Fi 7 (`802.11be`) hotspot verified on the 2.4Ghz/5Ghz/6 GHz band.
- ACS selected 6 GHz channel 37 & 133.
- Maximum channel bandwidth of 320 MHz is enabled.
- Android reports `config_wifiSoftap6ghzSupported=true`.

SSID, BSSID, client MAC, and IP details are intentionally omitted from this public verification.

## Exact target

- Device: POCO F7
- Codename: `onyx`
- ROM: Project Infinity-X 3.12
- Android: 16
- Vendor: `OS3.0.302.0.WOLMIXM`
- EHT hostapd source: Xiaomi.eu `OS3.0.305.0.WOLCNXM`

This is an exact-build module. Do not install it on another device, vendor build, or ROM unless the compatibility checks are updated.

## Stable release

Current stable version: **v1.0.1 Camera-Safe**. Version `v1.0.0` is superseded because its module-wide `/vendor/lib64` mount could break the camera HAL. The old asset is retained only for debugging/history.

## What the module changes

- Enables Android Wi-Fi 7 and 2.4/5/6 GHz framework/SoftAP resource gates.
- Enables up to 320 MHz channel bandwidth where supported by the band, firmware, regulatory domain, router, and client.
- Systemlessly supplies the EHT-capable Xiaomi.eu hostapd. Its uniquely renamed AIDL dependencies are isolated under `/vendor/etc/wifi/hostapd_miui_libs` and loaded through a hostapd-only RUNPATH.
- Does **not** create a module `/vendor/lib64` tree, preventing the global vendor-library shadowing that caused the camera to stop.
- Systemlessly copies the installed WCNSS configuration and sets:
  - `BandCapability=7`
  - `scan_mode_6ghz=1`
  - `oem_6g_support_disable=0`
- Applies a US country-code override after boot so ACS can expose 6 GHz channels.
- Leaves the physical vendor partition untouched.

## Installation

1. Root the exact target build with Magisk.
2. Remove or disable older standalone Wi-Fi 7 RRO/hostapd test modules.
3. Install `POCO-F7-InfinityX-WiFi7-6GHz-v1.0.1-camera-safe-final.zip` in Magisk.
4. Reboot.
5. Use WPA3-Personal for 6 GHz operation.

## Optional: 6 GHz low range / low speed fix

If the 6 GHz hotspot works but has **low range, unstable speed, or reports only `8.00 dBm` TX power** after hotspot OFF/ON, install the optional TX Auto module:

**[Download POCO-F7-6GHz-Hotspot-TX-Auto-v1.0.0.zip](https://github.com/akswap/poco-f7-onyx-infinityx-wifi7-hotspot/releases/download/v1.0.0-tx-auto/POCO-F7-6GHz-Hotspot-TX-Auto-v1.0.0.zip)**

This optional module:

- Detects the active SoftAP interface dynamically.
- Runs only when the hotspot is operating on 6 GHz.
- Reapplies driver-controlled `iw dev <interface> set txpower auto` when the reported power is stuck in the low-power state.
- Does not change 2.4/5 GHz hotspot power, country code, SAR, WCNSS files, overlays, or the Settings APK.
- Restored the tested POCO F7 hotspot from a reported `8.00 dBm` to `24.00 dBm` after hotspot restarts.

After installation and reboot, start the 6 GHz hotspot, wait 5–10 seconds, then verify:

```sh
iw dev wlan1 info
```

The AP interface may use a different name; run `iw dev` to identify the interface whose type is `AP`.

> **Note:** Install this add-on only if you have the low-range/8 dBm issue. The value reported by `iw` is a driver setting, not proof of actual EIRP. Firmware, regulatory rules, antenna gain, thermal policy, and hardware power class may impose lower limits. Use only where permitted by local regulations.

## Camera-safety verification

After reboot, keep Camera open for at least 10–15 seconds, capture a photo, start the hotspot, and repeat the camera test. Both Camera and hotspot were verified working together on the exact target build.

## Verification

Run as root:

```sh
cmd overlay lookup --user 0 com.android.wifi.resources \
com.android.wifi.resources:bool/config_wifi6ghzSupport

cmd overlay lookup --user 0 com.android.wifi.resources \
com.android.wifi.resources:bool/config_wifiSoftap6ghzSupported

cmd overlay lookup --user 0 com.android.wifi.resources \
com.android.wifi.resources:bool/config_wifiSoftapIeee80211beSupported

cmd overlay lookup --user 0 com.android.wifi.resources \
com.android.wifi.resources:bool/config_wifi11beSupportOverride

cmd wifi get-country-code
iw reg get
dumpsys wifi | grep -E "SupportedChannelListIn6g|mCurrentSoftApInfoMap"
```

A connected Wi-Fi 7 client should report `802.11be`. (WiFi 7)

## Rollback

Disable or remove the module in Magisk and reboot. The physical vendor partition is not modified.

## Important warning

The module applies a US regulatory-domain override. Wireless spectrum rules vary by country. Use only where the selected channels and power levels are legally permitted. The module is provided without warranty.

This release contains proprietary vendor binaries extracted from a user-owned Xiaomi.eu ROM for interoperability testing. No ownership is claimed; redistribution may be subject to the original vendor's terms.

## Download integrity

`POCO-F7-InfinityX-WiFi7-6GHz-v1.0.1-camera-safe-final.zip`

SHA-256:

```text
46c122746aac80891b67bdf0257a1732a45a9ba9edcf5924cc846896f823da10
```

## Credits

- Testing and device validation: AKHILESH KUMAR SHUKLA
- Android Wi-Fi resources: AOSP
- EHT hostapd/vendor components: Xiaomi.eu vendor image
