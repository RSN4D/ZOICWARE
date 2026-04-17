# ZOICWARE Security Audit Report

**Date:** 2026-04-17
**Scope:** Full codebase audit for malicious patterns and prompt injection (excluding expected registry modification and service disabling behavior)
**Audited by:** Claude Code (claude-opus-4-6)

## Findings Summary

**No malicious intent detected. No prompt injection found. No data exfiltration, credential theft, keyloggers, reverse shells, or crypto miners.**

All URLs point to legitimate sources (GitHub/zoicware repos, Microsoft, Nvidia, Intel, Realtek, Mozilla, 7-zip). No data is sent outbound. The codebase does what it claims: Windows 11 optimization and tweaking.

---

## Noteworthy Patterns (not malicious, but worth being aware of)

### 1. Obfuscated Defender toggle code (AveYo snippet)
**Files:** `src/Restore.ps1:1359-1423`, `src/zFunctions.psm1:~3909,~11155`

Uses backtick-escaped .NET reflection (`` DefineDynami`cAssembly ``, `` DefinePInvok`eMethod ``) with explicit comment at line 1384: *"The backtick sprinkles are used to keep ps event log clean"*. This is a well-known open-source snippet from GitHub user AveYo (LeanAndMean) used by many Windows tweaking tools to toggle Defender via low-level Win32 APIs. The obfuscation is to avoid PowerShell event log noise, not to hide from the user.

**Verdict:** Legitimate third-party code, widely used in the Windows tweaking community. Not a sign of malicious intent.

### 2. TrustedInstaller service hijacking for elevated execution
**Files:** `src/zFunctions.psm1:10175-10195` (`Run-Trusted`), `src/EdgeRemove.ps1:172`, `src/NvidiaAutoinstall.ps1:245`

Temporarily reconfigures the TrustedInstaller service `binPath` to run a base64-encoded PowerShell command, then restores the original binPath. This is a standard technique for modifying TrustedInstaller-protected files/registry keys.

**Verdict:** Expected for a system tweaker that needs to modify protected system components. The original binPath is restored immediately after.

### 3. Winlogon Userinit modification for safe-mode DDU execution
**File:** `src/NvidiaAutoinstall.ps1:479-488`

Appends a script to the Winlogon `Userinit` registry value before rebooting into safe mode for DDU (Display Driver Uninstaller). The safe-mode script itself removes the Winlogon entry as its first action (line 483: restores original value).

**Verdict:** Self-cleaning persistence for a single reboot cycle. If the safe-mode script fails to execute, the entry would persist — a minor robustness concern, not malicious.

### 4. System-wide ExecutionPolicy set to Bypass
**File:** `src/RUN ZOICWARE.cmd:5-21`

Sets PowerShell ExecutionPolicy to `Bypass` for both 32/64-bit, and `Unrestricted` if Group Policy restricts it. This change persists after the tool exits.

**Verdict:** Necessary for the tool to function, but it's a permanent system-wide change that isn't reverted. Users should be aware of this. Not malicious — it's the standard approach for PowerShell-based tools that need to run unsigned scripts.

### 5. Remote script execution via `iwr | iex`
**File:** `src/Install-OtherScripts.ps1:208,228,248,268`

Creates desktop shortcuts that download and execute scripts from `raw.githubusercontent.com/zoicware/*` repos. All URLs point to the same author's repositories (WindowsUpdateManager, DefenderProTools, RepairBadTweaks, RemoveWindowsAI).

**Verdict:** Standard pattern for distributing companion tools. All URLs are to the author's own GitHub repos. No integrity verification (hash/signature), which is typical for community PowerShell tools.

### 6. Defender executable renaming (`OFFmeansOFF.exe`)
**File:** `src/Restore.ps1:1401-1408`

When re-enabling Defender, checks if `MpCmdRun.exe` was renamed to `OFFmeansOFF.exe` (done during disabling) and renames it back. This is part of the Defender disable/enable toggle — the disabling side renames the exe to prevent it from running, and the restore side puts it back.

**Verdict:** Part of the Defender toggle mechanism. Aggressive but not malicious — it's designed to be reversible.

---

## Prompt Injection Check

Scanned all files (`.ps1`, `.psm1`, `.cmd`, `.md`, `.txt`, `.reg`, `.cfg`, `.json`, hidden files) for:
- AI manipulation phrases ("ignore previous", "you are now", "system prompt", etc.)
- Zero-width characters or Unicode tricks
- Hidden instructions in comments
- Encoded payloads in non-code files

**Result: No prompt injection attempts found anywhere in the codebase.**

---

## Companion Scripts Audit (`zoic_additional_manual/`)

**Scripts evaluated:** `WindowsUpdateManager.ps1`, `StripDefenderV3.ps1`, `RepairTweaks.ps1`, `RemoveWindowsAi.ps1`
**Source:** Downloaded from `raw.githubusercontent.com/zoicware/*` repos referenced in `Install-OtherScripts.ps1`

**Result: All 4 scripts are clean. No malicious code or prompt injection found.**

All external URLs contact legitimate sources only (GitHub/zoicware repos, `go.microsoft.com` for ADK, `store.rg-adguard.net` for Windows packages). No data is sent outbound. No credential harvesting, keyloggers, reverse shells, crypto miners, firewall changes, or user account manipulation.

### Noteworthy patterns in companion scripts

#### 7. Self-bootstrapping admin elevation via `irm | scriptblock`
**File:** `RemoveWindowsAi.ps1:62`

When the script is not running as admin, it re-launches itself by downloading its own source from GitHub and executing it with elevated privileges:
```powershell
irm 'https://raw.githubusercontent.com/zoicware/RemoveWindowsAI/main/RemoveWindowsAi.ps1' | & ([scriptblock]::Create(...))
```

**Verdict:** Downloads from the same repo the script lives in — this is a self-elevation bootstrap, not a malware delivery mechanism. Standard pattern, low risk.

#### 8. TrustedInstaller elevation with hidden window
**Files:** `RemoveWindowsAi.ps1:204-245`, `StripDefenderV3.ps1:10-27`

Both scripts use the same TrustedInstaller service `binPath` hijack pattern as the main ZOICWARE codebase. `RemoveWindowsAi.ps1` additionally runs the elevated PowerShell with `-win hidden` flag.

**Verdict:** Hidden window is cosmetic (avoids flashing console windows during background operations). Same legitimate TrustedInstaller pattern documented in finding #2.

#### 9. Scheduled task for update cleanup
**File:** `RemoveWindowsAi.ps1:~3702`

Creates a scheduled task (`Update-Cleanup-Check`) that runs at logon to check if Windows has been updated and re-applies AI removal settings if needed.

**Verdict:** Legitimate maintenance task to persist the tool's changes across Windows updates. Transparently documented in the script's feature list.

---

## What the codebase does NOT do
- No data exfiltration or outbound data transmission
- No credential/password/cookie harvesting
- No keylogging or screen capture
- No reverse shells or remote access
- No cryptocurrency mining
- No wallet/financial data access
- No user account creation or password changes
- No firewall port opening for inbound connections
- No sandbox/VM detection for evasion
- No hidden files or suspicious file names
