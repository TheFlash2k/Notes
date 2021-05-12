# What is Active Directory (AD):
Active Directory (AD) is a collection of machines and servers connected inside of domains, that are part of a bigger cluster of domains known as forests. All combined, make up the Active Directory network.
 
 # Why is Active Directory used:
 Consider the example of our university, we go to any computer in our university and we can easily sign in using our university ids, how so? Well, our university uses Active Directory to control and monitor the student's computers' information through a single (or maybe multiple who knows) domain controllers. Active Directory allows a single user to sign in to any computer on the active directory network and have access to his or her stored files and folders in the server, as well as the local storage on that machine. This allows for any student in the university to use any machine within the premise of university, without having to set up multiple users on a machine. Active Directory does it all for you.
# Components of AD:
- Domain Controllers
- AD DS Data Store
- Forests, Trees, Domains
- Users + Groups
- Trusts
- Policies
- Domain Services

## Domain Controllers:
A Domain Controller is a Windows Server with the Active Directory Domain Services (AD DS) installed and has been promoted to a domain controller in the forest. DCs are the center of an AD forest. They control each object in a domain. The tasks of a DC are:
- holds the AD DS Data Store
- handling of authentication and authorization services
- replicating updates from other domain controllers in the forest.
- Allowing admin access to manage domain resources.

## AD DS Data Store:
The Active Directory Data Store holds the databases and processes needed to store and manage directory information such as users, groups, and services.
Some of the content and characteristics of the AD DS Data Store:
- Contain the NTDS.dit file that has all the information about the AD DCs and the hashes of the passwords of the domain users
- Default storing path : %systemroot%/NTDS
- Can only be accessed by the domain controller.

## Forest:
A Forest, in terms of AD, is a collection of one or more domain trees inside an AD network. A forest is what categorizes the parts of the networks as a whole.
![[AD.forest.png]]
### Parts of a Forest:
An AD forest consists of the following parts:
- Domains
> Used to group and manage objects
- Trees
> A hierarchy of domains the AD DS
- Organizational Units (OU)
> Containers for groups, computers, user, printers and other OUs
- Trusts
> Allows users to access resources in other domains
- Objects
> Users, groups, printers, computers, shares
- Domain Services
> DNS Server, LLMNR, IPv6
- Domain Schema
> Rules for object creation

## Users:
The main purpose of AD was to make it easier for users to have access to different object. In an AD environment, users can be of many types based on the configuration established by the admin but, primarily there are four types of users:
- Domain Admins
> They control the domains and have access to the DC
- Services Accounts
> (Can also be domain admins). These accounts are not used for anything except service maintenence. They are required by Windows services
- Local Administrators
> Can make changes to the local machines and might have access to other users of the same or lower level but don't have access to DC
- Domain Users
> The everyday users. They can login into the machine whose access they possess and might also be a local adminstrator.

## Groups:
Combining multiple users into a container; sort of, is called a group. The reasons groups are very usefil is that it makes it easier to give permissions to all objects belonging to a certain group. In AD, there are two groups:
- Security Groups
> These groups are used to specify permissions for a large number of users
- Distribution Groups
> These groups are used to specify email distribution lists. Since we're the ones attacking an AD environment, these aren't that much beneficial to us but may come in handy during enumeration phase.

### Default Security Groups:
-  Domain Controllers
> All domain controllers in the domain
-  Domain Guests
> All domain guests
-  Domain Users
> All domain users
-  Domain Computers
> All workstations and servers joined to the domain
-  Domain Admins
> Designated administrators of the domain
-  Enterprise Admins
> Designated administrators of the enterprise
-  Schema Admins
> Designated administrators of the schema
-  DNS Admins
> DNS Administrators Group
-  DNS Update Proxy
> DNS clients who are permitted to perform dynamic updates on behalf of some other clients (such as DHCP servers).
-  Allowed RODC Password Replication Group
> Members in this group can have heir passwords replicated to all read-only domain controllers in the domain
- Group Policy Creator Owners
> Members in this group can modify group policy for the domain
-  Denied RODC Password Replication Group
> Members in this group cannot have their passwords replicated to any read-only domain controllers in the domain
-  Protected Users
> Members of this group are afforded additional protections against authentication security threats.
- Cert Publishers
> Members of this group are permitted to publish certificates to the directory
- Read-Only Domain Controllers
> Members of this group are Read-Only Domain Controllers in the domain
- Enterprise Read-Only Domain Controllers
> Members of this group are Read-Only Domain Controllers in the enterprise
- Key Admins
> Members of this group can perform administrative actions on key objects within the domain.
- Enterprise Key Admins
> Members of this group can perform administrative actions on key objects within the forest.
- Cloneable Domain Controllers
> Members of this group that are domain controllers may be cloned.
- RAS and IAS Servers
> Servers in this group can access remote access properties of users

## Domain Trusts:
Trusts are a mechanism that are in place for users in the network to gain access to other resources within the domain. Trusts outline the way that the domains inside of a forest communicate to each other.  Depending on the AD environment, trusts can be extented out to external domains and sometimes other forests. The type of trusts put in place determines how the domains and trees in a forest are able to communicate and send data to and from each other when attacking an Active Directory environment you can sometimes abuse these trusts in order to move laterally throughout the network. There are two types of trusts:
- Directional
> The direction of a trust flows from a trusting domain to a trusted domain.
- Transitive
> The trust relationship expands beyond just two domains to include other trusted domains.

![[AD.trusts.png]]
## Domain Policies:
Policies determine how a DC will operate and what set of rules will it follow or not follow. You can think of domain policies like domain groups, except instead of permissions they contain rules, and instead of only applying to a group of users, the policies apply to a domain as a whole. They simply act as a rulebook for Active  Directory that a domain admin can modify and alter as they deem necessary to keep the network running smoothly and securely. Alongside these, custom policies can also be setup by the domain admin. 

## Domain Services:
Domain services are the services provided by the Domain Controller to the entire Domain. A DC can provide a lot of Services but the default ones are:
- LDAP - Lightweight Directory Access Protocol
> This allows communication between applications and directory services
- Certificate Services
> Allows the domain controller to create, validate and revoke public key certificates
- DNS, LLMNR, NBT-NS
> Domain name Services for identifying IP hostnames
## Domain Authentication:
This is the most important and the most vulnerable part of any AD Environment. This is the authentication protocols set in place. There are two types of authentication protocols setup in place for AD:
- Kerberos
> The default authentication service for AD. This uses ticket-granting tickets (TGT) and service tickets to authenticate users and give users access to other resources across the domain.
- NTLM
> The default authentication protocol. This uses and encrypted challenge/reponse protocol.

## References:
https://tryhackme.com/room/activedirectorybasics
https://tryhackme.com/room/throwback