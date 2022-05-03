# North Coast AD Restructure
#northcoast #activedirectory 

Current Structure

```
\* AD Tree
├─ Carlsbad
├─ Disabled Accounts
├─ Distribution Groups
├─ Domain Controllers
├─ Executive Staff
├─ Fallbrook
|  ├─ Laptop
|  ├─ MAC
|  ├─ No Shutdown Computers
|  ├─ PC
|  ├─ Server OU
|  └─ Volunteer
├─ IT Staff
├─ Member Servers
|  └── Terminal Servers
├─ North Coast Users and Computers
|  ├─ Admins
|  ├─ Desktop
|  ├─ Email Distribution Groups - Bypass Recipients Inbound Rule
|  ├─ Remote
|  ├─ Shared Accounts
|  ├─ Special
|  ├─ Training Email Accounts
|  └─ Video Venues
├─ Ramona
|  ├─ MAC
|  ├─ PC
|  └─ Users
├─ Rancho Bernardo
|  ├─ PC
|  ├─ Server OU
|  └─ Users
├─ Security Groups
├─ Service Accounts
├─ Shared Mailboxes
├─ SME
|  ├─ Laptop
|  ├─ MAC
|  ├─ PC
|  ├─ Server OU
|  └─ Users
├─ SurfControl Lookup
└─ Vista
   ├─ Laptop
   ├─ MAC
   ├─ No Shutdown Computers
   ├─ PC
   ├─ Server OU
   ├─ Users
   └─ Volunteer
   └── IT Managed Accounts
```

## Questions about our current structure

1. What objects should go under North Coast Users and computers? Currently each campus has its own OU some of which have either computers or users in them but the majority of our users and computers seem to be in North Coast Users and Computers.
2. Server OU’s, Currently we have a Member Servers OU and also some campuses have their own server OUs, the majority of our servers are in the Member Servers OU, where should servers go?
3. Items to delete:
	1. OU Cafe / Cafe Computers & Cafe Users : Only one user named cafe exists in this OU tree, no group policies are applied.
	2. OU Executive Staff : no objects exist in this OU.
	3. Member Server Auto Update : 1 object Searchable Sermons is in this OU and has an automatic update policy applied, I think we should move it to member servers and delete this OU.
	4. North Coast Users and Computer / Email Distribution Groups - Bypass Recipients Inbound Rule, Empty OU with no group policys
	5. North Coast Users and Computer / Special / Uber Secure contains one disabled account, no group policies special to this OU.
	6. North Coast Users and Computer / Training Email Accounts is an empty OU
	7. Security Groups / Read Only Groups is empty
4. We have other additional Empty OU’s scattered under our Campus OU’s, I would like to clean those up.

New Structure:

```
\* North Coast (OU)
├─ Accounts
| ├── Staff
| | └── Departments
| ├── Volunteers
| ├── Shared Accounts
| ├── Disabled Accounts
| └── IT Managed Accounts
├─ Groups
| ├── Security Groups
| └── Distribution Groups
├─ Computers
| ├── Campus Workstations
| ├── Departments
| └── Special Purpose Computers OUs 
└─ Servers
 ├── Terminal
 ├── Print
 └── Hyper-v
```

## Why Change?

1. [https://docs.microsoft.com/en-us/previous-versions/technet-magazine/(v=msdn.10](https://docs.microsoft.com/en-us/previous-versions/technet-magazine/cc462797(v=msdn.10))
> Ensure that you don't unnecessarily create or link multiple GPOs. With a GPO, you can create one policy and apply it to multiple OUs. This is sometimes helpful, but it can also be harmful. If you have to change a GPO setting and you have an overly complex system of linked GPOs, you could accidentally apply a change to the wrong objects. The more links you create, the more difficult it is to grasp the scope of a policy. Likewise, you should avoid creating additional policies with the same settings as other policies. If you find that you often do this, consider modifying your OU structure to apply a new GPO inheritance model.
> Think about your OU structure in terms of GPOs, the goal should be to eliminate complexity. Make sure that the OU adds to the flow of GPO inheritance. If your OU contains servers that require the same policy as other servers, consider putting these computer objects under a broader Servers OU and creating multiple OUs for different server types below the Servers OU.
2. More flexible controls for applying computer policy, dynamic and WMI filtered policies can be used without creating many individual links.
3. No more need for inheritance blocking.
4. Adoption of Baseline Policies and reduction in quantity and complexity of Policies. This also helps security in that devices can have policies deployed based on what they are not necessarily where they are.
5. Intuitive object placement, by dividing objects based on type, then location it becomes obvious where objects should be put.
6. Eliminates empty staging OUs.

Why put everything under one OU instead of using the default active directory tree?

* Permission delegation, with the proposed structure we can grant domain admin like power without true domain admin privileges. It also allows for easy delegation of more Helpdesk level accounts that might be suitable for IT interns.