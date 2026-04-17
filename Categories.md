# ZOICWARE Script Categories

Functional breakdown of all scripts in `src/` and `zoic_additional_manual/`.

---

## Apps / Debloat
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `debloat()` | Remove Appx packages, Teams, OneDrive, optional features (custom or preset mode) |
| `src/EdgeRemove.ps1` | Remove Microsoft Edge browser and optionally WebView |
| `src/Remove-Locked.ps1` | Remove locked/system Appx packages via policy unlock |
| `src/Unpin.ps1` | Unpin items from the Windows 11 Start menu |
| `src/Disable-AppActions.ps1` | Disable UWP app quick actions via settings.dat registry |
| `src/Set-StoreSettings.ps1` | Configure Microsoft Store behavior via UWP registry |
| `zoic_additional_manual/RemoveWindowsAi.ps1` | Remove Windows AI/Copilot Appx packages |

## Windows Update
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `gpTweaks()` | Disable updates via Group Policy |
| `src/Restore.ps1` | Re-enable Windows Update |
| `zoic_additional_manual/WindowsUpdateManager.ps1` | Full GUI for managing update settings, service state, delivery optimization |

## Defender / Security
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `gpTweaks()` | Disable Defender via Group Policy and registry |
| `src/Restore.ps1` — `enableMsMpEng()` | Re-enable Defender, restore renamed executables, re-enable Tamper Protection |
| `zoic_additional_manual/StripDefenderV3.ps1` | Advanced Defender removal using DISM and TrustedInstaller; supports ISO images |

## Privacy / Telemetry
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `gpTweaks()` | Disable telemetry via Group Policy |
| `src/zFunctions.psm1` — `OptionalTweaks()` | Various privacy settings (activity history, app suggestions, etc.) |
| `src/Disable-AppActions.ps1` | Disable UWP app telemetry features |
| `src/NvidiaAutoinstall.ps1` | Disable Nvidia telemetry and HDCP |
| `zoic_additional_manual/RemoveWindowsAi.ps1` | Remove AI/Copilot telemetry and data collection |

## AI / Copilot
| Script | Description |
|--------|-------------|
| `zoic_additional_manual/RemoveWindowsAi.ps1` | Comprehensive AI/Copilot removal: disable registry keys, remove packages, prevent reinstall, remove Recall/CBS, hide components |

## Nvidia / GPU
| Script | Description |
|--------|-------------|
| `src/NvidiaAutoinstall.ps1` | Auto-detect GPU, download drivers, strip bloat, disable telemetry/HDCP, apply optimized control panel settings, enable legacy sharpen, MSI mode, DDU safe-mode uninstall |

## Network
| Script | Description |
|--------|-------------|
| `src/LocalNetworkInstaller.ps1` | Detect network adapters, install drivers (Realtek, Intel, Killer) locally or from cloud |
| `src/zFunctions.psm1` — `FixUploadBufferBloat()` | Enable QoS for upload priority |
| `src/zFunctions.psm1` — `OptionalTweaks()` | Network tweaks (Nagle, TCP, adapter settings) |

## Power Plans / Performance
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `import-powerplan()` | Import custom power plans, enable hidden plans (Ultimate Performance, Max Overlay), USB power tweaks, AMD-specific options |
| `src/zFunctions.psm1` — `OptionalTweaks()` | HAGS, timer resolution, DPC latency, performance tweaks |
| `zoic_additional_manual/RepairTweaks.ps1` | Validate and repair bad performance tweaks (bcdedit, prefetch, TCP tuning, DPC, timer resolution) |

## Services
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `disable-services()` | Disable 30+ unnecessary services (Bluetooth, Fax, Printer, Tablet, Defrag, Delivery Optimization, etc.) |
| `src/zFunctions.psm1` — `W11Tweaks()` | Set services to manual start |
| `src/zFunctions.psm1` — `remove-tasks()` | Remove non-critical scheduled tasks |
| `src/Restore.ps1` | Re-enable disabled services |

## UI / Shell / Windows 11
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `W11Tweaks()` | Restore Win10 taskbar, explorer, start menu, icons, sounds; remove rounded corners; legacy Notepad/Snipping/Task Manager; dark Winver |
| `src/zFunctions.psm1` — `OptionalTweaks()` | Black theme, mouse schemes, open file dialog warnings |
| `src/Unpin.ps1` | Unpin Start menu items |
| `src/winfetch.psm1` | Neofetch-style system info display |
| `src/RegTweaks.txt` | Registry tweaks for snap layout, taskbar alignment, system requirements bypass |

## Context Menu
| Script | Description |
|--------|-------------|
| `src/UltimateContextMenu/*.reg` | 12+ .reg files: Take Ownership, Run as Admin, Snipping Tool, Shutdown, Kill Tasks, PowerShell/CMD here, Super Delete, dark theme, PS1 options |

## Cleanup / Disk Space
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `UltimateCleanup()` | Clear temp files, event logs, Nvidia driver cache, Windows.old, duplicate drivers, disk cleanup utility |
| `src/zFunctions.psm1` — `install-packs()` | DISM cleanup, Ngen assembly optimization |

## Software Installation
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `install-packs()` | Install DirectX, Visual C++ Redistributables, .NET 3.5 |
| `src/zFunctions.psm1` — `Install-Browsers()` | Download and install Chrome, Firefox, Opera |
| `src/Install-OtherScripts.ps1` | Create desktop shortcuts to companion zoicware tools |

## Drivers
| Script | Description |
|--------|-------------|
| `src/NvidiaAutoinstall.ps1` | Nvidia GPU driver download, strip, and install |
| `src/LocalNetworkInstaller.ps1` | Network adapter driver installation (Realtek, Intel, Killer) |

## Registry
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `import-reg()` | Apply registry tweaks from RegTweaks.txt |
| `src/RegTweaks.txt` | Pre-built registry tweak definitions (core isolation, telemetry, UI, performance) |
| `src/Restore.ps1` | Revert registry modifications |

## Repair / Maintenance
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `Repair-Windows()` | Run SFC/DISM to repair Windows system files |
| `src/zFunctions.psm1` — `Restart-Explorer()` | Kill and restart Windows Explorer |
| `src/zFunctions.psm1` — `Restart-Bios()` | Restart into BIOS/UEFI |
| `zoic_additional_manual/RepairTweaks.ps1` | Detect and fix problematic tweaks applied by other tools |

## Activation
| Script | Description |
|--------|-------------|
| `src/zFunctions.psm1` — `install-key()` | Windows activation with KMS server and generic keys |

## Configuration / Core
| Script | Description |
|--------|-------------|
| `src/ZOICWARE.ps1` | Main entry point — admin check, config loading, WinForms UI |
| `src/RUN ZOICWARE.cmd` | Batch launcher — sets ExecutionPolicy, locates and runs ZOICWARE.ps1 |
| `src/configUI.ps1` | Import/export tweak configurations (drag-drop GUI) |
| `src/RunTweaks.ps1` | Execute tweaks from ZCONFIG.cfg with minimal UI |
| `src/Restore.ps1` | Master restoration GUI for reverting all changes |
