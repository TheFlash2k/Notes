# Table of Contents

# 1 - Attack Narrative:
## Scope:
The scope of this assessment was limited to : `10.200.19.0/24`

## Initial Enumeration:
During the initial enumeration phase, we were able to find 4 public facing machine. Following command was used to discover the machines:

```bash
$ nmap 10.200.19.0/24

Starting Nmap 7.80 ( https://nmap.org ) at 2021-05-12 04:37 PDT
Nmap scan report for 10.200.19.138
Host is up (0.17s latency).
Not shown: 996 filtered ports
PORT    STATE SERVICE
22/tcp  open  ssh
53/tcp  open  domain
80/tcp  open  http
443/tcp open  https

Nmap scan report for 10.200.19.177
Host is up (0.17s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE
22/tcp   open     ssh
1104/tcp filtered xrl
1105/tcp filtered ftranhc

Nmap scan report for 10.200.19.219
Host is up (0.17s latency).
Not shown: 993 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
5357/tcp open  wsdapi

Nmap scan report for 10.200.19.232
Host is up (0.17s latency).
Not shown: 995 closed ports
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
143/tcp   open     imap
993/tcp   open     imaps
52848/tcp filtered unknown
```

But, the diagram given to us in the TryHackMe page has 3 public facing servers:
![[AD.png]]

## Attack Path:
The attacking/enumeration path will be as follows: 
1. THROWBACK-PROD
2. THROWBACK-MAIL
3. THROWBACK-FW01

### THROWBACK-PROD
#### Initial Enumeration:
Initial Enumeration was done with nmap and `11` open ports were discovered to be open. The information revealed through these ports that was deemed to be useful:
- A Windows Box
- DNS Domain Name : `THROWBACK.local`
- Microsoft IIS 10.0

### THROWBACK-MAIL:
4 Ports open.
Guest credentials: tbhguest:WelcomeTBH1!

### THROWBACK-FW01:
> Default Credentials
> Got shell  by going to diagnostics/command prompt and injecting a simple

Bruteforcing the Mail:
- Creating a wordlist for that:
./tb-mail/username_wordlists.txt

'src/redirect.php:login_username=^USER^&secretkey=^PASS^:F=incorrect'
Password spraying:

hydra -L username_wordlist.txt -p /usr/share/wordlists/rockyou.txt 10.200.19.232 http-post-form 'src/redirect.php:login_username=^USER^&secretkey=^PASS^:F=incorrect'

## Throwback ws-01
This machine was compromised using phishing attack. This machine also is vulnerable to `Pass-the-Hash` attack.
