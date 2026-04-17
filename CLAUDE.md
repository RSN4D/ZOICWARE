# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ZOICWARE is a Windows 11 customization and optimization suite written in PowerShell. It provides 200+ tweaks for debloating, registry modifications, driver installation, service management, and system cleanup. All scripts require admin elevation and target Windows 11.

## Architecture

**Hub-and-spoke model** centered on `src/zFunctions.psm1` (11.5k lines):

- **ZOICWARE.ps1** — Main entry point. Checks admin/OS, loads zFunctions module, manages config files (ZCONFIG.cfg, zSettings.cfg), displays WinForms UI menu.
- **zFunctions.psm1** — Core module containing all shared functions: debloat, registry tweaks, policy management, service control, power plans, cleanup, UI utilities, web helpers. Search here first when looking for any utility function.
- **Restore.ps1** — Reverses all modifications (re-enables services, updates, defender, reinstalls apps).
- **configUI.ps1** — Import/export configuration UI with drag-drop support.
- **RunTweaks.ps1** — Executes tweaks from config files with minimal UI.
- **NvidiaAutoinstall.ps1** — NVIDIA driver installer with version selection and DDU integration.
- **Specialized scripts** — EdgeRemove.ps1, Unpin.ps1, Set-StoreSettings.ps1, etc. each handle a specific task.
- **UltimateContextMenu/** — .reg files for custom right-click menu entries.

## Configuration System

- `ZCONFIG.cfg` stored in `%USERPROFILE%` — persistent settings, each tweak tracked as `1`/`0`
- Auto-updated when new options are added to the tool
- Import/export via configUI.ps1 drag-drop UI

## UI Patterns

All dialogs use **dark-themed WinForms**: gradient backgrounds (RGB 61,74,102 → black), DM Mono font, single-click checked listboxes. Custom dialogs are built via functions in zFunctions.psm1 (`Custom-MsgBox`, etc.).

## Key Patterns

- Scripts dot-source or import `zFunctions.psm1` for shared functionality
- Registry operations use .NET methods directly and bulk `RegSetDwords` function
- `Run-Trusted` function elevates to TrustedInstaller for system-protected files
- All scripts check admin rights at startup
- `Search-File` utility for locating files across the system

## Launcher

`src/RUN ZOICWARE.cmd` — batch file entry point that launches ZOICWARE.ps1 with admin elevation.
