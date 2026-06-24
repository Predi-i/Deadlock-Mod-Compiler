# Predi's Deadlock Mod Compiler 🛠️

A lightweight, fully automated PowerShell build tool for Deadlock modders. 

If you are tired of manually running csdk compiling your files, tracking changed files, and packing VPKs by hand, this script automates the entire workflow.

## ⭐ Key Features

* **Interactive CLI Menu:** Select which mod to build directly from a simple list. You can also start or restart Deadlock without leaving the menu.
* **Smart Incremental Builds:** Uses MD5 caching to remember which files you've already compiled. If you only change one `.css` file, it will only compile that single file, saving a massive amount of time.
* **Force Rebuild:** Add `-Force` (or `-f`) to do a full clean rebuild of a single mod when the cache gets out of sync.
* **Batch Processing:** Automatically splits compilation tasks into batches of 25. This prevents Windows character limit crashes when building massive UI or model replacements.
* **Auto-Routing:** Automatically detects your Steam installation, Deadlock directory, and `addons` folder.
* **Safe Vtex Compilation:** Automatically strips unsupported algorithm parameters from `.vtex` files on-the-fly to prevent compiler crashes.

## ⚙️ Prerequisites

You need **Reduced CSDK 12** to compile Source 2 files.
* Download it here: [CSDK 12](https://deadlockmodding.pages.dev/modding-tools/csdk-12)
* Extract it to `C:\Reduced_CSDK_12`. 
*(If you want to keep it somewhere else, run the script once. It will fail but generate a `tools/data/config.json` file where you can set your custom path).*

## 📦 Folder Structure
Put the `tools` folder near your mods like this:

<img width="356" height="215" alt="изображение" src="https://github.com/user-attachments/assets/2aa998a1-5385-4a3b-ac61-747fbf4ce877" />

## 🛠️ How to Use
1. Run `tools/build_mod.bat`.
2. The script will scan your workspace and list all available mod folders. The menu also offers:
   * **[0] Settings** — configure paths and behaviour (see below).
   * **[S] Start Deadlock** / **[R] Restart Deadlock** — launch or relaunch the game.
3. Type the number of the mod you want to build and press **Enter**.
4. Choose your build destination (skipped if set in config):
   * **[1] Local:** Saves the compiled `.vpk` inside `tools/builds/`.
   * **[2] Addons:** Directly packs and sends the `.vpk` to your game's `game/citadel/addons/` folder.
5. The script will hash your files, compile the changes, and pack the `.vpk`!

### Force rebuild
Incremental builds rely on a cache. If something gets out of sync (e.g. a failed
build, renamed files, or stale compiled artifacts), type the mod number followed
by `-Force` (or the short form `-f`) at the menu prompt:

```
Enter the number of the mod to compile: 6 -Force
```

This wipes that mod's temporary build folders and recompiles every file from
scratch. The flag also works as a launch parameter (`build_mod.ps1 -Force`).

## 🔧 Configuration

The easiest way to configure the tool is the **Settings** menu (`[0]` from the main menu):

* **[1] Build destination** — cycles between `Ask`, `Builds`, and `Addons`.
* **[2] Execution mode** — toggles between `BuildOnly` and `BuildAndRestart` (close Deadlock, build, relaunch).
* **[3] Steam path** — manually override the Steam location if auto-detection fails.
* **[4] CSDK path** — manually override the Reduced CSDK location.
* **[5] Wipe CSDK citadel_addons folders** — fully clears both `content\citadel_addons` and `game\citadel_addons` in the CSDK and resets the build cache. Use this for a clean slate across **all** mods (the `-Force` flag only cleans a single mod). You'll be asked to confirm before anything is deleted.

These settings are stored in `data/config.json`, created inside the `tools` folder on first launch:
```json
{
  "BuildDestination": "Ask",
  "ExecutionMode": "BuildOnly",
  "SteamPath": "",
  "CsdkPath": "C:\\Reduced_CSDK_12"
}
```
* **BuildDestination**: Change to `"Builds"` or `"Addons"` to skip the destination prompt.
* **ExecutionMode**: Change to `"BuildAndRestart"` to automatically close Deadlock, build the mod, and launch Deadlock again.
* **SteamPath / CsdkPath**: Manually override paths if auto-detection fails.
