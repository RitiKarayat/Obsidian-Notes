# Introduction
## What is Active Directory? - 

Active Directory is a collection of machines and servers connected inside of domains, that are a collective part of a bigger forest of domains, that make up the Active Directory network. Active Directory contains many functioning bits and pieces, a majority of which we will be covering in the upcoming tasks. To outline what we'll be covering take a look over this list of Active Directory components and become familiar with the various pieces of Active Directory: 

-   Domain Controllers
-   Forests, Trees, Domains
-   Users + Groups 
-   Trusts
-   Policies 
-   Domain Services

## Why use Active Directory? -

The majority of large companies use Active Directory because it allows for the control and monitoring of their user's computers through a single domain controller. It allows a single user to sign in to any computer on the active directory network and have access to his or her stored files and folders in the server, as well as the local storage on that machine. This allows for any user in the company to use any machine that the company owns, without having to set up multiple users on a machine. Active Directory does it all for you.



# Physical Active Directory
## Domain Controllers -

﻿A domain controller is a Windows server that has Active Directory Domain Services (AD DS) installed and has been promoted to a domain controller in the forest. Domain controllers are the center of Active Directory -- they control the rest of the domain. I will outline the tasks of a domain controller below: 

-   holds the AD DS data store 
-   handles authentication and authorization services 
-   replicate updates from other domain controllers in the forest
-   Allows admin access to manage domain resources

## AD DS Data Store - 

The Active Directory Data Store holds the databases and processes needed to store and manage directory information such as users, groups, and services. Below is an outline of some of the contents and characteristics of the AD DS Data Store:

-   Contains the NTDS.dit - a database that contains all of the information of an Active Directory domain controller as well as password hashes for domain users
-   Stored by default in %SystemRoot%\NTDS
-   accessible only by the domain controller

# Forest
A forest is a collection of one or more domain trees inside of an Active Directory network. It is what categorizes the parts of the network as a whole.

The Forest consists of these parts which we will go into farther detail with later:

-   Trees - A hierarchy of domains in Active Directory Domain Services
-   Domains - Used to group and manage objects 
-   Organizational Units (OUs) - Containers for groups, computers, users, printers and other OUs
-   Trusts - Allows users to access resources in other domains
-   Objects - users, groups, printers, computers, shares
-   Domain Services - DNS Server, LLMNR, IPv6
-   Domain Schema - Rules for object creation

# Users + Groups
## Users Overview
The four types of users are: 

-   Domain Admins - This is the big boss: they control the domains and are the only ones with access to the domain controller.
-   Service Accounts (Can be Domain Admins) - These are for the most part never used except for service maintenance, they are required by Windows for services such as SQL to pair a service with a service account
-   Local Administrators - These users can make changes to local machines as an administrator and may even be able to control other normal users, but they cannot access the domain controller
-   Domain Users - These are your everyday users. They can log in on the machines they have the authorization to access and may have local administrator rights to machines depending on the organization.

## Groups Overview
﻿Groups make it easier to give permissions to users and objects by organizing them into groups with specified permissions. There are two overarching types of Active Directory groups: 

-   Security Groups - These groups are used to specify permissions for a large number of users
-   Distribution Groups - These groups are used to specify email distribution lists. As an attacker these groups are less beneficial to us but can still be beneficial in enumeration

# Trusts +Policies

## Domain Trusts Overview -

﻿Trusts are a mechanism in place for users in the network to gain access to other resources in the domain. For the most part, trusts outline the way that the domains inside of a forest communicate to each other, in some environments trusts can be extended out to external domains and even forests in some cases.

![](https://i.imgur.com/4uGI3bF.png)  

There are two types of trusts that determine how the domains communicate. I'll outline the two types of trusts below: 

-   Directional - The direction of the trust flows from a trusting domain to a trusted domain
-   Transitive - The trust relationship expands beyond just two domains to include other trusted domains

## Domain Policies Overview - 

Policies are a very big part of Active Directory, they dictate how the server operates and what rules it will and will not follow. You can think of domain policies like domain groups, except instead of permissions they contain rules, and instead of only applying to a group of users, the policies apply to a domain as a whole. They simply act as a rulebook for Active  Directory that a domain admin can modify and alter as they deem necessary to keep the network running smoothly and securely.

-   Disable Windows Defender - Disables windows defender across all machine on the domain
-   Digitally Sign Communication (Always) - Can disable or enable SMB signing on the domain controller





# Active Directory Domain Services +Authentication
## Domain Services Overview - 

Domain Services are exactly what they sound like. They are services that the domain controller provides to the rest of the domain or tree. Outlined below are the default domain services: 

-   LDAP - Lightweight Directory Access Protocol; provides communication between applications and directory services
-   Certificate Services - allows the domain controller to create, validate, and revoke public key certificates
-   DNS, LLMNR, NBT-NS - Domain Name Services for identifying IP hostnames

## Domain Authentication Overview - 

The most important part of Active Directory -- as well as the most vulnerable part of Active Directory -- is the authentication protocols set in place. There are two main types of authentication in place for Active Directory: NTLM and Kerberos. Since these will be covered in more depth in later rooms we will not be covering past the very basics needed to understand how they apply to Active Directory as a whole. For more information on NTLM and Kerberos check out the Attacking Kerberos room - [https://tryhackme.com/room/attackingkerberos](https://tryhackme.com/room/attackingkerberos).

-   Kerberos - The default authentication service for Active Directory uses ticket-granting tickets and service tickets to authenticate users and give users access to other resources across the domain.
-   NTLM - default Windows authentication protocol uses an encrypted challenge/response protocol

# Lab
ssh in the machine and set up powerview

![[Pasted image 20220418055517.png]]

![[Pasted image 20220418055556.png]]

![[Pasted image 20220418055621.png]]

![[Pasted image 20220418055648.png]]

