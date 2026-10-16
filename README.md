# Shortcut Virus Remover

**One-click cleanup for the "shortcut virus" that hides your files on USB drives and replaces them with shortcuts.**

Made by **Youssef Fathy** for **Widescreen**. Free, MIT license.

## ⬇️ Download

**[Download ShortcutVirusRemover.exe](../../releases/latest/download/ShortcutVirusRemover.exe)** (latest version)

## How to use

1. **Before you start:** plug in the infected USB drive(s). **Unplug any other USB drive** that has programs, installers or shortcuts you want to keep (see the warning below).
2. Right-click **ShortcutVirusRemover.exe** → **Properties** → tick **Unblock** → **OK**.
3. Double-click **ShortcutVirusRemover.exe** and click **Yes** when Windows asks for permission.
   > If Windows shows *"Windows protected your PC"*, click **More info → Run anyway**. This appears for any program that isn't from a large publisher.
4. Wait until it says **CLEANUP COMPLETE**, then press any key.
5. If it says a file was scheduled for delete-on-reboot, restart the PC.

## What it does

1. Creates a Windows **System Restore** point first (when Windows allows it).
2. Finds and stops the virus processes that are running (`rundll32` loading a `u<digits>.dll`, anything from `sysvolume`).
3. Deletes the virus Windows service (named `u` + digits).
4. **Moves** the virus DLL from `C:\Windows\System32` into a quarantine folder (`%ProgramData%\ShortcutVirusRemover\Quarantine\`), so it can be restored if it was a mistake.
5. Removes Microsoft Defender exclusions the virus adds for System32 and USB drives.
6. Cleans **every connected USB drive** and **unhides your real folders** so you can see your files again.
7. Turns on protection: disables AutoRun, shows hidden files and file extensions, and enables two Microsoft Defender protection rules.
8. Starts a Microsoft Defender Quick Scan in the background.

Nothing in your personal folders on the C: drive is touched. Everything it does is logged in `%ProgramData%\ShortcutVirusRemover\Logs\`.

## ⚠️ Important: what it deletes on USB drives

On **every** connected USB drive, **whether or not it is infected**, it permanently deletes:

- every `.lnk` (shortcut), `.exe`, `.bat`, `.cmd`, `.vbs`, `.vbe`, `.js`, `.jse`, `.wsf`, `.scr` and `.hta` file **at the top level** of the drive,
- **every shortcut (`.lnk`) in every folder** on the drive,
- files named like `u1234.vbs` / `u1234.dat` / `u1234.bin` anywhere on the drive,
- folders named `sysvolume`, `sysvolume.x86` or `systemvolume` at the top level.

USB drives have no Recycle Bin, so these files can't be recovered. **Unplug any USB drive with programs or shortcuts you want to keep before running it.** Normal documents, photos, videos and folders are not deleted.

## Settings it changes on the PC

These stay after it finishes (there's no automatic undo):

| Setting | Change |
|---|---|
| AutoRun | Turned off for all drives |
| File Explorer | Shows hidden files and file extensions |
| Microsoft Defender rule | *Block JavaScript or VBScript from launching downloaded executable content* |
| Microsoft Defender rule | *Block all Office applications from creating child processes* (can affect some Office add-ins or macros) |

## Source code

The .exe is the PowerShell script [`ShortcutVirusRemover.ps1`](ShortcutVirusRemover.ps1) packed into an .exe with [PS2EXE](https://github.com/MScholtes/PS2EXE). You can read exactly what it does there.

## Notes

- Requires Windows 10 or 11 and administrator permission.
- Recommended afterwards: run a **Microsoft Defender Offline scan** (Windows Security → Virus & threat protection → Scan options), and format the USB drive once you've copied your files off it.
- Some antivirus programs flag PS2EXE-made .exe files as suspicious even when they're harmless. The source script is included so anyone can check it.
- Provided as-is, without warranty (see [LICENSE](LICENSE)).
