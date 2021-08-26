# Attack Narrative:
## Scope:
The scope of this assessment was limited to : `10.200.19.0/24`

## Discovered Hosts:
When using nmap to discover each host, we found the following 3 hosts to be public facing:

```sql
10.200.19.138 - THROWBACK-FW01
10.200.19.219 - THROWBACK-PROD
10.200.19.232 - THROWBACk-MAIL
```

## Default Credentials for THROWBACK-FW01:
The `10.200.19.138` which is the Firewall is running PFSense. Trying the default credentials of `admin:pfsense` gave us direct access to the pfsense configuration panel
![[Pasted image 20210512180419.png]]

## Reverse Shell into THROWBACK-FW01:
By enumerating the PFSense webpage, we can see that there is a diagnostics tab which has a `Command Prompt` option. Using a simple php reverse shell can give us a direct root shell without much of a hassle.
![[Pasted image 20210512182729.png]]
![[Pasted image 20210512183157.png]]

## Weak Credentials:
By bruteforcing the mail server, we were able to guess the password of users:
![[Pasted image 20210512203352.png]]
Since we were able to capture the hash of the password of `WHumphrey`. We were able to crack it using crackstation:
![[Pasted image 20210513141357.png]]

## Bait to a simple Phishing Attack:
Since we had fallen into a sort of a pit and had nothing else to attack, we sent out a simple phishing link and one of your employees actually ran it and it resulted in giving us a shell into one of your machines: `THROWBACK-WS01`

## LLMNR Poisoning:
Using `Responder` we were able to grab the hash of a user:
[SMB] NTLMv2-SSP Client   : 10.200.19.219
[SMB] NTLMv2-SSP Username : THROWBACK\PetersJ
```bash
[SMB] NTLMv2-SSP Client   : 10.200.19.219
[SMB] NTLMv2-SSP Username : THROWBACK\PetersJ
[SMB] NTLMv2-SSP Hash     : PetersJ::THROWBACK:dba25a701b4d9639:413A9A8598FA00E51FD66477F55B9678:0101000000000000C0653150DE09D201F54E04CD40162C89000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D20106000400020000000800300030000000000000000000000000200000F6F2657A2FD64BF694F5F071C191C2C9E8F25B972FEEA0E623D06E66B6ACC2EB0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00350030002E00310037002E00340038000000000000000000
```
We used hashcat to crack this hash and it took us less than 15 minutes to crack this hash using a default `rockyou.txt` wordlist. The password for this user was `Throwback317`.
![[Pasted image 20210513195949.png]]

## Administrator on PROD:
Through normal enumeration of the machine, we found credentials in the Windows Credguard and Vault and we were able to access a shell as an administrator.
<u>**_Note_:**</u> Later we were able to RDP as Admin into the machine.
![[Pasted image 20210514182928.png]]


## Shell on WS-01:
The Workstation machine 01 (IP : 10.200.19.222) was a machine on the local network and was not exposed to the public, but we were able to get a shell on the machine by pivoting from PROD onto the local network of subnet : 10.200.19.0/24.
![[Pasted image 20210517022628.png]]
Not only one, but 2 users (BlaireJ and WHumphrey) were comprised and actually fell for Pass-The-Hash attack.

## Determination of Domain Admins using Bloodhound:
Since we were able to obtain a shell onto the machine, we can simply use Sharphound on the victim and then obtain information about the internals of the domains. And we were able to do so without any problem. With some basic enumeration, we were able to find out the users that were domain admins:
![[Pasted image 20210517024950.png]]

## Findings of another domain along with THROWBACK.local:
Using a simple query ran from bloodhound, we were able to enumerate that another domain existed along with the `Throwback.local`. The name of the other domain in `CORPORATE.local`
![[Pasted image 20210517025140.png]]
![[Pasted image 20210517025231.png]]

## Getting Admin on THROWBACK-Time using an Excel-Macro
We found out that we could upload a `.xlsm` file on the web-server (`10.200.19.176`).
![[Pasted image 20210517215831.png]]
We didn't have any user access but when during our enumeration of the mail server, we found out that user `MurphyF` had a password reset email sent to them
![[Pasted image 20210517215813.png]]
Then after resetting the password, we found out that we can upload an excel file. We crafted a payload and then uploaded the malicious file and we were able to get `Admin` on the machine
![[Pasted image 20210517215945.png]]

## <u>Dumping Database Data from THROWBACK-Time:</u>
During a previous enumeration step, we did an attack what's known as kerberoasting and we were able to obtain a hash of `SQLService`. We didn't know where it would come in handy but later on we found that in `THROWBACK-TIME`, we had a MYSQL Server running, we tried running it and to our surprise, found out that we had gotten access. Through that, we were able to find the total number databases:
![[Pasted image 20210517223049.png]]
And we were able to find plaintext passwords for users of timekeep:
![[Pasted image 20210517223125.png]]
Not only that, we found out that there was a database named `domain_users` and due to the existence of that table, we were able to find out the total amount of existing domain_users:
![[Pasted image 20210517223244.png]]

## User on THROWBACK-DC01:
Thanks to the database dump on the throwback-time, we were able to setup a wordlist. Before hand, we were able to ssh into the domain controller using `BlaireJ` but didn't have much of access to anything as blaireJ wasn't allowed to do much. But, as blaireJ, we were able to dump all the users on that machine by simply going to `%USERPROFILE%` and `cd../` i.e. `C:\Users\` and `dir` to list down all the directories of the users. Due to that, we made a pretty hefty username wordlist, in terms of password, we combined the one used in our `THROWBACK-MAIL` spraying and the one dumped from the database. After spraying it using `crackmapexec`, we were able to find a working set of credentials:
`JeffersD:Throwback2020`
We were able to ssh using proxychains into the domain controller as JeffersD without any problem:
![[Pasted image 20210517225626.png]]

## Admin on THROWBACK-DC01:
Since we got user pretty easily, through some basic AD enumeration using a tool known as sharphound, we were able to gather that there was an account with AD DCSYNC rights known as backup. We knew from other enumeration that backup account would be useful to us in dumping the hashes but we need the password to actually do so and exploit that DCSYNC Right. That's when we started doing more enumeration as `JeffersD` and we found in our `Documents` directory a very peculiar file named `backup_notice.txt`. Upon that, we were able to find plaintext password for `backup`
![[Pasted image 20210519003640.png]]
Then, from there, we ran our script and dumped the hash of `MercerH`. And hence cracking the hash gave us the password:
![[Pasted image 20210519003727.png]]
Through that, we were simply able to ssh into our domain controller.
![[Pasted image 20210519003803.png]]

## Crossing the Trust:
From our previous enumeration, we knew that there was another trust known as `CORPORATE.local` . So, we sent out a ping sweep from our THROWBACK/DC01, we got a hit on `10.200.19.118`. Now, in order to access these machine, we would have to kill our old proxy server that was running on `PROD` and switch it over to DC01. That would be pretty easy. Just transferring the binary onto DC01 whilst running as MercerH and simply running it and we'd have a meterpreter and from then onward it's pretty fun. That allowed us to actually create a proxy originating from the Domain Controller on THROWBACK-01 and then from there we were able to use `evil-winrm` to cross over to the other domain controller `CORP-DC01`

## Accessing CORP-DC01 as Admin:
From our enumeration done on THROWBACK-DC01, we were able to enumerate that our user, who was admin of this trust and domain was also admin on the other domain, `CORPORATE.local`. Through the use of that, we used our pre-setup proxychains server. SSH and RDP were disabled on this Domain Controller. But we got a shell using evil-winrm.

## Database Password Using Github:
We know that each commit in github is always saved. Through some passive recon, we found a few social media links of Throwback Hacks Security, primariliy Twitter and Linkedin. The most important that was found was github. We saw, user `Rikka Foxx` had a Github profile https://github.com/RikkaFoxx/. When going there, there was a simple Throwback-Time repository that had some various php files. Looking into the commits, we saw that `db_connect.php` was updated. When comparing the changes, we were able to find plain text password to the database

![[Pasted image 20210519183826.png]]

## Pivot to CORP-ADT01 and Getting Administrator Privlieges:
Through our basic enumeration on AD-DC01, we found out another system in the domain.
![[Pasted image 20210519190559.png]]
By enumerating further, we can find the ip of that system is `10.200.19.243`. We try to access that, but we were unable to as we couldn't directly access it from Throwback-DC01 hence we had to set our proxychains to CORP-DC01.
![[Pasted image 20210520131338.png]]
Since we gained some credentials from Github, we can try and access this server using that:
![[Pasted image 20210520131531.png]]
We were successfully able to SSH into ADT01 as `DaviesJ`
Now, for the privesc. After getting a shell onto the box, we went for Token delegation and for that we need a meterpreter session. We simply uploaded our reverse shell, disabled windows defender and got a meterpreter session. Using meterpreter, we loaded a built-in module called incognito and simply dumped all the available tokens and impersonated as NT-Authority (ADMIN) and we got a root shell.
![[Pasted image 20210520134233.png]]

## Accessing TBSEC-DC01:
We knew that a data-breach had occurred a while back. We used `breachgtfo.local` with tons of possible emails and wrote a little script to help us in narrowing down each result.
![[Pasted image 20210520141714.png]]
By accessing the mail, we can see that a particular with some credentials has been provided. Assuming these credentials are that of TBSEC-DC01:
![[Pasted image 20210520143647.png]]
From here, further enumeration led us to the finding that TBSEC-DC01 has the IP of `10.200.19.79`. Using the credentials, we can simply RDP into the Domain Controller.
![[Pasted image 20210520145728.png]]
After further enumeration, we tried to run a very popular tool called rubeus to try and get a kerberoasting. And that worked and we got a KGT ticket, we cracked that using hashcat and got a password.
![[Pasted image 20210520152242.png]]
Hashcat output:
Command used:
```bash
$ hashcat -m 13100 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```
![[Pasted image 20210520152303.png]]
`Account  : TBService`
`Password : securityadmin284650`
![[Pasted image 20210520152021.png]]

---
