# Debloating Android

**Objective:** A modular guide to reclaiming privacy, security, and control on Android—covering de-Googling, ad removal, and additional hardening steps.

**Notebook heading:** `Privacy & Security → Debloating Android`

---

## Table of Contents

- [Part I: De-Googling Android — Privacy-Friendly Alternatives](#part-i-de-googling-android--privacy-friendly-alternatives)
  - [1. Levels of De-Googling](#1-levels-of-de-googling)
  - [2. App Replacement Toolkit](#2-app-replacement-toolkit)
    - [2.1 Email & Communication](#21-email--communication)
    - [2.2 Cloud Storage & Files](#22-cloud-storage--files)
    - [2.3 Search & Browser](#23-search--browser)
    - [2.4 Photos & Media](#24-photos--media)
    - [2.5 Security & Authentication](#25-security--authentication)
    - [2.6 Maps & Navigation](#26-maps--navigation)
    - [2.7 Productivity & Organization](#27-productivity--organization)
    - [2.8 Translation](#28-translation)
    - [2.9 AI & Local LLMs](#29-ai--local-llms)
    - [2.10 Android App Stores](#210-android-app-stores)
    - [2.11 Keyboard & Voice Input](#211-keyboard--voice-input)
    - [2.12 System Utilities](#212-system-utilities)
  - [3. Step-by-Step Migration Plan](#3-step-by-step-migration-plan)
  - [4. Important Considerations](#4-important-considerations)
  - [5. Supporting Infrastructure](#5-supporting-infrastructure)
  - [6. Technical Limitations](#6-technical-limitations)
- [Part II: Removing Ads from Android](#part-ii-removing-ads-from-android)
  - [1. Xiaomi Phones (MIUI & HyperOS)](#1-xiaomi-phones-miui--hyperos)
    - [1.1 Disable the MSA (MIUI System Ads) App](#11-disable-the-msa-miui-system-ads-app)
    - [1.2 Disable Ad Personalization](#12-disable-ad-personalization)
    - [1.3 Disable Ads in Stock Xiaomi Apps & UI](#13-disable-ads-in-stock-xiaomi-apps--ui)
- [Part III: System-Wide DNS & Firewall Blocking](#part-iii-system-wide-dns--firewall-blocking)
  - [1. Private DNS (DNS-over-TLS)](#1-private-dns-dns-over-tls)
  - [2. On-Device Firewalls & Local VPN Blockers](#2-on-device-firewalls--local-vpn-blockers)
- [Part IV: Rootless Package Debloating (ADB & Shizuku)](#part-iv-rootless-package-debloating-adb--shizuku)
  - [1. PC Method — Universal Android Debloater (UAD-ng)](#1-pc-method--universal-android-debloater-uad-ng)
  - [2. On-Device Method — Shizuku + Canta](#2-on-device-method--shizuku--canta)
  - [3. Freezing Apps Instead of Removing (Hail / Ice Box)](#3-freezing-apps-instead-of-removing-hail--ice-box)
  - [4. Safety & Recovery](#4-safety--recovery)
- [Part V: Android & OEM Privacy & Telemetry Settings](#part-v-android--oem-privacy--telemetry-settings)
  - [1. Advertising ID & Personalization](#1-advertising-id--personalization)
  - [2. Usage, Diagnostics & OEM Telemetry](#2-usage-diagnostics--oem-telemetry)
  - [3. Location, Wi-Fi & Bluetooth Scanning](#3-location-wi-fi--bluetooth-scanning)
  - [4. Permissions & Background Hygiene](#4-permissions--background-hygiene)
- [Part VI: App Isolation & Tracker Control](#part-vi-app-isolation--tracker-control)
  - [1. Work Profile Sandboxing (Shelter / Insular)](#1-work-profile-sandboxing-shelter--insular)
  - [2. Inspecting & Blocking In-App Trackers](#2-inspecting--blocking-in-app-trackers)

---

## Part I: De-Googling Android — Privacy-Friendly Alternatives

### 1. Levels of De-Googling

Before replacing applications, the extent of de-Googling must be determined. Three standard approaches exist:

#### Level A — Stock Android with replaced applications

The least intrusive approach technically. The existing phone and operating system are retained, Google applications are disabled or uninstalled where the system permits (or removed for the current user via **ADB** / **Universal Android Debloater Next Generation (UAD-ng)** / **Shizuku + Canta** without root), and the alternatives in this document are installed. Google Play Services continues to operate in the background; consequently, some telemetry remains.

#### Level B — Custom ROM with microG

A custom Android build is installed that replaces Google Play Services with **microG**, an open-source reimplementation of Google's proprietary libraries. Most applications that depend on Google services (push notifications, mapping SDKs) continue to function. Suitable options include **CalyxOS** (ships with microG preinstalled), **LineageOS** (microG available via companion builds), and **/e/OS**.

#### Level C — Fully de-Googled (GrapheneOS or comparable)

A hardened ROM is installed containing **no Google services whatsoever**, for example **GrapheneOS** (limited to Pixel devices; provides the strongest security hardening and sandboxed exploit protection). Applications relying on Google push notifications (FCM) will not receive background pushes unless the application implements its own push mechanism (or uses **UnifiedPush**). This level offers the highest privacy at the cost of certain application-compatibility trade-offs.

**Recommendation:** Where a supported Pixel device is available, GrapheneOS (Level C) constitutes the most secure route. Where maximum application compatibility is required, Level B with microG represents the optimal compromise.

---

### 2. App Replacement Toolkit

Tools drawn from the established alternatives list are marked ★. Complementary additions are listed without the marker.

#### 2.1 Email & Communication

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Gmail | ★ Tuta Mail | Encrypted email with dedicated applications; transferring an existing address or using a custom domain facilitates migration. |
| WhatsApp | ★ Signal | End-to-end encrypted messaging and calls. Both parties require Signal; encouraging contacts to migrate is generally the most demanding aspect of the process. |
| Discord | ★ Fluxer | Third-party replacement for Discord communication. |

**Additions:**
- **Email clients:** **Proton Mail** is a strong alternative to Tuta Mail; **Thunderbird for Android** (formerly K-9 Mail) works well for standard IMAP/SMTP accounts with OpenPGP support.
- **Messaging & Dialer:** **Molly** (a security-hardened Signal fork) and **Element** (Matrix protocol) are viable options for encrypted chat. For SMS and calls, **Fossify Messages** and **Fossify Phone** provide clean, tracker-free open-source replacements for Google Messages and Google Phone.

#### 2.2 Cloud Storage & Files

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Drive | ★ Proton Drive | Encrypted cloud storage and synchronization. Not a full replacement for every Google Drive collaboration feature (see Section 4). |
| Google Keep | ★ DDocs.new | Browser-based collaborative notes and documents. |

**Additions:** **Syncthing** (or **Syncthing-Fork** on Android, for direct device-to-device folder synchronization with no cloud intermediary) complements Proton Drive effectively; **Joplin**, **Standard Notes**, or **Notesnook** provide stronger local-first note-taking where DDocs proves insufficiently robust. **Nextcloud** offers a full self-hosted suite for files, notes, and collaboration.

#### 2.3 Search & Browser

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Search | ★ Brave Search | Independent search index, usable in any browser and as Brave's default engine. |
| Google Chrome | ★ Brave Browser | Chromium-based with integrated advertisement and tracker blocking. |

**Additions:**
- **Browsers:** On GrapheneOS, **Vanadium** (hardened Chromium) serves as the default browser; **Fennec F-Droid** or **IronFox** (Firefox forks supporting uBlock Origin) are solid Gecko-based Android options, while **Mullvad Browser** or Firefox configured with privacy extensions are appropriate desktop counterparts.
- **Search engines:** **DuckDuckGo**, **Startpage**, and self-hosted **SearXNG** instances are reliable alternatives to Brave Search.

#### 2.4 Photos & Media

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Photos | ★ Ente Photos | Encrypted photo storage and synchronization with on-device machine-learning search. |

**Additions:**
- **Local Gallery:** **Fossify Gallery** provides an offline, ad-free local photo and video manager if cloud sync is not needed.
- **YouTube & Video:** **NewPipe** (a YouTube front-end operating without a Google account) addresses casual YouTube usage where videos remain hosted by Google; **PipePipe**, **LibreTube**, and **Grayjay** are capable alternatives.
- **Audio & Media Players:** **AntennaPod** (FOSS podcast manager) and **VLC** or **mpv-android** (local video/audio playback).

#### 2.5 Security & Authentication

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Password Manager | ★ Bitwarden | Open-source password management with cross-platform synchronization; self-hosting is possible via Vaultwarden. |
| Google Authenticator | ★ Ente Auth | Encrypted two-factor authentication codes with backup and cross-device synchronization (Google Authenticator lacks encrypted synchronization). |
| No VPN | ★ Proton VPN | Fills an absent category with a no-logs, open-source client offering a free tier. |

**Additions:** **KeePassDX** (local-first offline password database, easily synced via Syncthing) and **Aegis Authenticator** (open-source, locally encrypted 2FA app with automated backups) for users who prefer keeping credentials off cloud servers. **Mullvad VPN** and **IVPN** are strong privacy-focused VPN alternatives.

**Migration procedure:** Export passwords from Google (Settings → Password Manager → Export) and import them into Bitwarden. Two-factor accounts are to be migrated individually: each site is added to Ente Auth first, the generated codes are verified, and only then is the previous entry removed.

#### 2.6 Maps & Navigation

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Maps | ★ Organic Maps | Offline-first OpenStreetMap maps with turn-by-turn navigation. |

**Additions:** **CoMaps** (community-driven fork of Organic Maps) and **OsmAnd** (highly detailed OpenStreetMap client with extensive routing, GPX tracking, and plugin support).

**Caveats:** Live traffic data is unavailable, business data is community-maintained, and search performance is weaker for specific business names. Offline maps for the relevant region should be downloaded prior to travel.

#### 2.7 Productivity & Organization

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Calendar | ★ Fossify Calendar | Local-first FOSS calendar application. |
| Google Contacts | ★ Fossify Contacts | Local-first FOSS contacts application. |

**Additions:** **Tasks.org** (open-source to-do and task manager with CalDAV sync) replaces Google Tasks, and **Fossify Clock** / **Fossify Calculator** cover basic utilities.

**Consideration:** The Fossify applications are primarily Android applications — they are not full replacements for Google Calendar or Contacts *cloud synchronization*. Where cross-device synchronization is required, pairing them with **DAVx⁵** (addition) and a CalDAV/CardDAV provider (for example a Nextcloud instance, or an end-to-end encrypted provider offering CalDAV) is necessary.

#### 2.8 Translation

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Translate | ★ LibreTranslate | Self-hostable, open-source translation software. Quality and language coverage vary; adequate for common European languages, weaker elsewhere. |

**Additions:** On-device alternatives merit evaluation as well — Android's native offline translation or open-source front-ends like **SimplyTranslate** / **DeepL** (for higher accuracy when privacy trade-offs are acceptable) can compensate where LibreTranslate's coverage falls short.

#### 2.9 AI & Local LLMs

| Original Service | Alternative | Notes |
| --- | --- | --- |
| ChatGPT | ★ Ollama | A local LLM runtime, not a direct ChatGPT replacement by itself. Open models (Llama, Mistral, Gemma, and comparable) are executed locally with no cloud dependency. |

**Reality assessment:** Mobile hardware is constrained. Ollama performs optimally on a PC or Mac; on Android, `llama.cpp`-based local assistant applications (such as **PocketPal AI** or **ChatterUI**) represent the practical equivalents, and available RAM must be tested (as a rough guideline, a 7-billion-parameter model quantized to 4-bit requires approximately 4–5 GB of free memory). Smaller models (1B–3B) are adequate for everyday queries; frontier-level reasoning is not to be expected.

#### 2.10 Android App Stores

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Google Play Store | ★ Aurora Store | An anonymous front-end client for *accessing Google Play applications* — not an independent marketplace. Permits installation of most Play applications without a Google account. |

**Additions:** **F-Droid** (or **Droid-ify**, a faster Material You F-Droid client) should be installed first (a FOSS application repository — the majority of the tools in this document are available there), followed by **Obtainium** for receiving updates directly from developers' release channels (GitHub, GitLab, etc.). In combination with Aurora Store, these three repositories cover nearly all requirements.

#### 2.11 Keyboard & Voice Input

Replacing **Gboard** is critical when de-Googling, as the keyboard processes everything typed on the device.

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Gboard | HeliBoard | Open-source, completely offline keyboard based on AOSP with customizable layouts, themes, and optional swipe-typing library support. |
| Gboard (Voice Typing) | FUTO Voice Input | Offline, privacy-respecting speech-to-text powered by local Whisper/Conformer models; integrates with HeliBoard and FUTO Keyboard. |

**Additions:** **FUTO Keyboard** (modern offline keyboard with built-in swipe and voice input) and **FlorisBoard** are strong alternatives depending on preference.

#### 2.12 System Utilities

| Original Service | Alternative | Notes |
| --- | --- | --- |
| Files by Google | Material Files / Fossify File Manager | Open-source, clean file managers with no telemetry or cloud upsells. |
| Google Lens / QR | Binary Eye | Fast, open-source barcode and QR scanner with no tracking permissions. |
| Google Fit | Gadgetbridge | Open-source companion app for smartwatches and fitness bands without vendor cloud accounts. |

---

### 3. Step-by-Step Migration Plan

1. **Prepare on a desktop computer:** Export all required data from Google — the Takeout archive (photos, contacts, calendar), the password export, and the two-factor account list.
2. **Install the repository layer first:** F-Droid (or Droid-ify), followed by Bitwarden, Ente Auth, Aurora Store, Obtainium, the Fossify applications, HeliBoard, Organic Maps, and NewPipe.
3. **Establish security foundations:** Import passwords into Bitwarden; migrate two-factor accounts into Ente Auth one at a time and verify each before deletion from Google Authenticator. *This step is to be completed early — loss of two-factor access results in account lockouts.*
4. **Communication:** Configure Tuta Mail (ideally with a custom domain, permitting unrestricted provider changes later), then Signal, and begin migrating contacts incrementally.
5. **Data transfers:** Upload photographs to Ente Photos, synchronize files to Proton Drive, and import contacts and calendar entries via DAVx⁵ with a CalDAV provider (or retain them locally with the Fossify applications).
6. **Daily-use applications:** Designate Brave (or Vanadium) as the default browser, Brave Search as the default engine, HeliBoard as the default keyboard, and Organic Maps for navigation.
7. **Remove Google components:** Disable or uninstall surviving Google applications (Gmail, Maps, Photos, Gboard, and comparable). On Level A, **Universal Android Debloater (UAD-ng)** via ADB or **Shizuku + Canta** on-device can safely remove user-space Google bloatware without root. *Google Play Services is to be retained if Level A was chosen and banking or push-dependent applications are in use.*
8. **(Optional) ROM installation:** If Level B or C is pursued, back up all data, unlock the bootloader, and flash GrapheneOS, CalyxOS, or LineageOS in accordance with the official installation guides, then reinstall applications from F-Droid and Aurora Store and restore data. An afternoon of work should be anticipated.
9. **Verify critical applications:** Test banking applications, government applications, and push notifications. At Level C, consult the GrapheneOS application-compatibility reports before presuming failure — many applications function without Google services.

---

### 4. Important Considerations

- **Aurora Store:** An alternative client for accessing Google Play apps, not an independent app marketplace. Certain paid applications and device-verified installations still require a disposable Google account.
- **Fossify:** Primarily Android applications; not full replacements for Google Calendar or Contacts cloud synchronization — DAVx⁵ with a CalDAV provider is required for that purpose.
- **Organic Maps:** Focuses on offline maps and navigation; no live traffic data and thinner business listings.
- **LibreTranslate:** Self-hostable translation software; translation quality and language availability can vary.
- **Proton Drive:** Cloud storage, not a direct replacement for every Google Drive collaboration feature.
- **Ente Photos:** Offers encrypted photo storage and synchronization.
- **Signal:** End-to-end encrypted messaging; adoption is required on both sides of a conversation.
- **Bitwarden:** Password management with cross-platform synchronization (self-hostable via Vaultwarden).
- **Ollama:** Local LLM runtime, not a direct ChatGPT replacement; open models are run locally without reliance on a cloud AI service.

---

### 5. Supporting Infrastructure

Two structural challenges of full de-Googling warrant explicit attention:

- **Push notifications:** The principal technical obstacle of complete de-Googling. On stock Android and microG, notifications flow through FCM or its microG equivalent; on a no-Google ROM, only applications implementing their own push systems (Signal, Tuta, and most F-Droid applications) or supporting **UnifiedPush** (via distributors like **ntfy** or **Sunup**) deliver background notifications reliably without battery-draining polling.
- **Banking applications:** Most banks require devices passing "Play Integrity" certification. Level A configurations always pass; microG configurations vary; GrapheneOS passes most checks when its sandboxed Play Services are installed, and outcomes otherwise vary by institution.

---

### 6. Technical Limitations

The following limitations are inherent to the approach described in this document and cannot be fully eliminated:

- Third-party replacements for proprietary platforms (Discord → Fluxer) depend on continued upstream compatibility and may break without notice.
- Self-hostable services (LibreTranslate) shift maintenance responsibility onto the user.
- Local LLM output quality is bounded by available hardware and model size, not by the runtime used.
- Encrypted storage providers replicate synchronization but not necessarily the full collaborative feature set of the services they replace.

---

## Part II: Removing Ads from Android

### 1. Xiaomi Phones (MIUI & HyperOS)

> [!NOTE]
> The steps in this section are specifically for **Xiaomi phones** running **MIUI** or **HyperOS** (including Redmi and POCO devices). Applying all of these settings removes the majority—if not 100%—of built-in system and stock-app advertisements.

#### 1.1 Disable the MSA (MIUI System Ads) App

**MSA** stands for *MIUI System Ads* and acts as the central hub for system-wide ads on Xiaomi devices. Revoking its authorization dramatically reduces ad delivery across the OS, though individual stock apps should still be configured afterward.

1. Open the **Settings** app.
2. Select **Fingerprints, face data, and screen lock** (on **MIUI**, tap **Passwords & security**).
3. Go to **Authorization & revocation** and toggle off **msa**.
4. Tap **Revoke** to confirm.

#### 1.2 Disable Ad Personalization

This does not stop Xiaomi from serving ads entirely, but it prevents the system from using your usage data and personal preferences to serve personalized ads.

1. Open the **Settings** app.
2. Select **Fingerprints, face data, and screen lock** (on **MIUI**, tap **Passwords & security**).
3. Go to **Privacy** and tap **Ad services**.
4. Turn off **Personalized ad recommendations**.

#### 1.3 Disable Ads in Stock Xiaomi Apps & UI

##### Mi Browser
1. Open the **Mi Browser** app.
2. Tap the **profile** icon in the bottom-right corner and tap the **Settings** (gear) icon.
3. Select **Privacy & security** and toggle off **Personalized services**.

##### File Manager
> [!NOTE]
> Turning off recommendations directly in File Manager is only available on **MIUI**; on **HyperOS**, you can instead withdraw data-collection consent.

- **On MIUI:**
  1. Open the **File Manager** app.
  2. Tap the **hamburger menu** (three horizontal lines) in the upper-left corner.
  3. Select **Settings** → **About**.
  4. Turn off **Recommendations**.
- **On HyperOS:**
  1. Open the **File Manager** app.
  2. Tap the **three-dot menu** button and choose **Settings**.
  3. Toggle off **Withdrawal of consent** and tap **OK** to confirm.

##### Security (and Cleaner)
1. Launch the **Security** app.
2. Tap the **gear icon** in the upper-right corner.
3. Toggle off **Receive recommendations**.
4. Select **Cleaner** and toggle off **Receive recommendations** there as well.

##### Music
1. Open Xiaomi's **Music** app.
2. Tap the **menu** button in the upper-left corner and choose **Settings**.
3. Select **Advanced settings**.
4. Turn off **Show ads**.

##### Themes
1. Open the **Themes** app.
2. Tap **My account** in the bottom-right corner.
3. Tap the **Settings** icon in the upper-right corner.
4. Toggle off **Show ads**.
5. Toggle off **Personalized recommendations**.

##### Downloads
1. Open the **Downloads** app.
2. Tap the **three-dot menu** button in the top-right corner and choose **Settings**.
3. Turn off **Show recommended content**.

##### Lock Screen (Glance for Mi)
1. Open the **Settings** app.
2. Go to **Lock screen** and tap **Glance for Mi**.
3. Tap **Privacy policy**.
4. Toggle off **Recommendations for you**.

##### ShareMe
*(Ads cannot be completely disabled in ShareMe, but personalized ads can be turned off.)*
1. Open the **ShareMe** app.
2. Tap **More** (menu) in the top-right corner.
3. Tap **About** → **Help & Feedback**.
4. Turn off **Personalized ads**.

##### Home Screen Folders
1. Tap the folder you want to remove promoted apps from.
2. Tap the **folder name** at the top.
3. Turn off **Promoted apps**.

---

## Part III: System-Wide DNS & Firewall Blocking

Network-level filtering blocks ad networks, analytics endpoints, and OEM telemetry domains across all installed applications—without requiring root access.

### 1. Private DNS (DNS-over-TLS)

Android 9 and later natively support **Private DNS**, which encrypts DNS queries and routes them through a filtering resolver without occupying Android's single VPN slot.

1. Open **Settings** → **Network & internet** (or **Connection & sharing** on MIUI/HyperOS) → **Private DNS**.
2. Select **Private DNS provider hostname** and enter a filtering resolver:

| Provider | Hostname / Setup | Notes |
| --- | --- | --- |
| **AdGuard DNS** | `dns.adguard-dns.com` | Zero-setup system-wide ad and tracker blocking. |
| **NextDNS** | `<your-id>.dns.nextdns.io` | Customizable blocklists (including Xiaomi/Samsung native telemetry lists), analytics logs, and per-device profiles. |
| **Control D** | Custom endpoint (from dashboard) | Configurable ad, tracker, and malware filtering. |
| **Quad9** | `dns.quad9.net` | Focuses on security/malware blocking rather than ad blocking. |

> [!NOTE]
> Captive portals (hotel, airport, or café Wi-Fi login pages) may fail to load while Private DNS is active. Temporarily switch Private DNS to **Off** or **Automatic** to sign in, then re-enable it.

### 2. On-Device Firewalls & Local VPN Blockers

Because Android allows only one active VPN connection at a time, local VPN-based firewalls cannot run simultaneously with a traditional privacy VPN (such as Proton VPN or Mullvad), unless the VPN app itself provides ad/tracker DNS filtering.

- **RethinkDNS:** Open-source local VPN combining a per-app firewall with custom DNS-over-HTTPS/DNSCrypt blocklists. Useful for blocking background internet access for specific OEM or offline apps.
- **NetGuard:** Open-source, no-root firewall focused on per-app Wi-Fi and mobile data rules.
- **AdAway:** Supports non-root local VPN filtering (using hosts-based blocklists) as well as root-level `/etc/hosts` modification.

---

## Part IV: Rootless Package Debloating (ADB & Shizuku)

Preinstalled system applications ("bloatware") that cannot be uninstalled through normal Settings can be removed or disabled for the current user (`--user 0`) without unlocking the bootloader or rooting the device.

### 1. PC Method — Universal Android Debloater (UAD-ng)

**Universal Android Debloater Next Generation (UAD-ng)** is a cross-platform desktop tool that connects over **ADB** and classifies preinstalled packages by safety level (*Recommended*, *Advanced*, *Expert*, *Unsafe*).

1. On the phone, enable **Developer options** (`Settings → About phone → tap Build number / OS version 7 times`).
2. Enable **USB debugging** inside Developer options.
3. Connect the phone to a PC via USB, authorize the RSA debugging prompt on the phone, and launch **UAD-ng**.
4. Review package descriptions carefully and uninstall only packages marked **Recommended** (or those you have verified).

### 2. On-Device Method — Shizuku + Canta

If a PC is unavailable, **Shizuku** grants elevated ADB-level privileges on-device via Android's **Wireless debugging** (Android 11+).

1. Install **Shizuku** and **Canta** (both available via F-Droid / GitHub).
2. Enable **Developer options** → **Wireless debugging**, then pair and start Shizuku following its in-app guide.
3. Open **Canta**, grant it Shizuku permission, and select bloatware packages to uninstall (Canta integrates UAD's safety tags and descriptions).

### 3. Freezing Apps Instead of Removing (Hail / Ice Box)

When an application is occasionally required (for example, an OEM camera plugin, banking app, or regional service) but runs persistently in the background, **freezing** (disabling) it is safer than uninstalling:

- **Hail** (F-Droid, works with Shizuku) can freeze selected apps automatically when the screen locks or on demand, preventing background wakeups, telemetry, and battery drain until you open them again.

### 4. Safety & Recovery

> [!WARNING]
> Removing core system dependencies (such as Android System WebView, Package Installer, or essential OEM framework services) can cause bootloops, broken Settings menus, or failed OTA updates. Always check package descriptions before acting.

- **Export a backup list first:** Save your currently installed packages before making changes:
  ```bash
  adb shell pm list packages > packages-backup.txt
  ```
- **Restore an uninstalled system package:** Because `--user 0` removals do not delete the APK from `/system`, you can restore a package at any time without a factory reset:
  ```bash
  adb shell cmd package install-existing <package.name>
  ```
- **Factory reset fallback:** A factory reset restores all system packages removed via `--user 0`.

---

## Part V: Android & OEM Privacy & Telemetry Settings

Even without removing packages, built-in OS toggles significantly reduce background profiling and data collection.

### 1. Advertising ID & Personalization

1. Open **Settings** → **Privacy** (or **Google → Ads** on stock/OEM builds).
2. Tap **Ads**.
3. Select **Delete advertising ID** (on older Android versions without deletion, tap **Reset advertising ID** and enable **Opt out of Ads Personalization**).

### 2. Usage, Diagnostics & OEM Telemetry

- **Google Diagnostics:** Go to `Settings → Privacy → Usage & diagnostics` (or `Google → three-dot menu → Usage & diagnostics`) and toggle it **off**.
- **Android Personalization / Intelligence:** Review `Settings → Privacy → Android System Intelligence` (or *Device Personalization Services*) and clear/disable unused personalization features.
- **OEM Telemetry (e.g., Xiaomi):** Under `Settings → Fingerprints, face data, and screen lock → Privacy` (or `Passwords & security → Privacy`), turn off **User Experience Program** and **Send diagnostic data automatically**.

### 3. Location, Wi-Fi & Bluetooth Scanning

Android can scan for nearby Wi-Fi networks and Bluetooth beacons in the background even when Wi-Fi and Bluetooth are toggled off.

1. Open **Settings** → **Location** → **Location services**.
2. Turn off **Wi-Fi scanning** and **Bluetooth scanning**.
3. In **Settings → Network & internet → Internet**, verify that saved Wi-Fi networks use **Randomized MAC** (`Privacy → Use randomized MAC`) rather than the device's hardware MAC address.

### 4. Permissions & Background Hygiene

- **Auto-revoke permissions:** Ensure **Pause app activity if unused** / **Remove permissions if app isn't used** is enabled for non-critical apps (`App info → Permissions`).
- **Special app access audit:** Periodically review `Settings → Apps → Special app access`—specifically **Usage access**, **Accessibility services**, **Display over other apps**, **Device admin apps**, and **All files access**—and revoke access for any app that does not strictly require it.
- **Lock screen privacy:** Set `Settings → Notifications → Notifications on lock screen` to **Hide sensitive content** (or **Don't show notifications at all**).

---

## Part VI: App Isolation & Tracker Control

When proprietary applications (banking, ride-hailing, shopping, or messaging) cannot be replaced, isolating them limits the data they can harvest from the rest of your device.

### 1. Work Profile Sandboxing (Shelter / Insular)

Android's built-in **Work Profile** creates a separate storage and app-visibility boundary on the same device:

- **Shelter** or **Insular** (both FOSS, available on F-Droid) let you spin up an isolated Work Profile without an enterprise MDM server.
- Apps installed inside the Work Profile cannot read your main profile's contacts, call logs, photos, or list of installed personal apps.
- You can **freeze** or toggle off the entire Work Profile with a single tap when those apps are not in use, stopping all background activity and notifications.

### 2. Inspecting & Blocking In-App Trackers

- **Exodus Privacy** (app or web report) / **App Manager** (FOSS, F-Droid): Scan installed APKs to identify embedded advertising, analytics, and crash-reporting SDKs (such as Google Firebase Analytics, Facebook Audience Network, or Adjust) and inspect requested permissions.
- **TrackerControl** (F-Droid): Uses a local VPN interface to intercept and selectively block in-app tracker connections per app, showing which domains each app attempts to contact in real time.


