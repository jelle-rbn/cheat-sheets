# Powershell Cheat Sheet

**Table of contents**

- [File and Folder Commands](#file-and-folder-commands)
  - [Navigate and list](#navigate-and-list)
  - [Create, copy, move, delete](#create-copy-move-delete)
  - [Read and write files](#read-and-write-files)
  - [Test and measure](#test-and-measure)
  - [Archive](#archive)
- [Process and Service Commands](#process-and-service-commands)
  - [Services](#services)
  - [Processes](#processes)
  - [Scheduled tasks](#scheduled-tasks)
- [Networking and Remote Commands](#networking-and-remote-commands)
  - [Network info](#network-info)
  - [HTTP requests](#http-requests)
  - [Remoting](#remoting)
- [String and Data Manipulation](#string-and-data-manipulation)
  - [String operations](#string-operations)
  - [Format and date](#format-and-date)
  - [JSON / CSV](#json--csv)
  - [Variables and types](#variables-and-types)
- [Error Handling and Logging](#error-handling-and-logging)
  - [Error handling](#error-handling)
  - [Write to output streams](#write-to-output-streams)
  - [Null coalescing (PS 7+)](#null-coalescing-ps-7)
- [Active Directory Quick Reference](#active-directory-quick-reference)
  - [Users](#users)
  - [Groups](#groups)
- [Useful One-Liners](#useful-one-liners)
  - [Find large files](#find-large-files)
  - [Get local admins on a machine](#get-local-admins-on-a-machine)
  - [Find open firewall ports](#find-open-firewall-ports)
  - [Export running services to CSV](#export-running-services-to-csv)
  - [Kill all processes matching a name](#kill-all-processes-matching-a-name)
  - [Get Windows version info](#get-windows-version-info)
  - [Find recently modified files](#find-recently-modified-files)

## File and Folder Commands

The most frequent file system operations used in PowerShell, with the key parameters that matter for scripting.

### Navigate and list

```powershell
Set-Location C:\Logs                         # cd equivalent
Get-Location                                 # print current path (pwd)
Get-ChildItem -Path C:\Logs -Filter *.log -Recurse  # ls -r *.log
```

### Create, copy, move, delete

```powershell
New-Item -Path C:\Logs\app -ItemType Directory -Force`
Copy-Item C:\Source\file.txt C:\Dest\ -Force
Move-Item C:\Old\file.txt C:\New\file.txt
Remove-Item C:\Temp\* -Recurse -Force
```

### Read and write files

```powershell
Get-Content C:\Logs\app.log                  # read all lines
Get-Content C:\Logs\app.log -Tail 50         # last 50 lines
Set-Content  C:\Config\settings.txt 'value'  # overwrite
Add-Content  C:\Logs\script.log 'entry'      # append
Out-File     C:\Reports\out.txt              # write pipeline output
```

### Test and measure

```powershell
Test-Path C:\Logs\app.log                    # Boolean: does it exist?
(Get-Item C:\Logs\app.log).Length            # file size in bytes
Get-Content C:\Data\file.csv | Measure-Object -Line  # count lines
Get-FileHash C:\Installer.exe -Algorithm SHA256
```

### Archive

```powershell
Compress-Archive -Path C:\Logs\* -DestinationPath C:\Archive\logs.zip -Force
Expand-Archive   -Path C:\Archive\logs.zip  -DestinationPath C:\Logs\Expanded
```

---

## Process and Service Commands

Service and process management - the commands for keeping Windows systems running and diagnosing what is consuming resources.

### Services

```powershell
Get-Service                              # list all services
Get-Service -Name W3SVC                  # specific service
Start-Service   W3SVC                    # start
Stop-Service    W3SVC -Force             # stop
Restart-Service W3SVC                    # restart
Set-Service     W3SVC -StartupType Automatic  # change startup type
Get-Service | Where-Object Status -ne Running  # stopped services
```

### Processes

```powershell
Get-Process                              # all processes
Get-Process -Name chrome                 # specific process
Stop-Process -Name notepad -Force        # kill by name
Stop-Process -Id 1234 -Force             # kill by PID
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10  # top 10 CPU
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10  # top 10 RAM
```

### Scheduled tasks

```powershell
Get-ScheduledTask | Where-Object State -eq 'Running'
Start-ScheduledTask -TaskName 'MyTask'
Stop-ScheduledTask  -TaskName 'MyTask'
```

---

## Networking and Remote Commands

Network diagnostics, web requests, and remote execution - the commands for working across systems.

### Network info

```powershell
Get-NetIPAddress -AddressFamily IPv4     # IP addresses
Get-NetAdapter | Where-Object Status -eq Up  # active adapters
Get-NetTCPConnection | Where-Object State -eq Listen  # listening ports
Test-Connection -ComputerName server01 -Count 1  # ping
Test-NetConnection -ComputerName server01 -Port 443  # TCP port test
Resolve-DnsName server01.contoso.com     # DNS lookup
```

### HTTP requests

```powershell
Invoke-WebRequest  -Uri 'https://api.example.com' -Method GET
Invoke-RestMethod  -Uri 'https://api.example.com/data' -Method POST -Body ($body | ConvertTo-Json) -ContentType 'application/json'
```

### Remoting

```powershell
Invoke-Command -ComputerName server01 -ScriptBlock { Get-Service }
Enter-PSSession -ComputerName server01
$session = New-PSSession -ComputerName server01
Invoke-Command -Session $session -ScriptBlock { hostname }
Remove-PSSession $session
```

---

## String and Data Manipulation

String operations, type conversions, JSON, and CSV - the data manipulation toolkit.

### String operations

```powershell
'hello world'.ToUpper()                  # HELLO WORLD
'hello world'.Replace('hello','hi')      # hi world
'hello world' -split ' '                 # @('hello','world')
'a','b','c' -join ', '                   # a, b, c
'hello world' -match 'w(\w+)'            # True; $Matches[1] = 'orld'
'  hello  '.Trim()                        # 'hello'
```

### Format and date

```powershell
Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
'{0:N2}' -f 1234567.89                   # 1,234,567.89
[math]::Round(3.14159, 2)                # 3.14
```

### JSON / CSV

```powershell
$obj  | ConvertTo-Json -Depth 5
$json | ConvertFrom-Json
Import-Csv   C:\data.csv
Export-Csv   C:\out.csv  -NoTypeInformation
$str | ConvertFrom-Csv -Header Name,Dept
```

### Variables and types

```powershell
$psvt = $PSVersionTable.PSVersion       # PS version
[int]'42'                                # cast string to int
[string]42                               # cast int to string
$null -eq $var                           # null check
@($result).Count                         # safe count (handles null)
```

---

## Error Handling and Logging

The constructs that separate scripts that run in production from ones that run once and never again.

### Error handling

```powershell
$ErrorActionPreference = 'Stop'          # make all errors terminating
try {
    Get-Content C:\Logs\missing.txt -ErrorAction Stop
} catch [System.IO.FileNotFoundException] {
    Write-Warning "File not found: $($_.Exception.Message)"
} catch {
    Write-Error "Unexpected error: $($_.Exception.Message)"
} finally {
    Write-Host 'Cleanup done'
}
```

### Write to output streams

```powershell
Write-Host    'Normal output'            # console only
Write-Verbose 'Diagnostic info'          # visible with -Verbose
Write-Warning 'Caution message'          # yellow warning
Write-Error   'Error message'            # red error stream
Add-Content C:\Logs\script.log "$(Get-Date) - Entry"  # log to file
```

### Null coalescing (PS 7+)

```powershell
$value = $maybeNull ?? 'default'
$obj?.Property                           # safe navigation
```

---

## Active Directory Quick Reference

The AD cmdlets for day-to-day administration.

### Users

```powershell
Get-ADUser -Identity 'jsmith' -Properties *
Get-ADUser -Filter "Department -eq 'IT'" -Properties Department, Mail
New-ADUser -Name 'Jane Doe' -SamAccountName 'jdoe' -AccountPassword (Read-Host -AsSecureString) -Enabled $true
Set-ADUser -Identity 'jsmith' -Title 'Senior Engineer'
Disable-ADAccount 'jsmith'
Enable-ADAccount  'jsmith'
Unlock-ADAccount  'jsmith'
Set-ADAccountPassword -Identity 'jsmith' -Reset -NewPassword (ConvertTo-SecureString 'NewP@ss!' -AsPlainText -Force)
```

### Groups

```powershell
Get-ADGroup -Identity 'Domain Admins' -Properties Members
Get-ADGroupMember -Identity 'IT-Admins'
Add-ADGroupMember -Identity 'IT-Admins' -Members 'jsmith'
Remove-ADGroupMember -Identity 'IT-Admins' -Members 'jsmith' -Confirm:$false
```

---

## Useful One-Liners

High-value one-liners to copy and adapt for common daily tasks.

### Find large files

```powershell
Get-ChildItem C:\ -Recurse -File -ErrorAction SilentlyContinue |
    Sort-Object Length -Descending | Select-Object -First 20 |
    Select-Object FullName, @{N='MB';E={[math]::Round($_.Length/1MB,2)}}
```

### Get local admins on a machine

```powershell
Get-LocalGroupMember -Group 'Administrators'
```

### # Find open firewall ports

```powershell
Get-NetFirewallRule -Enabled True -Direction Inbound |
    Get-NetFirewallPortFilter | Select-Object Protocol, LocalPort
```

### Export running services to CSV

```powershell
Get-Service | Where-Object Status -eq Running |
    Export-Csv C:\Reports\running_services.csv -NoTypeInformation
```

### Kill all processes matching a name

```powershell
Get-Process notepad -ErrorAction SilentlyContinue | Stop-Process -Force
```

### Get Windows version info

```powershell
$PSVersionTable.PSVersion                # PowerShell version
(Get-CimInstance Win32_OperatingSystem).Caption  # OS name
```

### Find recently modified files

```powershell
Get-ChildItem C:\Logs -Recurse -File |
    Where-Object LastWriteTime -gt (Get-Date).AddHours(-24) |
    Select-Object Name, LastWriteTime
```
