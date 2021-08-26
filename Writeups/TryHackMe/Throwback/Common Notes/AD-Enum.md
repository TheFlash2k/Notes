## Get-ADDomain
Get-ADDomain is a commandlet that pulls a large majority of the information about the Domain you’re attacking. It can list all of the Domain Controllers for a given environment, tell you the NetBIOS Domain name, the FQDN (Fully Qualified Domain name) and much more. Using the Select-Object command, we can filter out some of the unnecessary objects that may be displayed (like COntainers, Group Policy Objects, and much more)

`Get-ADDomain | Select-Object NetBIOSName, DNSRoot, InfrastructureMaster`

## Get-ADForest
Get-ADForest is another commandlet that pulls all the Domains within a Forest and lists them out to the user. This may be useful if a bidirectional trust is setup, it may allow you to gain a foothold in another domain on the LAN. Just like Get-ADDomain, there is a lot of output, so we will be using Select-Object to trim the output down.
`Get-ADForest | Select-Object Domain`

## Get-ADTrust 
Get-ADTrust is the last built in Powershell commandlet that we will be discussing, after this, we will move over to Powerview. Get-ADTrust provides a ton of information about the Trusts within the AD Domain. It can tell you if it’s a one way or bidirectional trust, who the source is, who the target is, and much more. One required field is -Filter, this is required in the event that you want to filter on a specific Domain/Trust, if you do not (like in most circumstances), you can simply provide a \* to wildcard the results.
`Get-ADTrust -Filter * | Select-Object Direction,Source,Target`

## Introduction to PowerView
Powerview (part of [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) by PowerShellMafia) is an excellent suite of tools that can be used for enumeration, and exploitation of an AD Domain, today we’re only going to cover Powerview’s ability to enumerate information about the domain and their associated trusts, you can get the .ps1 [here](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1).

## Get-NetDomain
Get-NetDomain is similar to the ActiveDirectory module’s Get-ADDomain but contains a lot less information, which can be better. Basic info such as the Forest, Domain Controllers, and Domain Name are enumerated.
`Get-NetDomain`
## Get-NetDomainController
Get-NetDomainController is another useful cmdlet that will list all of the Domain Controllers within the network. This is incredibly useful for initial reconnaissance, especially if you do not have a Windows device that’s joined to the domain.
`Get-NetDomainController`    

## Get-NetForest
Get-NetForest is similar to Get-ADForest, and provides similar output. It provides all the associated Domains, the root domain, as well as the Domain Controllers for the root domain.
`Get-NetForest`    
## Get-NetDomainTrust
Get-NetDomainTrust is similar to Get-ADTrust with our SelectObject filter applied to it. It’s short, sweet and to the point!
`Get-NetDomainTrust`