# POCO F7 (onyx) Wi-Fi 7 + 6 GHz Hotspot (Magisk Module)

Verified Magisk module enabling real **Wi-Fi 7 (802.11be)**, standalone **6 GHz Wi-Fi**, **5 GHz + 6 GHz MLO**, and **6 GHz SoftAP** on the POCO F7 (`onyx`) running Project Infinity-X 3.12 with the Global `OS3.0.302.0.WOLMIXM` vendor.

## Verified result

- Protocol: 802.11be (WiFi 7)
- Band: 6 GHz
- ACS hotspot channel: 37 & 133
- Observed hotspot client link speed: 5188/5188 Mbps
- Security: WPA3-Personal
- Maximum channel bandwidth: 320 MHz enabled
- Client: Intel Wi-Fi 7 BE200 320 MHz

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

Current stable version: **v1.0.0**. The earlier `v0.4-test` release is retained as the verified development record.

## What the module changes

- Enables Android Wi-Fi 7 and 2.4/5/6 GHz framework/SoftAP resource gates.
- Enables up to 320 MHz channel bandwidth where supported by the band, firmware, regulatory domain, router, and client.
- Systemlessly supplies the EHT-capable Xiaomi.eu hostapd and its versioned AIDL libraries.
- Systemlessly copies the installed WCNSS configuration and sets:
  - `BandCapability=7`
  - `scan_mode_6ghz=1`
  - `oem_6g_support_disable=0`
- Applies a US country-code override after boot so ACS can expose 6 GHz channels.
- Leaves the physical vendor partition untouched.

## Installation

1. Root the exact target build with Magisk.
2. Remove or disable older standalone Wi-Fi 7 RRO/hostapd test modules.
3. Install `POCO-F7-WiFi7-6GHz-US-ACS-v1.0.0.zip` in Magisk.
4. Reboot.
5. Use WPA3-Personal for 6 GHz operation.

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

`POCO-F7-WiFi7-6GHz-US-ACS-v1.0.0.zip`

SHA-256:

```text
5271d0d6bbc1adedd860308a8d346d9cc06efaf958e6ae2cb7895e28048dc098
```

## Credits

- Testing and device validation: AKHILESH KUMAR SHUKLA
- Android Wi-Fi resources: AOSP
- EHT hostapd/vendor components: Xiaomi.eu vendor image
