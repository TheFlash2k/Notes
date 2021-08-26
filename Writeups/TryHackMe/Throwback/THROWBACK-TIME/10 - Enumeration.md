## NMAP:
Note: connection to proxychains is a must before carrying out any enumeration
Hostname : THROWBACK-TIME

```bash
# Nmap 7.80 scan initiated Mon May 17 08:41:41 2021 as: nmap -sC -sV -p 139,3306,80,445,3389,443 -oN main.nmap 10.200.19.176
Nmap scan report for timekeep.throwback.local (10.200.19.176)
Host is up (0.42s latency).

PORT     STATE SERVICE       VERSION
80/tcp   open  http          Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.2.31)
| http-robots.txt: 1 disallowed entry 
|_/dev/
|_http-server-header: Apache/2.4.43 (Win64) OpenSSL/1.1.1g PHP/7.2.31
|_http-title: Throwback Hacks Timekeep
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp  open  ssl/http      Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.2.31)
| http-robots.txt: 1 disallowed entry 
|_/dev/
|_http-server-header: Apache/2.4.43 (Win64) OpenSSL/1.1.1g PHP/7.2.31
|_http-title: Throwback Hacks Timekeep
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp  open  microsoft-ds?
3306/tcp open  mysql?
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.4.13-MariaDB
|   Thread ID: 42
|   Capabilities flags: 63486
|   Some Capabilities: SupportsCompression, Speaks41ProtocolOld, DontAllowDatabaseTableColumn, ConnectWithDatabase, Speaks41ProtocolNew, SupportsTransactions, IgnoreSigpipes, IgnoreSpaceBeforeParenthesis, FoundRows, InteractiveClient, SupportsLoadDataLocal, ODBCClient, Support41Auth, LongColumnFlag, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: BK3[SmMt&[ym8nB>OR/1
|_  Auth Plugin Name: mysql_native_password
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THROWBACK
|   NetBIOS_Domain_Name: THROWBACK
|   NetBIOS_Computer_Name: THROWBACK-TIME
|   DNS_Domain_Name: THROWBACK.local
|   DNS_Computer_Name: THROWBACK-TIME.THROWBACK.local
|   DNS_Tree_Name: THROWBACK.local
|   Product_Version: 10.0.17763
|_  System_Time: 2021-05-17T15:45:06+00:00
| ssl-cert: Subject: commonName=THROWBACK-TIME.THROWBACK.local
| Not valid before: 2021-05-13T12:25:38
|_Not valid after:  2021-11-12T12:25:38
|_ssl-date: 2021-05-17T15:45:26+00:00; -2s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 1s, median: -1s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-17T15:44:59
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon May 17 08:45:29 2021 -- 1 IP address (1 host up) scanned in 228.07 seconds

```