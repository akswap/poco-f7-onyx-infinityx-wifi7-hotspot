# POCO F7 (onyx) Wi-Fi 7 + 6 GHz Hotspot (Magisk Module)


Verified Magisk module enabling real **Wi-Fi 7 (802.11be)** and **6 GHz SoftAP** on the POCO F7 (`onyx`) running Project Infinity-X 3.12 with the Global `OS3.0.302.0.WOLMIXM` vendor.


## Verified result


- Protocol: 802.11be
- Band: 6 GHz
- ACS channel: 133
- Link speed: 5188/5188 Mbps
- Security: WPA3-Personal
- Maximum channel bandwidth: 320 MHz enabled
- Client: Intel Wi-Fi 7 BE200 320 MHz


## Exact target


- Device: POCO F7
- Codename: `onyx`
- ROM: Project Infinity-X 3.12
- Android: 16
- Vendor: `OS3.0.302.0.WOLMIXM`
- EHT hostapd source: Xiaomi.eu `OS3.0.305.0.WOLCNXM`


This is an exact-build module. Do not install it on another device, vendor build, or ROM unless the compatibility checks are updated.


## What the module changes


- Enables Android Wi-Fi 7 and 6 GHz framework/SoftAP resource gates.
- Enables up to 320 MHz channel bandwidth where supported by the band, firmware, regulatory domain, and client.
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
3. Install `POCO-F7-WiFi7-6GHz-US-ACS-v0.4-test.zip` in Magisk.
4. Reboot.
5. Use WPA3-Personal for 6 GHz hotspot operation.


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
