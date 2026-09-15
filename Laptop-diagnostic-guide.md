# Laptop Diagnostic Guide - Before Buying (Especially Second-Hand)

A comprehensive checklist of event logs, hardware diagnostics, and manual checks to perform before purchasing any laptop.

---

## TABLE OF CONTENTS

1. [Windows Event Logs](#1-windows-event-logs)
2. [Drive / Storage Health](#2-drive--storage-health)
3. [Battery Diagnostics](#3-battery-diagnostics)
4. [CPU & Thermal Diagnostics](#4-cpu--thermal-diagnostics)
5. [Memory (RAM) Diagnostics](#5-memory-ram-diagnostics)
6. [GPU / Display Checks](#6-gpu--display-checks)
7. [Keyboard & Input Devices](#7-keyboard--input-devices)
8. [Connectivity (WiFi, Bluetooth, Ports, Webcam)](#8-connectivity-wifi-bluetooth-ports-webcam)
9. [Audio / Speakers](#9-audio--speakers)
10. [BIOS / UEFI Checks](#10-bios--uefi-checks)
11. [OS & Software Checks](#11-os--software-checks)
12. [Physical Inspection](#12-physical-inspection)
13. [PowerShell One-Liners](#13-powershell-one-liners)
14. [Third-Party Tools](#14-third-party-tools)
15. [Red Flags Summary](#15-red-flags-summary)
16. [Pre-Purchase Checklist](#pre-purchase-checklist-quick-reference)
 
---
 
## 1. Windows Event Logs

Event logs are the digital footprint of everything that has happened on a computer. They record hardware failures, driver crashes, unexpected shutdowns, and security events. For second-hand laptops, event logs reveal problems the seller may not tell you about.

### How to Open Event Viewer
```
Press Win + R → type "eventvwr.msc" → Enter
```

### How to Filter Event Logs (Step by Step)
1. Open Event Viewer
2. Click on "Windows Logs" → "System" (or "Application")
3. On the right panel, click "Filter Current Log..."
4. In the "Logged" dropdown, select "Last 30 days" or "Custom range"
5. Check the boxes for "Critical" and "Error"
6. Click OK
7. Review each event by clicking on it and reading the "General" tab

### 1.1 System Event Log
**Location:** Windows Logs → System

This log records hardware, driver, and system-level events. This is the most important log to check when buying a second-hand laptop.

#### Power and Shutdown Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 41 | Kernel-Power | System rebooted without clean shutdown (crash, BSOD, or power loss) | CRITICAL |
| 1074 | Kernel-Power | User initiated shutdown/restart (normal) | Info |
| 6008 | EventLog | Previous shutdown was unexpected (confirms crash) | Warning |
| 6006 | EventLog | Event Log service stopped (clean shutdown) | Info |
| 6005 | EventLog | Event Log service started (clean boot) | Info |
| 29 | Kernel-Boot | Fast startup failed (corrupted hibernation data) | Warning |
| 17 | Kernel-Power | Power source switched (AC to battery or vice versa) | Info |

#### Disk and Storage Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 7 | Disk | Device \Device\HarddiskX has a bad block | CRITICAL |
| 11 | Disk | The driver detected a controller error | CRITICAL |
| 15 | Disk | Device not ready | CRITICAL |
| 51 | Disk | An error was detected on \Device\HarddiskX | Warning |
| 1104 | Ntfs | The NTFS file system flagged the volume as dirty | Warning |
| 52 | Ntfs | Windows detected a file system corruption | Warning |
| 134 | Ntfs | A corruption was detected in the file system | CRITICAL |
| 100 | Disk | The device, \Device\HarddiskX, has a bad block | CRITICAL |

#### Hardware Error Events (WHEA)

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 17 | WHEA-Logger | A corrected hardware error occurred (CPU/RAM/Motherboard) | Warning |
| 18 | WHEA-Logger | A correctable machine check exception occurred | Warning |
| 19 | WHEA-Logger | A corrected machine check error | Warning |
| 20 | WHEA-Logger | A corrected machine check error | Warning |
| 47 | WHEA-Logger | An uncorrectable machine check error occurred | CRITICAL |
| 48 | WHEA-Logger | A fatal hardware error has occurred | CRITICAL |
| 52 | WHEA-Logger | PCIe device reported correctable error | Warning |
| 28440 | WHEA-Logger | WHEA error record was truncated | Warning |

#### Blue Screen of Death (BSOD) Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 1001 | BugCheck | BSOD occurred - contains stop code and parameters | CRITICAL |
| 1001 | MemoryDiagnostics-Results | Windows Memory Diagnostic test result | Info/Warning |

**Example BSOD event message:**
```
The computer has rebooted from a bugcheck. The bugcheck was: 0x0000001E
(KMODE_EXCEPTION_NOT_HANDLED). A dump was saved to: C:\WINDOWS\Minidump\091526-12345-01.dmp
```
The stop code (e.g., 0x0000001E) tells you what failed. Search the stop code online to identify the root cause.

#### Network and Driver Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 219 | NDIS | Network adapter warning | Warning |
| 10317 | NDIS | Miniport fatal error (Wi-Fi power transition failure) | Warning |
| 5010 | NDIS | Network adapter returned invalid value to driver | Warning |
| 5002 | NDIS | Network adapter detected error | Warning |
| 2003 | NDIS | Network adapter resource requirements changed | Info |

#### Service and System Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 7000 | Service Control Manager | Service failed to start | Error |
| 7001 | Service Control Manager | Service depends on missing service | Error |
| 7009 | Service Control Manager | Service timed out during startup | Error |
| 7023 | Service Control Manager | Service terminated with error | Error |
| 7026 | Service Control Manager | Following boot-start drivers failed to load | Warning |
| 7030 | Service Control Manager | Service marked as interactive but system does not allow it | Warning |
| 10016 | DCOM | Application-specific permission denied (benign, common) | Warning |

#### Memory Events

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 1201 | MemoryDiagnostics-Results | Memory test completed | Info |
| 1101 | MemoryDiagnostics-Results | Memory test scheduled | Info |
| 1102 | MemoryDiagnostics-Results | Memory test was aborted | Warning |
| 2004 | Resource-Exhaustion-Detector | Windows committed memory exhaustion | Warning |

**How to filter:**
- Right-click on System log → Filter Current Log
- Check "Critical" and "Error"
- Set timeframe to last 6 months if possible
- Look for recurring patterns

**What to look for:**
- Frequent kernel-power events (ID 41) = unstable system, possible bad RAM or PSU
- Repeated disk errors (ID 7, 11, 51, 100) = failing drive
- WHEA errors (ID 17, 18, 47) = CPU, RAM, or motherboard problems
- Multiple BugCheck events (ID 1001) = recurring BSOD
- Service failures (ID 7000-7030) = corrupted drivers or software
- Wi-Fi fatal errors (ID 10317, 5010) = failing wireless adapter

### 1.2 Application Event Log
**Location:** Windows Logs → Application

This log records application crashes, .NET errors, and software problems.

> **Note:** Event ID 1000 appears twice below with different Sources. This is normal - Windows reuses ID 1000 for both Application Error crashes and SideBySide assembly errors. Check the Source column to tell them apart.

| Event ID | Source | Meaning | Severity |
|----------|----------|---------|----------|
| 1000 | Application Error | Application crashed (shows faulting module) | Error |
| 1001 | Windows Error Reporting | Application crash details sent to Microsoft | Info |
| 1002 | Application Hang | Application stopped responding | Warning |
| 1026 | .NET Runtime | .NET application error | Error |
| 8198 | Software Licensing Service | Windows activation failed | Warning |
| 8233 | Software Licensing Service | VL activation attempt failed | Warning |
| 86 | CertificateServicesClient | Certificate enrollment failed (SCEP) | Warning |
| 1000 | SideBySide | Assembly installation/repair needed | Error |
| 1001 | .NET Runtime Optimization | .NET optimization failed | Warning |

**What to look for:**
- Frequent Application Error events (ID 1000) = unstable software, missing DLLs
- Activation failures (ID 8198, 8233) = licensing problems
- Certificate enrollment failures (ID 86) = corporate/domain machine removed from network

### 1.3 Setup Event Log
**Location:** Windows Logs → Setup

> **Note:** Setup log Event IDs vary by Windows version. The IDs below are the most common. Focus on dates and descriptions rather than exact ID numbers.

| Event ID | Meaning |
|----------|---------|
| 2 | System start |
| 3 | System stop |
| 41999 | Windows Setup completed successfully |
| 4 | Windows Setup started |
| 6 | Windows Setup progress |

**What to look for:**
- Shows OS installation and update history
- Multiple recent installations = likely wiped due to problems
- Check if major feature updates were applied
- Recent install date on old laptop = seller wiped it to hide issues

### 1.4 Security Event Log
**Location:** Windows Logs → Security

| Event ID | Meaning | Severity |
|----------|---------|----------|
| 4625 | Failed logon attempt | Warning |
| 4648 | Logon with explicit credentials (runas) | Info |
| 4720 | User account created | Warning (unexpected = suspicious) |
| 4732 | Member added to local group | Warning (unexpected = suspicious) |
| 4726 | User account deleted | Warning |
| 4634 | Account logoff | Info |
| 4647 | User initiated logoff | Info |
| 4672 | Special privileges assigned (admin logon) | Info |

**What to look for:**
- Verify user accounts (unexpected accounts = suspicious)
- Check for failed login attempts (could indicate password issues)
- Look for new account creation (may indicate backdoor)
- Check admin group membership changes

### 1.5 Hardware Events Log
**Location:** Applications and Services Logs → Microsoft → Windows → Hardware-Events

- Contains hardware-specific warnings from Windows Hardware Error Architecture (WHEA)
- Shows manufacturer-reported hardware failures
- Check for recurring hardware error patterns

### 1.6 PowerShell: Query Event Logs

```powershell
# Get all critical and error events from the last 30 days
Get-WinEvent -FilterHashtable @{
    LogName='System'
    Level=1,2  # 1=Critical, 2=Error
    StartTime=(Get-Date).AddDays(-30)
} | Select TimeCreated, Id, Message | Format-Table -AutoSize

# Get events with all severity levels (Critical, Error, Warning)
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2,3; StartTime=(Get-Date).AddDays(-30)} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message | Format-Table -AutoSize

# Count events by ID (to find most common problems)
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-90)} -ErrorAction SilentlyContinue |
    Group-Object Id | Sort-Object Count -Descending | Select Count, Name

# Check for kernel-power (unexpected shutdowns)
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Kernel-Power'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Message | Format-Table -AutoSize

# Check for BSOD events
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1001; ProviderName='Microsoft-Windows-WER-SystemErrorReporting'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Message

# Hardware errors (WHEA)
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message

# Disk errors
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='disk'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message

# Check for event logs from the last 24 hours only (quick check)
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddHours(-24)} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message | Format-Table -AutoSize

# Export event log summary to file
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-30)} -ErrorAction SilentlyContinue |
    Group-Object Id | Sort-Object Count -Descending |
    Select Count, Name, @{N='LastOccurrence';E={$_.Group[0].TimeCreated}} |
    Export-Csv -Path "$env:USERPROFILE\Desktop\EventLogSummary.csv" -NoTypeInformation
```
 
---
 
## 2. Drive / Storage Health

The storage drive is one of the most critical components. A failing drive means data loss and costly replacement. SSDs degrade over time, and HDDs have mechanical parts that wear out.

### 2.1 S.M.A.R.T. Status (Windows Built-in)

```powershell
# Check disk health status
Get-PhysicalDisk | Select FriendlyName, MediaType, HealthStatus, OperationalStatus, Size

# Get detailed disk info via WMIC (deprecated on Windows 11 24H2+, use Get-CimInstance below if wmic is missing)
wmic diskdrive get Model, SerialNumber, Size, Status, InterfaceType

# Modern replacement for wmic (works on Windows 10/11)
Get-CimInstance Win32_DiskDrive | Select Model, SerialNumber, Size, Status, InterfaceType

# Check disk partition style (GPT or MBR)
Get-Partition | Select DiskNumber, PartitionNumber, Type, Size

# Get disk serial number (important for verifying the same drive)
Get-PhysicalDisk | Select FriendlyName, SerialNumber, FirmwareVersion
```

### 2.2 What S.M.A.R.T. Attributes to Check

S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology) tracks drive health. These are the most critical attributes:

#### HDD-Specific Attributes

| Attribute | ID | What It Means | Bad Threshold |
|-----------|----|---------------|---------------|
| Reallocated Sector Count | 5 | Sectors remapped due to damage | > 0 is concerning |
| Current Pending Sector Count | 197 | Sectors waiting to be remapped | > 0 is bad |
| Uncorrectable Sector Count | 198 | Sectors that cannot be fixed | > 0 is bad |
| Power-On Hours | 9 | Total hours drive has been on | > 30,000 hours is high |
| Temperature | 194 | Drive temperature | > 50 degrees C consistently is bad |
| Start/Stop Count | 10 | Number of spindle start/stop cycles | High count = lots of power cycles |
| Seek Error Rate | 1 | Frequency of read/write head seek errors | High value = mechanical wear |
| Spin Retry Count | 11 | Times spindle failed to spin up on first attempt | > 0 is bad |

#### SSD-Specific Attributes

| Attribute | ID | What It Means | Bad Threshold |
|-----------|----|---------------|---------------|
| Wear Leveling Count | 177 | SSD remaining life percentage | < 90% on new drive is bad |
| NAND Writes | 241 | Total data written to SSD | Check vs TBW rating |
| Reallocated NAND Block Count | 196 | Bad NAND blocks remapped | > 0 is concerning |
| Available Reserved Space | 232 | Reserved space for wear leveling | < 10% is bad |
| Program Fail Count | 171 | Failed program operations | > 0 is bad |
| Erase Fail Count | 172 | Failed erase operations | > 0 is bad |
| Unexpected Power Loss Count | 181 | Unexpected power loss events | High count = unreliable |
| Current Temperature | 194 | SSD temperature | > 70 degrees C is bad |

### 2.3 CrystalDiskInfo (Third-Party Tool)

Download and run CrystalDiskInfo (free):
- Health Status should show "Good" (blue)
- "Caution" (yellow) = early warning signs, investigate further
- "Bad" (red) = drive is failing, DO NOT buy

**What CrystalDiskInfo shows:**
- Overall health status
- Temperature
- Power-on hours
- All S.M.A.R.T. attributes with raw values
- Drive interface (SATA, NVMe)
- Transfer mode

### 2.4 Check Drive Type

```powershell
# Check if SSD or HDD
Get-PhysicalDisk | Select FriendlyName, MediaType, BusType
# MediaType: SSD or Unspecified (for NVMe)
# BusType: SATA, NVMe, USB

# Check drive interface speed
Get-PhysicalDisk | Select FriendlyName, BusType, MediaType | Format-Table -AutoSize
```

**Drive types ranked by speed:**
1. NVMe SSD (fastest, 3,500+ MB/s)
2. SATA SSD (500-550 MB/s)
3. HDD (slowest, 80-160 MB/s)

### 2.5 Check Disk Space Usage

```powershell
# Check disk usage for all drives
Get-PSDrive -PSProvider FileSystem | Select Name,
    @{N='Used(GB)';E={[math]::Round($_.Used/1GB,2)}},
    @{N='Free(GB)';E={[math]::Round($_.Free/1GB,2)}}

# More accurate disk space check
Get-WmiObject Win32_LogicalDisk -Filter "DriveType=3" |
    Select DeviceID,
    @{N='SizeGB';E={[math]::Round($_.Size/1GB,2)}},
    @{N='FreeGB';E={[math]::Round($_.FreeSpace/1GB,2)}},
    @{N='PercentFree';E={[math]::Round(($_.FreeSpace/$_.Size)*100,1)}}
```

### 2.6 Additional Disk Health Event IDs

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 7 | Disk | Device has a bad block | CRITICAL |
| 11 | Disk | Driver detected controller error | CRITICAL |
| 15 | Disk | Device not ready | CRITICAL |
| 51 | Disk | Error detected on disk | Warning |
| 52 | Disk | Timeout on disk | Warning |
| 55 | Ntfs | Data attribute mismatch | Warning |
| 1104 | Ntfs | Volume marked as dirty | Warning |

```powershell
# Check for disk errors in event log
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='disk'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message

# Check NTFS file system errors
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Ntfs'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message
```
 
---
 
## 3. Battery Diagnostics

Battery degradation is common in used laptops. A battery with low capacity means you are plugged in all the time. Battery replacement costs between $30-$100 depending on the laptop model.

### 3.1 Generate Battery Report

```powershell
powercfg /batteryreport /output "$env:USERPROFILE\Desktop\battery_report.html"
```

Open the HTML file in a browser. It contains detailed information about battery capacity, usage history, and health.

### 3.2 Key Battery Metrics

| Metric | What to Look For | Good Value |
|--------|------------------|------------|
| Design Capacity | Original capacity when new (fixed value) | Check manufacturer spec |
| Full Charge Capacity | Current maximum capacity | Should be close to Design Capacity |
| Cycle Count | Number of complete charge/discharge cycles | < 300 for good health |
| Battery Health % | Full Charge / Design x 100 | > 80% is acceptable |
| Date of Manufacture | When battery was made | Check if replaced recently |
| Chemistry | Battery type | Li-ion or Li-poly |
| Rated Capacity | Manufacturer rated capacity | Compare to Design Capacity |

### 3.3 Battery Health Calculation

```
Battery Health (%) = (Full Charge Capacity / Design Capacity) x 100
```

| Health Range | Rating | Action |
|--------------|--------|--------|
| 90-100% | Excellent | No action needed |
| 80-89% | Good | Acceptable for purchase |
| 70-79% | Acceptable | Budget for future replacement |
| 50-69% | Poor | Plan to replace soon |
| Below 50% | Dead | Battery needs immediate replacement |

### 3.4 PowerShell Battery Check

```powershell
# Quick battery status
Get-CimInstance -ClassName Win32_Battery | Select
    EstimatedChargeRemaining,
    BatteryStatus,
    Status

# Get battery details
$battery = Get-CimInstance -ClassName Win32_Battery
$battery | Select Name,
    EstimatedChargeRemaining,
    BatteryStatus,
    @{N='Status';E={
        switch($_.BatteryStatus) {
            1 {'Discharging'}
            2 {'On AC Power'}
            3 {'Fully Charged'}
            4 {'Low'}
            5 {'Critical'}
            6 {'Charging'}
            7 {'Charging and High'}
            8 {'Charging and Low'}
            9 {'Charging and Critical'}
            10 {'Undefined'}
            11 {'Not Installed'}
            default {'Unknown'}
        }
    }}
```

### 3.5 Battery Status Codes

| Code | Status |
|------|--------|
| 1 | Discharging (running on battery) |
| 2 | On AC Power (plugged in, not charging) |
| 3 | Fully Charged |
| 4 | Low (below 10%) |
| 5 | Critical (below 5%) |
| 6 | Charging |
| 7 | Charging and High |
| 8 | Charging and Low |
| 9 | Charging and Critical |
| 10 | Undefined |
| 11 | Not Installed |

### 3.6 Battery Warning Signs in Event Log

| Event ID | Provider | Meaning |
|----------|----------|---------|
| 1 | Kernel-Power | System entered sleep/hibernate due to battery |
| 41 | Kernel-Power | Unexpected shutdown (possible battery failure) |

```powershell
# Check for power-related events
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Kernel-Power'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message | Format-Table -AutoSize
```

### 3.7 Battery Physical Warning Signs

- **Swollen battery**: Laptop case bulging, trackpad lifting, keyboard raised - STOP using immediately, fire hazard
- **Battery not detected**: BIOS shows no battery - may be disconnected or dead
- **Rapid discharge**: Battery drops from 100% to 0% in minutes - battery needs replacement
 
---
 
## 4. CPU & Thermal Diagnostics

Overheating CPUs cause throttling (slow performance), unexpected shutdowns, and reduced lifespan. A laptop that runs hot may need thermal paste replacement or fan cleaning.

### 4.1 Check CPU Usage at Idle

```powershell
# CPU info
Get-WmiObject -Class Win32_Processor | Select Name,
    NumberOfCores,
    NumberOfLogicalProcessors,
    MaxClockSpeed,
    CurrentClockSpeed

# Current CPU usage (take 5 samples over 10 seconds)
Get-Counter '\Processor(_Total)\% Processor Time' -SampleInterval 2 -MaxSamples 5

# Check all processes by CPU usage
Get-Process | Sort-Object CPU -Descending | Select -First 15 Name, CPU, Id
```

**What to look for:**
- Idle CPU usage should be less than 10%
- High idle usage means background processes, malware, or failing hardware
- If a process is using high CPU when idle, investigate it

### 4.2 Check for Thermal Throttling and Hardware Errors

```powershell
# Check for WHEA hardware errors (thermal, CPU, RAM errors)
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message

# Check for kernel-power events (unexpected shutdowns often caused by overheating)
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Kernel-Power'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message
```

### 4.3 Temperature Monitoring (Third-Party Tools)

Use **HWMonitor** or **Core Temp** (both free):

| Temperature Range | Status | Action |
|-------------------|--------|--------|
| 30-50 degrees C (idle) | Normal | No action needed |
| 50-70 degrees C (light load) | Normal | No action needed |
| 70-85 degrees C (heavy load) | Acceptable | Monitor if consistently high |
| 85-95 degrees C (heavy load) | Hot | Consider cleaning fans, replacing thermal paste |
| Above 95 degrees C | Dangerous | Stop using, immediate action needed |

**What to check:**
- CPU idle temperature: 30-50 degrees C is normal
- CPU load temperature: less than 85 degrees C is acceptable
- Above 95 degrees C under load = thermal issues
- Check if fan is spinning and audible
- Listen for grinding or rattling noises (fan bearing failure)

### 4.4 CPU Stress Test

Run **Prime95** or **Intel Burn Test** for 15-30 minutes:
- Monitor temperatures continuously
- Check for BSOD or crashes
- Listen for fan noise
- If it crashes or BSODs during stress test, the laptop has a problem

### 4.5 CPU Warning Signs in Event Log

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 17 | WHEA-Logger | Corrected hardware error (may indicate thermal issue) | Warning |
| 18 | WHEA-Logger | Correctable machine check exception | Warning |
| 47 | WHEA-Logger | Uncorrectable error (CPU failure) | CRITICAL |
| 41 | Kernel-Power | Unexpected shutdown (possibly from overheating) | CRITICAL |

```powershell
# Check for CPU-related errors
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Where-Object { $_.Message -like '*processor*' -or $_.Message -like '*CPU*' -or $_.Message -like '*thermal*' }
```
 
---
 
## 5. Memory (RAM) Diagnostics

Faulty RAM causes random crashes, BSODs, and data corruption. RAM problems are difficult to diagnose because they are intermittent.

### 5.1 Check RAM Configuration

```powershell
# RAM details for each stick
Get-WmiObject -Class Win32_PhysicalMemory | Select BankLabel,
    @{N='Capacity(GB)';E={[math]::Round($_.Capacity/1GB,2)}},
    Speed,
    Manufacturer,
    PartNumber,
    SerialNumber

# Total RAM
[System.Math]::Round((Get-WmiObject -Class Win32_ComputerSystem).TotalPhysicalMemory/1GB, 2)

# Memory slots used and maximum capacity
Get-WmiObject -Class Win32_PhysicalMemoryArray | Select MemoryDevices, MaxCapacity

# Check if running in dual-channel mode
Get-CimInstance -ClassName Win32_PhysicalMemoryArray | Select MemoryDevices
```

**What to look for:**
- All slots should be populated for best performance (dual-channel)
- Same speed and manufacturer for all sticks is ideal
- DDR4-3200 is standard for modern laptops, DDR5-4800+ for newer models
- Maximum capacity matches laptop specifications

### 5.2 Windows Memory Diagnostic

```powershell
# Launch Windows Memory Diagnostic (requires restart)
mdsched.exe

# Check if memory diagnostic was run recently
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1201, 1101, 1102; ProviderName='Microsoft-Windows-MemoryDiagnostics-Results'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Message
```

**What the results mean:**
- ID 1201: Memory test completed - no errors
- ID 1101: Memory test scheduled
- ID 1102: Memory test was aborted

### 5.3 What to Look For in RAM

| Item | What to Check | Why It Matters |
|------|---------------|----------------|
| Matched pairs | Same speed, manufacturer, size | Dual-channel mode |
| Maximum capacity | Does it support upgrade? | Future-proofing |
| Speed | DDR4-3200 is standard | Performance |
| ECC vs Non-ECC | Server RAM vs desktop | Compatibility |
| Error count | Any errors from memory diagnostic | Bad RAM |

### 5.4 Check for Memory Leaks

```powershell
# Monitor memory usage over time (6 samples, 10 seconds apart)
Get-Counter '\Memory\Available MBytes', '\Memory\% Committed Bytes In Use' -SampleInterval 10 -MaxSamples 6

# Check current memory usage
Get-WmiObject -Class Win32_OperatingSystem | Select
    @{N='TotalMemoryGB';E={[math]::Round($_.TotalVisibleMemorySize/1MB,2)}},
    @{N='FreeMemoryGB';E={[math]::Round($_.FreePhysicalMemory/1MB,2)}},
    @{N='UsedMemoryGB';E={[math]::Round(($_.TotalVisibleMemorySize - $_.FreePhysicalMemory)/1MB,2)}},
    @{N='UsagePercent';E={[math]::Round(($_.TotalVisibleMemorySize - $_.FreePhysicalMemory)/$_.TotalVisibleMemorySize*100,1)}}
```

### 5.5 RAM Warning Signs in Event Log

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 1201 | MemoryDiagnostics-Results | Memory test completed successfully | Info |
| 1102 | MemoryDiagnostics-Results | Memory test was aborted | Warning |
| 17 | WHEA-Logger | Corrected hardware error (may be RAM) | Warning |
| 18 | WHEA-Logger | Correctable machine check exception | Warning |
| 2004 | Resource-Exhaustion-Detector | Windows committed memory exhaustion | Warning |

```powershell
# Check for memory-related errors
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Where-Object { $_.Message -like '*memory*' -or $_.Message -like '*RAM*' }
```
 
---
 
## 6. GPU / Display Checks

The GPU handles all graphics output. A failing GPU causes screen artifacts, crashes, and display problems.

### 6.1 GPU Information

```powershell
# GPU details
Get-WmiObject -Class Win32_VideoController | Select Name,
    @{N='AdapterRAM(GB)';E={[math]::Round($_.AdapterRAM/1GB,2)}},
    DriverVersion,
    DriverDate,
    Status,
    VideoModeDescription

# Check GPU driver date (should be recent)
Get-WmiObject -Class Win32_VideoController | Select Name, DriverVersion, DriverDate |
    Sort-Object DriverDate -Descending
```

**What to look for:**
- Driver date should be within the last 12 months
- Status should be "OK"
- Dedicated GPU (NVIDIA/AMD) vs integrated (Intel UHD/Iris) matters for gaming/design

### 6.2 Display Tests

1. **Dead pixel test**: Visit a dead pixel test website (search "dead pixel test online")
   - Display solid white, black, red, green, blue screens
   - Look for pixels that stay one color regardless of what is displayed

2. **Backlight bleed**: Display a black screen in a dark room
   - Light leaking from edges is normal for IPS panels
   - Large bright spots indicate damage

3. **Color uniformity**: Display solid colors
   - Look for uneven coloring or dark spots

4. **Resolution check**: Right-click desktop → Display Settings → verify native resolution

### 6.3 GPU Stress Test

Use **FurMark** or **Unigine Heaven**:
- Run for 15-30 minutes
- Monitor temperatures (less than 90 degrees C under load is acceptable)
- Look for artifacts, flickering, or crashes
- Check fan noise (should be smooth, no grinding)

### 6.4 GPU Errors in Event Log

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 41 | Kernel-Power | System crashed (possibly GPU-related) | CRITICAL |
| 1001 | BugCheck | BSOD (may be GPU driver issue) | CRITICAL |
| 17 | WHEA-Logger | Corrected hardware error | Warning |

```powershell
# GPU-related errors in event log
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Where-Object { $_.Message -like '*display*' -or $_.Message -like '*gpu*' -or $_.Message -like '*video*' }
```
 
---
 
## 7. Keyboard & Input Devices
 
### 7.1 Test Every Key
 
1. Open any text editor or browser
2. Visit **keyboardtester.com** or similar
3. Press EVERY key including:
   - Function keys (F1-F12)
   - Arrow keys
   - Enter, Backspace, Space, Tab
   - Ctrl, Alt, Shift combinations
   - Numpad keys (if present)
   - Caps Lock, Num Lock, Scroll Lock indicators
 
### 7.2 Test Trackpad
 
- Check all gestures (pinch, scroll, swipe)
- Test left and right click buttons
- Check for dead zones
- Verify multi-finger gestures work
 
### 7.3 Check Touchscreen (if applicable)
 
- Test all corners and edges
- Check for ghost touches
- Verify multi-touch support
 
---
 
## 8. Connectivity (WiFi, Bluetooth, Ports, Webcam)

A laptop with broken connectivity is frustrating to use. Test every connection method.

### 8.1 WiFi Test

```powershell
# WiFi adapter info
netsh wlan show interfaces

# WiFi networks visible
netsh wlan show networks

# WiFi driver info
Get-NetAdapter | Where-Object {$_.InterfaceDescription -like '*Wireless*'} |
    Select Name, InterfaceDescription, Status, LinkSpeed, MacAddress

# Check WiFi signal strength
netsh wlan show interfaces | findstr "Signal"
```

**Test by:**
- Connecting to a known good network
- Running speed test (speedtest.net)
- Checking signal strength (should be -50 dBm or better)
- Testing at different distances from router
- Downloading a file to check for drops

### 8.2 Bluetooth Test

```powershell
# Bluetooth info
Get-NetAdapter | Where-Object {$_.InterfaceDescription -like '*Bluetooth*'} |
    Select Name, Status, MacAddress

# Check Bluetooth service
Get-Service bthserv | Select Status, StartType
```

### 8.3 USB Port Test

- Test every USB port individually with a known working device
- Check USB 2.0, 3.0, 3.1, Type-C ports
- Verify data transfer works (copy a file, not just power)
- Test with a USB flash drive - should appear in File Explorer

```powershell
# Check USB controllers
Get-WmiObject Win32_USBController | Select Name, Status
Get-WmiObject Win32_USBHub | Select DeviceID, Name, Status
```

### 8.4 HDMI / Display Port Test

- Connect to external display (TV or monitor)
- Check all available video ports (HDMI, DisplayPort, VGA)
- Test different resolutions
- Check audio over HDMI (play a video)
- Verify second display is detected in Display Settings

### 8.5 Audio Ports

- Test headphone jack (plug in headphones, play audio)
- Test microphone input (record voice, play back)
- Check for static or crackling sounds
- Test at different volume levels

### 8.6 Ethernet Port (if applicable)

```powershell
# Ethernet adapter info
Get-NetAdapter | Where-Object {$_.InterfaceDescription -like '*Ethernet*' -or $_.InterfaceDescription -like '*LAN*'} |
    Select Name, Status, LinkSpeed
```

### 8.7 Webcam Test

```powershell
# Check if webcam device is detected
Get-PnpDevice -Class Camera | Select FriendlyName, Status

# Check webcam via WMI
Get-WmiObject Win32_PnPEntity | Where-Object { $_.Name -like '*camera*' -or $_.Name -like '*webcam*' -or $_.Name -like '*video*' } |
    Select Name, Status
```

**Physical test:**
1. Open Camera app (Windows built-in) or any video call app
2. Check image quality
3. Check for dead spots or discoloration
4. Test microphone through the webcam
5. Check if camera indicator light turns on when activated
 
---
 
## 9. Audio / Speakers
 
### 9.1 Test Speakers
 
1. Play different frequencies (bass, mid, treble)
2. Check for distortion at high volume
3. Verify volume controls work
4. Test with different audio sources
 
### 9.2 Test Microphone
 
1. Use Voice Recorder or Sound Recorder
2. Record and playback
3. Check for background noise
4. Test with video call application
 
### 9.3 Audio Drivers
 
```powershell
# Audio devices
Get-WmiObject -Class Win32_SoundDevice | Select Name, Manufacturer, Status
```
 
---
 
## 10. BIOS / UEFI Checks
 
### 10.1 Access BIOS
 
- Restart laptop
- Press manufacturer key during boot:
  - **Dell**: F2
  - **HP**: F10 or Esc
  - **Lenovo**: F2 or Fn+F2
  - **ASUS**: F2 or Del
  - **Acer**: F2 or Del
  - **MSI**: Del
 
### 10.2 BIOS Checks

> **Tip:** You can also check TPM and Secure Boot from inside Windows without rebooting:
> ```powershell
> Get-Tpm | Select TpmPresent, TpmReady
> Confirm-SecureBootUEFI
> ```

| Item | What to Check |
|------|---------------|
| BIOS Version | Is it outdated? Check manufacturer website |
| Boot Order | Verify SSD/HDD is detected |
| CPU Info | Correct model shown |
| RAM Info | All sticks detected, correct size |
| Storage | All drives detected |
| Temperature | BIOS shows CPU temp |
| Secure Boot | Should be enabled |
| TPM | Required for Windows 11 |
| Warranty | Some manufacturers show in BIOS |
 
### 10.3 BIOS Password Warning
 
- **Ask seller to remove BIOS password before purchase**
- BIOS lock can make laptop unusable
- Some laptops have admin passwords that are very difficult to remove
 
---
 
## 11. OS & Software Checks

The operating system condition reveals how the laptop was used and maintained.

### 11.1 Windows Activation

```powershell
# Check Windows activation
slmgr.vbs /dli

# Check detailed activation status
slmgr.vbs /dlv

# Check activation status via WMI
Get-WmiObject -Class SoftwareLicensingProduct |
    Where-Object {$_.LicenseStatus -eq 1} |
    Select Name, LicenseStatus, PartialProductKey
```

**What to look for:**
- LicenseStatus should be 1 (Activated)
- PartialProductKey should match the sticker on the laptop (if present)
- OEM key in BIOS should match the license

### 11.2 Check Windows Edition and Version

```powershell
# Windows version
[System.Environment]::OSVersion.Version
(Get-WmiObject -Class Win32_OperatingSystem).Caption

# Check Windows build number
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").DisplayVersion
```

### 11.3 Check Installed Software

```powershell
# List installed programs (fast method - does not trigger Windows Installer)
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select DisplayName, DisplayVersion, InstallDate |
    Sort InstallDate -Descending

# Also check 32-bit programs on 64-bit Windows
Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* -ErrorAction SilentlyContinue |
    Select DisplayName, DisplayVersion, InstallDate |
    Sort InstallDate -Descending

# Check for remote access software (suspicious on second-hand laptops)
Get-Service | Where-Object { $_.DisplayName -like '*TeamViewer*' -or
    $_.DisplayName -like '*AnyDesk*' -or
    $_.DisplayName -like '*UltraViewer*' -or
    $_.DisplayName -like '*Ammyy*' -or
    $_.DisplayName -like '*Chrome Remote*' }
```

**Warning:** Do NOT use `Get-WmiObject Win32_Product` - it is extremely slow (takes 5-10 minutes) and triggers Windows Installer reconfiguration for every installed program.

### 11.4 Startup Programs

```powershell
# Startup programs
Get-CimInstance -ClassName Win32_StartupCommand |
    Select Name, Command, Location

# Check startup programs via registry (more comprehensive)
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run" -ErrorAction SilentlyContinue
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -ErrorAction SilentlyContinue
```

### 11.5 Windows Update History

```powershell
# Check update history
Get-HotFix | Sort InstalledOn -Descending |
    Select HotFixID, Description, InstalledOn

# Check last 10 updates
Get-HotFix | Sort InstalledOn -Descending -First 10 |
    Select HotFixID, Description, InstalledOn
```

**What to look for:**
- Updates should be recent (within last 3 months)
- Old updates mean the laptop was not maintained
- Security updates are critical for protection

### 11.6 Check for Suspicious Accounts

```powershell
# Local user accounts
Get-LocalUser | Select Name, Enabled, LastLogon, PasswordLastSet

# Check admin group members
Get-LocalGroupMember -Group "Administrators" | Select Name, ObjectClass

# Check for hidden accounts
Get-LocalUser | Where-Object { $_.Enabled -eq $true } | Select Name, Enabled
```

### 11.7 Windows License Warning Signs

| Event ID | Provider | Meaning | Severity |
|----------|----------|---------|----------|
| 8198 | SoftwareLicensingService | Activation failed | Warning |
| 8233 | SoftwareLicensingService | VL activation attempt failed | Warning |
| 12288 | SoftwareLicensingService | Activation timer error | Warning |

```powershell
# Check for activation failures
Get-WinEvent -FilterHashtable @{LogName='Application'; ProviderName='Microsoft-Windows-Security-SPP'} -ErrorAction SilentlyContinue |
    Where-Object { $_.Id -in @(8198, 8233) } |
    Select TimeCreated, Id, Message
```
 
---
 
## 12. Physical Inspection
 
### 12.1 External Check
 
| Item | What to Look For |
|------|------------------|
| Chassis | Cracks, dents, scratches |
| Hinges | Smooth movement, no wobble |
| Screen | Scratches, cracks, dead pixels |
| Keyboard | Loose keys, uneven surface |
| Ports | Bent pins, loose connections |
| Bottom panel | Missing screws, signs of opening |
| Rubber feet | Intact and not worn |
| Charging port | No damage, fits charger snugly |
 
### 12.2 Open the Laptop (if possible)
 
| Item | What to Look For |
|------|------------------|
| Dust | Excessive dust = poor maintenance |
| Fan | Should spin freely, no grinding |
| Thermal paste | Old/dried paste needs replacement |
| Battery | Swollen battery = dangerous |
| RAM slots | Available for upgrade |
| M.2 slots | Available for SSD upgrade |
| Water damage indicators | White = dry, Pink/Red = liquid damage |
 
### 12.3 Smell Test
 
- **Burning smell** = electrical damage
- **Sweet chemical smell** = liquid damage
- **Musty smell** = mold/moisture exposure
 
---
 
## 13. PowerShell One-Liners

Copy and paste these commands for quick diagnostics:

> **Note:** `Get-WmiObject` is used below for compatibility with Windows PowerShell 5.1. On PowerShell 7+, replace it with `Get-CimInstance` (same parameters).

```powershell
# === SYSTEM INFO ===
systeminfo | findstr /C:"Boot Time" /C:"System Model" /C:"System Manufacturer" /C:"BIOS Version" /C:"OS Version"

# === HARDWARE SUMMARY ===
Get-WmiObject -Class Win32_ComputerSystem | Select Manufacturer, Model, TotalPhysicalMemory, NumberOfProcessors
Get-WmiObject -Class Win32_OperatingSystem | Select Caption, Version, BuildNumber, OSArchitecture

# === ALL HARDWARE ===
Get-WmiObject -Class Win32_Processor | Select Name, NumberOfCores, MaxClockSpeed
Get-WmiObject -Class Win32_PhysicalMemory | Select Capacity, Speed, Manufacturer
Get-WmiObject -Class Win32_DiskDrive | Select Model, Size, InterfaceType
Get-PhysicalDisk | Select FriendlyName, MediaType, HealthStatus

# === BATTERY ===
powercfg /batteryreport /output "$env:USERPROFILE\Desktop\battery_report.html"
Get-WmiObject -Class Win32_Battery | Select EstimatedChargeRemaining, BatteryStatus

# === DRIVERS (recently installed) ===
# Note: InstallDate is usually empty, DriverDate is reliable
Get-WmiObject -Class Win32_PnPSignedDriver | Select DriverName, DriverVersion, DriverDate | Sort DriverDate -Descending

# === INSTALLED PROGRAMS (fast method) ===
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select DisplayName, DisplayVersion, InstallDate | Sort InstallDate -Descending

# === EVENT LOG SUMMARY ===
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-30)} -ErrorAction SilentlyContinue |
    Group-Object Id | Sort-Object Count -Descending | Select Count, Name, @{N='LastOccurrence';E={$_.Group[0].TimeCreated}}

# === QUICK HEALTH CHECK ===
Write-Host "=== DISK HEALTH ===" -ForegroundColor Cyan
Get-PhysicalDisk | Select FriendlyName, HealthStatus
Write-Host "`n=== BATTERY ===" -ForegroundColor Cyan
Get-WmiObject -Class Win32_Battery | Select EstimatedChargeRemaining, BatteryStatus
Write-Host "`n=== RECENT ERRORS ===" -ForegroundColor Cyan
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-7)} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message -First 10

# === CHECK FOR SUSPICIOUS REMOTE ACCESS SOFTWARE ===
Get-Service | Where-Object { $_.DisplayName -like '*TeamViewer*' -or
    $_.DisplayName -like '*AnyDesk*' -or
    $_.DisplayName -like '*UltraViewer*' -or
    $_.DisplayName -like '*Ammyy*' } |
    Select DisplayName, Status, StartType

# === CHECK FOR CRITICAL HARDWARE ERRORS ===
Write-Host "=== WHEA HARDWARE ERRORS ===" -ForegroundColor Cyan
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-WHEA-Logger'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message -First 5

Write-Host "`n=== UNEXPECTED SHUTDOWNS ===" -ForegroundColor Cyan
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='Microsoft-Windows-Kernel-Power'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message -First 5

Write-Host "`n=== DISK ERRORS ===" -ForegroundColor Cyan
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='disk'; Level=1,2} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message -First 5

Write-Host "`n=== BSOD EVENTS ===" -ForegroundColor Cyan
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1001; ProviderName='Microsoft-Windows-WER-SystemErrorReporting'} -ErrorAction SilentlyContinue |
    Select TimeCreated, Message -First 5

# === FULL LAPTOP DIAGNOSTIC EXPORT ===
Write-Host "Generating full diagnostic report..." -ForegroundColor Yellow
$report = @()
$report += "LAPTOP DIAGNOSTIC REPORT - $(Get-Date)"
$report += "=" * 50
$report += ""
$report += "SYSTEM INFO:"
$report += Get-CimInstance Win32_ComputerSystem | Select Manufacturer, Model | Format-List | Out-String
$report += "CPU:"
$report += Get-CimInstance Win32_Processor | Select Name, NumberOfCores | Format-List | Out-String
$report += "RAM:"
$report += Get-CimInstance Win32_PhysicalMemory | Select Capacity, Speed, Manufacturer | Format-Table | Out-String
$report += "DISK:"
$report += Get-PhysicalDisk | Select FriendlyName, HealthStatus, Size | Format-Table | Out-String
$report += "BATTERY:"
$report += Get-CimInstance Win32_Battery | Select EstimatedChargeRemaining, BatteryStatus | Format-List | Out-String
$report += "RECENT ERRORS:"
$report += Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-7)} -ErrorAction SilentlyContinue |
    Select TimeCreated, Id, Message -First 20 | Format-Table | Out-String
$report | Out-File "$env:USERPROFILE\Desktop\LaptopDiagnosticReport.txt"
Write-Host "Report saved to Desktop\LaptopDiagnosticReport.txt" -ForegroundColor Green
```
 
---
 
## 14. Third-Party Tools
 
### Free Tools
 
| Tool | Purpose | Website |
|------|---------|---------|
| CrystalDiskInfo | Drive S.M.A.R.T. health | crystalmark.info |
| HWMonitor | Temperature monitoring | cpuid.com/hwmonitor |
| Core Temp | CPU temperature | alcpu.com |
| MemTest86 | RAM testing | memtest86.com |
| FurMark | GPU stress test | geeks3d.com |
| HWiNFO | Comprehensive hardware info | hwinfo.com |
| BatteryBar Pro | Battery health | batterybarpro.com |
| Windows Memory Diagnostic | Built-in RAM test | Windows tool |
| Speccy | System overview | piriform.com/speccy |
 
### Portable Tools (No Install Required)
 
- CrystalDiskInfo Portable
- HWMonitor Portable
- GPU-Z Portable
- CPU-Z Portable
 
---
 
## 15. Red Flags Summary

### DO NOT BUY if you see:

| Red Flag | Why It Matters |
|----------|----------------|
| WHEA errors in event log (ID 17, 18, 47) | Hardware is failing |
| Multiple BSOD events (ID 1001 BugCheck) | System is unstable |
| Frequent unexpected shutdowns (ID 41 Kernel-Power) | Power or hardware failure |
| S.M.A.R.T. status "Bad" | Drive is dying |
| Battery health less than 50% | Needs expensive replacement |
| BIOS password set | May be stolen, hard to remove |
| Excessive dust or corrosion | Poor maintenance or water damage |
| Swollen battery | Fire hazard |
| High idle CPU usage | Malware or hardware issues |
| Missing screws or panels | Unauthorized repairs |
| Burning smell | Electrical damage |
| Grinding fan noise | Fan bearing failure |
| Screen artifacts | GPU failure |
| Keyboard not responding | Potential liquid damage |
| Remote access software installed | Previous owner may have backdoor access |
| Windows not activated or failing activation | Pirated or invalid license |
| Disk errors in event log (ID 7, 11, 51) | Failing storage drive |

### Seller Red Flags (Walk Away)

| Red Flag | Why It Matters |
|----------|----------------|
| Seller refuses to let you run diagnostics | Hiding something |
| Seller rushes you to decide | Pressuring you to not find problems |
| No original charger | May be stolen, charger costs $30-$100 |
| No receipt or proof of purchase | May be stolen |
| Serial number scratched off or missing | Stolen laptop |
| Seller will not remove BIOS password | Laptop may be locked |
| Multiple user accounts on system | Previous owner data still present |
| Laptop was recently wiped (recent install date) | Trying to hide issues |
| Seller cannot answer basic questions about usage | May be selling someone else's laptop |
| Price too good to be true | Usually means something is wrong |

### NICE TO HAVE:

| Feature | Why It Matters |
|---------|----------------|
| SSD instead of HDD | Much faster performance |
| Dual-channel RAM (2 sticks) | Better performance |
| 80%+ battery health | Good battery life remaining |
| NVMe SSD | Faster than SATA SSD |
| USB-C charging | Convenient, universal charger |
| Fingerprint reader | Quick login |
| Backlit keyboard | Useful in dark |
| Original box and accessories | Shows careful owner |
| Warranty still valid | Protection if problems arise |
| Clean event log (no critical errors) | Well-maintained system |
 
---
 
## Pre-Purchase Checklist (Quick Reference)

Print this checklist and bring it when inspecting a laptop:

### Software Checks
- [ ] Check System Event Log for Critical/Error events
- [ ] Check for BSOD (BugCheck) events (ID 1001)
- [ ] Check for unexpected shutdowns (Kernel-Power ID 41)
- [ ] Check for WHEA hardware errors (ID 17, 18, 47)
- [ ] Check for disk errors (ID 7, 11, 51)
- [ ] Check Windows activation status
- [ ] Check for remote access software (TeamViewer, AnyDesk, UltraViewer)
- [ ] Check installed programs for suspicious software
- [ ] Check startup programs
- [ ] Check Windows update history (should be recent)
- [ ] Verify no BIOS password is set

### Hardware Checks
- [ ] Verify disk health (CrystalDiskInfo = Good, no bad sectors)
- [ ] Generate battery report (health greater than 80%)
- [ ] Check battery for swelling (physical inspection)
- [ ] Check CPU idle temperature (30-50 degrees C)
- [ ] Run memory test (no errors)
- [ ] Verify RAM configuration (dual-channel if possible)

### Physical Tests
- [ ] Test all keyboard keys
- [ ] Test trackpad gestures
- [ ] Test all ports (USB, HDMI, audio, charging)
- [ ] Test WiFi and Bluetooth connectivity
- [ ] Test speakers and microphone
- [ ] Test webcam
- [ ] Check for dead pixels on screen
- [ ] Test display at different brightness levels
- [ ] Check for screen backlight bleed

### BIOS and Final Checks
- [ ] Enter BIOS and verify all hardware detected
- [ ] Check BIOS version is current
- [ ] Verify Secure Boot and TPM status
- [ ] Physical inspection (no damage, dust, smells)
- [ ] Test with stress test (CPU + GPU) if possible
- [ ] Check upgrade potential (RAM slots, M.2 slots)
- [ ] Verify original charger is included
- [ ] Check for proof of purchase / receipt
 
---
 
*Last updated: September 2026*
*Created for laptop buyers and technicians*
*Over 100 Event IDs covered for comprehensive diagnostics*
 