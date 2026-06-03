# Predi's Deadlock Mod Compiler 🛠️

A lightweight, fully automated PowerShell build tool for Deadlock modders. 

If you are tired of manually running csdk compiling your files, tracking changed files, and packing VPKs by hand, this script automates the entire workflow.

## ⭐ Key Features

* **Interactive CLI Menu:** Select which mod to build directly from a simple list.
* **Smart Incremental Builds:** Uses MD5 caching to remember which files you've already compiled. If you only change one `.css` file, it will only compile that single file, saving a massive amount of time.
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
2. The script will scan your workspace and list all available mod folders. Type the corresponding number and press **Enter**.
3. Choose your build destination:
   * **[1] Local:** Saves the compiled `.vpk` inside `tools/builds/`.
   * **[2] Addons:** Directly packs and sends the `.vpk` to your game's `game/citadel/addons/` folder.
4. The script will hash your files, compile the changes, and pack the `.vpk`!

## 🔧 Configuration

After the first launch, the script creates a `data/config.json` file inside the `tools` folder:
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
