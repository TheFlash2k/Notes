# What is Active Directory (AD):
Active Directory (AD) is a collection of machines and servers connected inside of domains, that are part of a bigger cluster of domains known as forests. All combined, make up the Active Directory network.

# Components of AD:
- Domain Controllers
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

### AD DS Data Store:
The Active Directory Data Store holds the databases and processes needed to store and manage directory information such as users, groups, and services.
Some of the content and characteristics of the AD DS Data Store:
- Contain the NTDS.dit file that has all the information about the AD DCs and the hashes of the passwords of the domain users
- Default storing path : %systemroot%/NTDS
- Can only be accessed by the domain controller.

## Forest:
A Forest, in terms of AD, is a collection of one or more domain trees inside an AD network. A forest is what categorizes the parts of the networks as a whole.