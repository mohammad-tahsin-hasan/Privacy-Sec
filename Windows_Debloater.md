# Windows 10 & Windows 11 Optimization and Debloating Guide

*A practical guide to improving performance, removing unnecessary Microsoft applications, reducing background activity, and improving privacy.*

**Target systems:** Windows 10 and Windows 11 (Home and Pro where noted)  
**Approach:** Moderate debloating  
**Approach:** Shared instructions with version-specific notes where Windows 10 and Windows 11 differ  
**Tools:** PowerShell, Windows Settings, Group Policy (Pro and higher editions), and selected third-party utilities

> **Important:** Make a backup and create a restore point before making system-wide changes. Remove only applications you do not use. Preserve Windows Security, Defender, Windows Update, drivers, networking, and essential system components.

## Compatibility and Windows 10 Support Notice

This guide covers **Windows 10 and Windows 11**. Most PowerShell checks, application-removal concepts, performance recommendations, and the four utilities in Part 4 work across both versions, but individual settings, package availability, and menu names vary by release and edition. Follow the Windows-version note beside an instruction and skip settings that are not present on your PC.

**Windows 10 lifecycle:** Standard support for Windows 10 Home and Pro ended on **October 14, 2025**. As of November 2026, a Windows 10 PC should be enrolled in an applicable Extended Security Updates (ESU) program or moved to a supported Windows release. LTSC and other specialized editions have separate lifecycle dates. Debloating does not extend Microsoft's support or security-update commitment.

**Edition note:** Group Policy Editor (`gpedit.msc`) is normally available on Pro, Enterprise, and Education editions, but not Windows Home. If it is unavailable, use the corresponding Settings page where available; do not download unofficial Group Policy packages.

**Version note:** Windows 10 reached its final feature release at version 22H2 for ordinary Home/Pro servicing. Windows 11 feature names and menus change between releases. Use `winver` to identify your exact version before applying version-specific tweaks.

## Table of Contents

1. [Windows Performance Optimization](#part-1--windows-performance-optimization)
2. [Microsoft Bloatware Removal](#part-2--microsoft-bloatware-removal)
3. [Windows Features and Privacy](#part-3--windows-features-and-privacy)
4. [Debloating, Privacy, and App Installation Tools](#part-4--tools-used-for-debloating-privacy-and-app-installation)
5. [Components You Should Not Remove](#part-5--components-you-should-not-remove)
6. [Recommended Debloating Workflow](#part-6--recommended-debloating-workflow)
7. [Verification and System Integrity](#part-7--verification-and-system-integrity)
8. [Rollback and Recovery](#part-8--rollback-and-recovery)
9. [Quick Reference](#quick-reference)

---

# Part 1 — Windows Performance Optimization

## 1. Disable Unnecessary Startup Programs

Prevent unnecessary applications from launching automatically when Windows starts.

**Steps:**

1. Open Task Manager using `Ctrl + Shift + Esc`.
2. Select **Startup apps**.
3. Review the applications.
4. Disable applications you do not need immediately after startup.

Examples you might disable if unused:

- Microsoft Teams
- OneDrive
- Spotify
- Discord
- Game launchers
- Unnecessary third-party updaters

**PowerShell — list startup applications:**

```powershell
Get-CimInstance Win32_StartupCommand |
    Select-Object Name, Command, Location
```

**Benefit:** Faster boot time and lower background resource usage.

**Caution:** Do not disable security software, essential drivers, or hardware utilities without understanding their purpose.

## 2. Clean Temporary Files

Remove unnecessary temporary files and cached data.

**Steps:**

1. Open **Settings → System → Storage → Temporary files** (Windows 11) or **Settings → System → Storage → Temporary files** (Windows 10; the page layout may differ).
2. Review the available categories.
3. Select the files you want to remove.
4. Click **Remove files**.

You can enable Storage Sense under **Settings → System → Storage → Storage Sense** to automate cleanup.

**PowerShell — check the temporary directory size:**

```powershell
Get-ChildItem $env:TEMP -Force -ErrorAction SilentlyContinue |
    Measure-Object -Property Length -Sum
```

**Benefit:** Frees up disk space.

**Caution:** Review the Downloads category carefully before deleting files.

## 3. Uninstall Unused Applications

Remove applications you no longer use.

**Steps:**

1. Open **Settings → Apps → Installed apps** (Windows 11) or **Settings → Apps → Apps & features** (Windows 10).
2. Review installed applications.
3. Select the three-dot menu beside an application.
4. Choose **Uninstall**.

**PowerShell — list applications managed by WinGet:**

```powershell
winget list
```

**Benefit:** Frees storage and may reduce background processes.

## 4. Update Windows and Drivers

Keep Windows and hardware drivers updated.

**Steps:**

1. Open **Settings → Windows Update** (Windows 11) or **Settings → Update & Security → Windows Update** (Windows 10).
2. Click **Check for updates**.
3. Review available updates.
4. Install appropriate driver updates.

For hardware-specific drivers, use the manufacturer's official website.

Common drivers to check:

- Graphics
- Chipset
- Network
- Audio
- Bluetooth
- Storage controllers

**Benefit:** Improves compatibility, security, and stability.

**Caution:** Avoid unofficial driver-updater utilities.

## 5. Optimize Your Hard Drive

The correct optimization depends on your storage type.

| Storage type | Recommended action |
|---|---|
| HDD | Defragmentation |
| SSD | Windows optimization and TRIM |

**Steps:**

1. Search for **Defragment and Optimize Drives**.
2. Open the utility.
3. Select your drive.
4. Click **Optimize**.

Windows automatically handles the appropriate optimization operation for supported storage devices.

**Benefit:** Helps maintain storage performance.

**Caution:** Do not manually defragment an SSD like a traditional HDD.

## 6. Disable Unnecessary Visual Effects

Reduce graphical effects to improve responsiveness, especially on older computers.

**Steps:**

1. Press `Win + R`.
2. Enter `sysdm.cpl`.
3. Open the **Advanced** tab.
4. Under Performance, click **Settings**.
5. Select **Adjust for best performance**, or manually disable unnecessary effects.

You can retain useful features such as font smoothing while disabling animations and transparency.

**Benefit:** Reduces graphical overhead.

## 7. Check for Malware

Malware can consume system resources and affect performance.

**Steps:**

1. Open **Windows Security**.
2. Select **Virus & threat protection**.
3. Click **Scan options**.
4. Select **Full scan**.
5. Click **Scan now**.

For suspected serious infections, Microsoft Defender Offline is another available scanning option.

**Benefit:** Detects and removes malicious software.

## 8. Use ReadyBoost (Limited Use)

ReadyBoost uses compatible USB flash storage to cache disk data.

**Steps:**

1. Insert a compatible USB drive.
2. Open File Explorer.
3. Right-click the USB drive.
4. Select **Properties**.
5. Open the **ReadyBoost** tab.
6. Select **Use this device**, if available.

**Important:**

- Mainly useful for certain older systems with HDDs and limited RAM.
- Generally offers little or no benefit on modern PCs with SSDs.
- It is not a replacement for physical RAM.

**Benefit:** May improve responsiveness on certain older HDD-based systems.

## 9. Adjust Power Settings

Choose a power mode appropriate for your performance needs.

**Steps:**

1. Open **Windows 11:** Settings → System → Power & battery  
**Windows 10:** Settings → System → Power & sleep (additional power settings are available in Control Panel).
2. Find **Power mode**.
3. Select **Best performance** when maximum performance is needed.

**Benefit:** Allows the system to prioritize performance over energy efficiency.

**Caution:** This may increase electricity consumption, battery usage, and heat.

## 10. Restart Your PC

Restart Windows periodically to clear temporary system states and complete pending updates.

**Steps:**

1. Open Start.
2. Select Power.
3. Click **Restart**.

**Benefit:** Can resolve temporary slowdowns and software issues.

## Recommended Performance Optimization Priority

| Priority | Action | Expected benefit |
|---:|---|---|
| 1 | Disable unnecessary startup apps | Faster startup |
| 2 | Check for malware | Security and performance |
| 3 | Clean temporary files | More storage |
| 4 | Uninstall unused applications | Less software overhead |
| 5 | Update Windows and drivers | Stability and compatibility |
| 6 | Optimize storage | Storage maintenance |
| 7 | Adjust visual effects | Better responsiveness |
| 8 | Adjust power settings | Performance tuning |
| 9 | Restart PC | Resolves temporary issues |
| 10 | ReadyBoost | Limited benefit on older HDD systems |

---

# Part 2 — Microsoft Bloatware Removal

## 1. Microsoft Applications You Can Consider Removing

These applications are generally optional if you do not use them.

| Application | Recommendation |
|---|---|
| Clipchamp | Remove if unused |
| Microsoft Teams (personal) | Remove if unused |
| Microsoft 365 / Office hub | Remove if unused |
| OneNote | Remove if unused |
| Outlook (new) | Remove if unused |
| Microsoft To Do | Remove if unused |
| Maps | Remove if unused |
| News / Microsoft Start | Remove if unused |
| Feedback Hub | Remove if unused |
| Tips | Remove if unused |
| Get Help | Remove if unused |
| People | Remove if unused |
| Xbox applications | Remove if you do not use Xbox services |
| Solitaire Collection | Remove if unused |
| Phone Link | Remove if unused |
| Power Automate | Remove if unused |
| Mixed Reality Portal | Remove if unused and not using VR |

### 1.1 Check Installed Microsoft Applications

Open PowerShell:

```powershell
Get-AppxPackage |
    Select-Object Name |
    Sort-Object Name
```

To filter applications you may want to remove:

```powershell
Get-AppxPackage |
    Where-Object {
        $_.Name -match "Clipchamp|Teams|Outlook|Todos|FeedbackHub|GetHelp|Getstarted|WindowsMaps|People|Solitaire|PowerAutomate|YourPhone|BingNews"
    } |
    Select-Object Name, PackageFullName
```

### 1.2 Remove Selected Applications

Run PowerShell as Administrator. First review the installed package names above. Then edit the list below to include only applications you have confirmed you do not use.

```powershell
$AppsToRemove = @(
    "Clipchamp.Clipchamp",
    "MSTeams",
    "Microsoft.OutlookForWindows",
    "Microsoft.Todos",
    "Microsoft.WindowsFeedbackHub",
    "Microsoft.GetHelp",
    "Microsoft.Getstarted",
    "Microsoft.WindowsMaps",
    "Microsoft.People",
    "Microsoft.MicrosoftSolitaireCollection",
    "Microsoft.PowerAutomateDesktop",
    "Microsoft.YourPhone",
    "Microsoft.BingNews"
)

foreach ($App in $AppsToRemove) {
    Get-AppxPackage -Name $App |
        Remove-AppxPackage -ErrorAction SilentlyContinue
}
```

**Important:**

- Review installed package names before running the script.
- This removes matching applications for the current user.
- Some package names may differ between Windows builds.
- It does not remove every provisioned copy for future user accounts.
- An empty result for a package means it is not installed for that account.

### 1.3 Verify Removal

```powershell
Get-AppxPackage |
    Where-Object {
        $_.Name -match "Clipchamp|MSTeams|OutlookForWindows|Todos|WindowsFeedbackHub"
    } |
    Select-Object Name
```

An empty result means the matching packages are no longer installed for the current account.

## 2. Remove Microsoft Copilot and Unwanted AI Integrations

If you do not use Microsoft's consumer AI features, you can remove the standalone Copilot application and disable unwanted suggestions.

### 2.1 Identify Copilot Applications

```powershell
Get-AppxPackage *Copilot* |
    Select-Object Name, PackageFullName
```

### 2.2 Remove the Standalone Copilot Application

If the standalone package is installed:

```powershell
Get-AppxPackage -Name "Microsoft.Copilot" |
    Remove-AppxPackage
```

Alternatively:

1. Open **Settings → Apps → Installed apps** (Windows 11) or **Settings → Apps → Apps & features** (Windows 10).
2. Search for Copilot.
3. Select the three-dot menu.
4. Click Uninstall if available.

### 2.3 Disable Unwanted AI-Related Features

Review the following settings:

| Location | Recommended action |
|---|---|
| Settings → Personalization → Taskbar | Remove unwanted Copilot taskbar presence |
| Settings → Privacy & security → Diagnostics & feedback | Disable tailored experiences |
| Settings → System → Clipboard | Disable cloud clipboard if unused |
| Settings → Apps → Startup | Disable unwanted AI app startup entries |
| Settings → Personalization → Text input | Disable unwanted suggestions |

If your keyboard has a Copilot key, review its configuration under **Settings → Personalization → Text input**, if supported by your Windows build.

**Important:** Do not remove Windows AI platform components, WebView2, Windows Search, or system drivers simply because their names contain AI-related terms. They may be required by other applications.

## 3. Remove OneDrive

OneDrive can run background synchronization processes. If you do not use cloud synchronization, you can unlink and uninstall it.

### 3.1 Protect Your Files First

Before uninstalling:

- Check Desktop, Documents, and Pictures.
- Ensure important files are downloaded locally.
- Back up important files elsewhere.
- Check whether Windows Known Folder Backup is enabled.

Cloud-only files may not be available locally.

### 3.2 Unlink OneDrive

1. Click the OneDrive cloud icon in the system tray.
2. Open Settings.
3. Select Account.
4. Click **Unlink this PC**.
5. Quit OneDrive.

Unlinking stops synchronization without deleting your cloud account.

### 3.3 Uninstall OneDrive

Use:

**Windows 11:** Settings → Apps → Installed apps → Microsoft OneDrive → Uninstall  
**Windows 10:** Settings → Apps → Apps & features → Microsoft OneDrive → Uninstall

Alternatively:

```powershell
winget list --name OneDrive
```

If WinGet identifies the package correctly:

```powershell
winget uninstall --id Microsoft.OneDrive -e
```

### 3.4 Verify Removal

```powershell
winget list --name OneDrive
```

**Important:** Do not manually delete your OneDrive folder until you have checked its contents.

## 4. Remove Microsoft 365 / Office

If you do not use Microsoft Office applications, you can uninstall them.

### 4.1 Identify Installed Office Applications

```powershell
winget list |
    findstr /i "Microsoft 365 Office Outlook"
```

You can also check:

**Windows 11:** Settings → Apps → Installed apps  
**Windows 10:** Settings → Apps → Apps & features

Look for:

- Microsoft 365
- Microsoft Office
- Microsoft 365 Apps
- Microsoft Office Desktop Apps
- Microsoft 365 Copilot

### 4.2 Uninstall Office

Use:

**Windows 11:** Settings → Apps → Installed apps → Microsoft 365 / Office → Uninstall  
**Windows 10:** Settings → Apps → Apps & features → Microsoft 365 / Office → Uninstall

For the Microsoft Store version, if installed:

```powershell
Get-AppxPackage -Name "Microsoft.Office.Desktop" |
    Remove-AppxPackage
```

For Click-to-Run Office installations, use Settings or Microsoft's official uninstall process.

Official reference: [Uninstall Microsoft 365 or Office from a PC](https://support.microsoft.com/en-us/office/uninstall-office-from-a-pc-9dd49b83-264a-477a-8fcc-2fdf5dbf61d8).

### 4.3 Verify Removal

```powershell
Get-AppxPackage -Name "Microsoft.Office.Desktop"
```

For conventional desktop installations, also check Installed apps.

---

# Part 3 — Windows Features and Privacy

## 1. Windows Features and Recommendations

The availability of Copilot, Widgets, Start recommendations, and AI controls depends on Windows version, region, edition, and installed updates. Windows 10 does not include every Windows 11 feature; do not try to remove a feature that is not present.


| Feature | Recommendation |
|---|---|
| Copilot | Uninstall or disable if unwanted |
| Widgets | Disable if unused |
| OneDrive | Uninstall or unlink if unused |
| Bing search in Start | Disable web search integration if unwanted |
| Advertising ID | Turn off |
| Tailored experiences | Turn off |
| Optional diagnostic data | Turn off |
| Activity history | Disable if unwanted |
| Suggested content in Settings | Disable |
| App launch tracking | Disable if unwanted |

## 2. Reduce Windows Telemetry

Windows 10 Pro and Windows 11 Pro provide Group Policy controls for diagnostic data collection. The exact policy label and available choices depend on the Windows release and edition.

However, it does not officially provide the same Security-only diagnostic data level available to certain Enterprise and Education editions.

For supported Pro editions, choose **Required diagnostic data** (called **Basic** on some older Windows 10 policy templates). The Security-only level is generally reserved for eligible Enterprise, Education, and specialized editions.

### 2.1 Configure Diagnostic Data Using Group Policy

1. Press `Win + R`.
2. Enter:

```text
gpedit.msc
```

3. Navigate to:

```text
Computer Configuration
  → Administrative Templates
    → Windows Components
      → Data Collection and Preview Builds
```

4. Open **Allow diagnostic data** (called **Allow Telemetry** in older Windows 10 templates).
5. Select **Enabled**.
6. Choose **Send required diagnostic data**.
7. Click Apply → OK.

### 2.2 Disable Tailored Experiences

Navigate to:

- **Windows 11:** Settings → Privacy & security → Diagnostics & feedback
- **Windows 10:** Settings → Privacy → Diagnostics & feedback

Recommended settings:

| Setting | Recommendation |
|---|---|
| Send optional diagnostic data | Off |
| Tailored experiences | Off |
| Improve inking and typing | Off, if unused |
| Feedback frequency | Never |
| Diagnostic Data Viewer | Off, unless needed |

You can also delete previously collected diagnostic data from this page.

### 2.3 Configure Diagnostic Data Through PowerShell

Alternatively, open Administrator PowerShell:

```powershell
New-Item `
    -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection" `
    -Force

New-ItemProperty `
    -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection" `
    -Name "AllowTelemetry" `
    -PropertyType DWord `
    -Value 1 `
    -Force
```

Verify:

```powershell
Get-ItemProperty `
    -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection" `
    -Name AllowTelemetry
```

Restart Windows after changing the policy.

**Important:** This configures Required diagnostic data where the policy is supported. It does not disable all Windows data collection.

## 3. Disable Advertising Personalization

Navigate to:

- **Windows 11:** Settings → Privacy & security → General
- **Windows 10:** Settings → Privacy → General

Turn off the available options for:

- Personalized advertising using advertising ID.
- Locally relevant website content using language lists.
- App launch tracking.
- Suggested content in Settings.

The wording and availability of these options may vary by Windows build.

## 4. Review Activity and App Permissions

Navigate to:

- **Windows 11:** Settings → Privacy & security
- **Windows 10:** Settings → Privacy

| Setting | Recommended action |
|---|---|
| General | Disable unwanted personalization |
| Speech | Turn off online speech recognition if unused |
| Inking & typing personalization | Turn off if unused |
| Diagnostics & feedback | Use Required diagnostic data |
| Location | Restrict unnecessary applications |
| Camera | Review app permissions |
| Microphone | Review app permissions |
| Contacts | Restrict unnecessary app access |
| Calendar | Restrict unnecessary app access |
| Account info | Restrict unnecessary app access |

Do not disable hardware permissions globally if you rely on them.

## 5. Disable Unnecessary Background Activity

**Windows 10 note:** For supported Store apps, background permissions are under **Settings → Privacy → Background apps**. Windows 11 manages background activity differently; many apps expose per-app controls under **Settings → Apps → Installed apps → Advanced options**, but this option is not available for every app. Traditional desktop programs may manage their own background activity.


### 5.1 Disable Startup Applications

Open Task Manager using `Ctrl + Shift + Esc`.

Go to **Startup apps** and disable unnecessary applications.

### 5.2 Restrict Background App Permissions

Navigate to:

**Windows 11:** Settings → Apps → Installed apps  
**Windows 10:** Settings → Apps → Apps & features

For supported applications:

- **Windows 11:** Select the three-dot menu → Advanced options.
- **Windows 10:** Select the application in Apps & features → Advanced options.

If **Background apps permissions** is available, select **Never** when you do not need background operation. Not every application exposes this control.

Not all desktop applications provide this option.

### 5.3 Disable Unwanted Notifications

Navigate to:

**Settings → System → Notifications**

Disable unnecessary application notifications and promotional messages.

### 5.4 Disable Widgets

Navigate to:

**Windows 11:** Settings → Personalization → Taskbar → Widgets  
**Windows 10:** There is no equivalent Widgets taskbar switch; disable News and interests from the taskbar context menu if you do not use it.

Turn Widgets off if unused.

This removes the taskbar entry but does not uninstall the underlying Windows components.

## 6. Disable Unwanted Windows Consumer Suggestions

Review:

**Windows 11:** Settings → Personalization → Start  
**Windows 10:** Settings → Personalization → Start

Disable unwanted recommendations and suggestions, such as:

- Recently added apps.
- Recently opened items.
- Recommendations.
- Suggested content.

Also review:

**Windows 11:** Settings → System → Notifications → Additional settings  
**Windows 10:** Settings → System → Notifications & actions

Disable welcome experiences and suggestions after updates if available.

---

# Part 4 — Tools Used for Debloating, Privacy, and App Installation

The following tools serve different purposes. They are not interchangeable, and none should be treated as a one-click performance booster.

## 1. Raphire Win11Debloat

**Compatibility:** The project documents support for Windows 10 and Windows 11. Individual tweaks may be specific to one version/build; review its current README and options before applying them. The project name does not mean it is limited to Windows 11.

**Purpose:** Remove selected preinstalled applications and apply Windows configuration tweaks.

- PowerShell-based, open-source debloating project.
- Review its available options and configuration before applying changes.
- Choose only the app removals and tweaks that match your requirements.
- Avoid aggressive presets and options you do not understand.

Project: [Raphire Win11Debloat on GitHub](https://github.com/Raphire/Win11Debloat)

## 2. Remove Windows AI — Zoicware

**Compatibility:** The project documents support for Windows 10 and Windows 11 on supported builds. Feature coverage varies by build and edition; some components may not be removable on every edition.

**Purpose:** Remove or disable selected Windows AI and Copilot-related components.

- Review the project's current supported features and Windows-version compatibility.
- Read the script and its documentation before running it.
- Create a restore point and backup important files first.
- Do not assume every AI-related component can or should be removed. Some Windows features and third-party applications may depend on shared components.

Project: [RemoveWindowsAI on GitHub](https://github.com/zoicware/RemoveWindowsAI)

## 3. O&O ShutUp10++

**Compatibility:** The vendor lists compatibility with Windows 10 and Windows 11. Available recommendations depend on the installed version and Windows build.

**Purpose:** Configure Windows privacy and reduce selected data collection, recommendations, and background-related settings.

- Review each setting rather than applying every option blindly.
- Prefer the tool's recommended settings as a starting point.
- Read the warning or explanation shown for each option.
- Some changes may affect diagnostics, app behavior, or Windows features; keep a record of what you change.

Official website: [O&O ShutUp10++](https://www.oo-software.com/en/shutup10)

## 4. Ninite

**Compatibility:** Ninite installs and updates supported third-party desktop applications on Windows 10 and Windows 11. The individual application list and support requirements can change.

**Purpose:** Install or update multiple common desktop applications in one installer.

- Select the applications you actually need.
- Download the generated installer from Ninite's official website.
- Run it to install or update the selected applications.
- Ninite is an application installer, **not** a Windows debloater or privacy tool.

Website: [Ninite](https://ninite.com)

## Recommended Order

1. Back up important files and create a restore point.
2. Review Windows settings and uninstall apps you do not use.
3. Use Win11Debloat selectively for app removal and supported configuration tweaks.
4. Use RemoveWindowsAI only for the AI components you have identified and want to remove.
5. Use O&O ShutUp10++ to review privacy-related settings one by one.
6. Use Ninite separately when you want to install or update common applications.
7. Restart Windows and verify that Windows Update, Defender, networking, audio, Bluetooth, and your required applications still work.

### Safety Recommendations

- Download each utility only from the official project or vendor website linked above.
- Check the repository's current instructions; command-line usage and supported options can change between releases.
- Do not run downloaded scripts without reviewing what they do.
- Avoid running multiple debloating/privacy tools with overlapping settings in one session. Make one set of changes, restart, and test before continuing.
- Keep a record of changed settings so you can reverse them where possible.
- A restore point is useful for some system changes, but it is not a substitute for a separate backup of personal files.
- Windows feature updates may restore removed apps or reset some settings. Recheck your configuration after major updates.

---

# Part 5 — Components You Should NOT Remove

Be careful with these components.

| Component | Why you should keep it |
|---|---|
| Microsoft Defender | Core malware protection |
| Windows Security | Security management |
| Windows Update | Security and stability updates |
| Microsoft Store | App installation and updates |
| Microsoft Visual C++ Redistributables | Application dependencies |
| .NET Framework and .NET runtimes | Application dependencies |
| Microsoft Edge WebView2 Runtime | Required by many applications |
| Windows Installer | Software installation |
| Windows Search | Search and system integration |
| Windows Audio | Sound functionality |
| DHCP Client | Network connectivity |
| DNS Client | Name resolution |
| Cryptographic Services | Certificates and update validation |
| Task Scheduler | Windows maintenance and scheduled tasks |
| Windows Event Log | Diagnostics and troubleshooting |
| Core Bluetooth and networking services | Hardware and connectivity |

**Avoid bulk-disabling Windows services**, including:

- `DiagTrack`
- `SysMain`
- `WSearch`
- `BITS`
- `DoSvc`

Their usefulness depends on the system and workload. Disabling them can cause unexpected problems without delivering meaningful performance improvements.

---

# Part 6 — Recommended Debloating Workflow

Follow this sequence instead of running every command and tweak at once.

1. **Backup and create a restore point**
   - Back up important personal files.
   - Create a Windows restore point.
   - Export installed application lists.

2. **Remove unnecessary applications**
   - Teams personal.
   - Clipchamp.
   - Outlook (new).
   - Feedback Hub.
   - Tips and Get Help.
   - Other unused applications.

3. **Remove OneDrive and Microsoft 365**
   - Check cloud-synced files.
   - Unlink OneDrive.
   - Uninstall OneDrive.
   - Uninstall unused Office applications.

4. **Remove unwanted Copilot features**
   - Uninstall the standalone Copilot app.
   - Disable unwanted AI suggestions.
   - Review startup entries.

5. **Configure privacy**
   - Use Required diagnostic data.
   - Disable optional diagnostic data.
   - Disable advertising personalization.
   - Review application permissions.

6. **Reduce background activity**
   - Disable unnecessary startup apps.
   - Restrict supported background applications.
   - Disable unwanted notifications and widgets.

7. **Optimize Windows**
   - Clean temporary files.
   - Optimize storage.
   - Review power settings.
   - Adjust unnecessary visual effects.

8. **Restart and verify**
   - Check Windows Update.
   - Check Defender.
   - Test networking, audio, Bluetooth, and installed applications.

---

# Part 7 — Verification and System Integrity

After debloating, restart Windows and perform these checks.

## 1. Check Installed Applications

```powershell
Get-AppxPackage |
    Select-Object Name |
    Sort-Object Name
```

## 2. Check OneDrive

```powershell
winget list --name OneDrive
```

## 3. Check Copilot

```powershell
Get-AppxPackage *Copilot*
```

## 4. Check Microsoft 365 Store Package

```powershell
Get-AppxPackage -Name "Microsoft.Office.Desktop"
```

## 5. Check Startup Applications

```powershell
Get-CimInstance Win32_StartupCommand |
    Select-Object Name, Command, Location
```

## 6. Check Diagnostic Policy

```powershell
Get-ItemProperty `
    "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection" `
    -Name AllowTelemetry
```

## 7. Check Windows System Integrity

Open Administrator Terminal.

First:

```powershell
DISM /Online /Cleanup-Image /RestoreHealth
```

Then:

```powershell
sfc /scannow
```

These commands check and repair Windows component and system-file integrity. They are not debloating commands.

## 8. Final Testing Checklist

- [ ] Windows Update works.
- [ ] Microsoft Defender works.
- [ ] Microsoft Store opens.
- [ ] Internet connectivity works.
- [ ] Wi-Fi works.
- [ ] Bluetooth works.
- [ ] Audio and microphone work.
- [ ] Camera works.
- [ ] Sleep and shutdown work.
- [ ] Windows Search works.
- [ ] Installed applications open normally.

---

# Part 8 — Rollback and Recovery

Not every debloating change has the same rollback method.

| Change | How to restore |
|---|---|
| Removed Store applications | Reinstall through Microsoft Store |
| Removed Copilot | Reinstall through Microsoft Store if available |
| Removed OneDrive | Download and reinstall from Microsoft |
| Removed Microsoft 365 | Reinstall through your Microsoft account or Office installer |
| Disabled startup apps | Re-enable in Task Manager |
| Changed privacy settings | Turn the settings back on |
| Changed Group Policy | Set the policy to Not Configured |
| Changed telemetry registry | Restore its previous value or remove the policy value |
| Major Windows problems | Use System Restore |

## 1. Roll Back Telemetry Registry Changes

If you used the PowerShell registry method, you can remove the policy value:

```powershell
Remove-ItemProperty `
    -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\DataCollection" `
    -Name "AllowTelemetry" `
    -ErrorAction SilentlyContinue
```

If you used Group Policy, set **Allow diagnostic data** to **Not Configured**.

## 2. Reinstall OneDrive

Official download:

[Download Microsoft OneDrive](https://www.microsoft.com/microsoft-365/onedrive/download)

## 3. Restore Windows Through System Restore

If Windows becomes unstable:

1. Press `Win + R`.
2. Run:

```text
rstrui.exe
```

3. Select your pre-debloat restore point.
4. Follow the restoration wizard.

System Restore can revert system settings and installed applications, but it is not a substitute for a personal-file backup.

---

# Quick Reference

| Purpose | Tool / command |
|---|---|
| List installed applications | `winget list` |
| List Windows app packages | `Get-AppxPackage` |
| List startup applications | `Get-CimInstance Win32_StartupCommand` |
| Check Windows version | `winver` |
| Open Group Policy | `gpedit.msc` |
| Open System Properties | `sysdm.cpl` |
| Open System Restore | `rstrui.exe` |
| Repair Windows image | `DISM /Online /Cleanup-Image /RestoreHealth` |
| Check system files | `sfc /scannow` |
| Clean temporary files | Settings → System → Storage (both; layout varies) |
| Optimize storage | Defragment and Optimize Drives |

## Windows 10 Support and Lifecycle

- [Microsoft: Windows 10 support ended October 14, 2025](https://www.microsoft.com/windows/end-of-support)
- [Microsoft Lifecycle: Windows 10 Home and Pro](https://learn.microsoft.com/en-us/lifecycle/products/windows-10-home-and-pro)

## Official Microsoft Resources

- [Windows diagnostics, feedback, and privacy](https://support.microsoft.com/en-us/windows/privacy/diagnostics-feedback-and-privacy-in-windows)
- [Microsoft Copilot support](https://support.microsoft.com/en-us/copilot)
- [Uninstall Microsoft 365 or Office](https://support.microsoft.com/en-us/office/uninstall-office-from-a-pc-9dd49b83-264a-477a-8fcc-2fdf5dbf61d8)
- [Download OneDrive](https://www.microsoft.com/microsoft-365/onedrive/download)
- [PowerShell Remove-AppxPackage documentation](https://learn.microsoft.com/en-us/powershell/module/appx/remove-appxpackage)
- [Windows diagnostic data policy](https://learn.microsoft.com/en-us/windows/privacy/configure-windows-diagnostic-data-in-your-organization)

---

# Final Recommendations

For Windows 10 or Windows 11, use the following approach. On Windows 10, also make sure the PC has an applicable security-update path because standard Home/Pro support has ended.

| Area | Recommended level |
|---|---|
| Application removal | Moderate |
| Startup optimization | Moderate |
| Privacy configuration | High, within supported settings |
| Telemetry reduction | Required diagnostic data |
| Copilot removal | Remove standalone app if unwanted |
| OneDrive | Uninstall if unused |
| Microsoft 365 | Uninstall if unused |
| Windows services | Leave core services untouched |
| Windows security | Keep enabled |
| System updates | Keep enabled |

**Main principle:** Remove unnecessary applications and disable unwanted features, but preserve the Windows components responsible for security, updates, networking, drivers, and application compatibility.

This gives you a cleaner Windows installation without unnecessarily compromising stability. The safest result comes from selective app removal and reversible settings changes—not from disabling core services or applying every available tweak.
