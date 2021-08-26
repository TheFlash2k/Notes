## Nmap:
NOTE: 22 is also open because i somehow ssh'd into it xdddd but doesn't show up on the nmap scan
```bash
# Nmap 7.80 scan initiated Mon May 17 08:52:38 2021 as: nmap -sC -sV -p 139,3306,80,445,3389,443 -oN main.nmap 10.200.19.117
Nmap scan report for 10.200.19.117
Host is up (0.53s latency).

PORT     STATE  SERVICE       VERSION
80/tcp   open   http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
139/tcp  open   netbios-ssn   Microsoft Windows netbios-ssn
443/tcp  closed https
445/tcp  open   microsoft-ds?
3306/tcp closed mysql
3389/tcp open   ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THROWBACK
|   NetBIOS_Domain_Name: THROWBACK
|   NetBIOS_Computer_Name: THROWBACK-DC01
|   DNS_Domain_Name: THROWBACK.local
|   DNS_Computer_Name: THROWBACK-DC01.THROWBACK.local
|   DNS_Tree_Name: THROWBACK.local
|   Product_Version: 10.0.17763
|_  System_Time: 2021-05-17T15:53:37+00:00
| ssl-cert: Subject: commonName=THROWBACK-DC01.THROWBACK.local
| Not valid before: 2021-05-13T12:25:23
|_Not valid after:  2021-11-12T12:25:23
|_ssl-date: 2021-05-17T15:53:53+00:00; -1s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2021-05-17T15:53:42
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon May 17 08:53:54 2021 -- 1 IP address (1 host up) scanned in 76.02 seconds

```


## DCSYNC Rights:

User `mercerh` has the DCSYNC right and is also an administrator.
