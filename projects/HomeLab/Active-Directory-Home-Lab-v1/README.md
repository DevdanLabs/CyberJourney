# Active Directory Home Lab
### Building a Windows Enterprise Environment with Active Directory Domain Services

> A hands-on project demonstrating the deployment of a small enterprise Active Directory environment using Windows Server 2022, Windows 10 Pro, and VMware Workstation.

---

# Project Overview

This project documents the complete process of designing and deploying a small Active Directory environment from scratch inside VMware Workstation. The objective was not only to install Windows Server, but to understand how enterprise Windows infrastructures are built and managed in real-world organizations.

The lab consists of a dedicated Domain Controller (DC01) running Windows Server 2022 and a Windows 10 Pro workstation (WS01) joined to the newly created Active Directory domain. Throughout the project, core Windows Server administration tasks were performed, including Active Directory Domain Services (AD DS) deployment, DNS configuration, Organizational Unit (OU) design, Security Group management, domain user administration, and workstation domain integration.

Rather than relying on preconfigured virtual machines, every component of the environment was built manually to gain practical experience with enterprise deployment workflows.

---

# Project Objectives

- Build a fully functional Active Directory domain from scratch.
- Deploy Windows Server 2022 as a Domain Controller.
- Configure Active Directory Domain Services (AD DS).
- Create an enterprise-style Organizational Unit (OU) hierarchy.
- Manage domain users and security groups.
- Deploy a Windows 10 Pro client workstation.
- Join the workstation to the Active Directory domain.
- Validate domain authentication, DNS resolution, and network connectivity.
- Create a reusable home lab for future Windows administration and cybersecurity projects.

---

# Skills Demonstrated

- Windows Server Administration
- Active Directory Administration
- DNS Configuration
- Enterprise Network Design
- VMware Virtualization
- Windows Client Deployment
- User and Group Management
- Organizational Unit Design
- Domain Authentication
- Network Troubleshooting

---

# Technology Stack

| Component | Technology |
|-----------|------------|
| Hypervisor | VMware Workstation Pro |
| Server OS | Windows Server 2022 Standard Evaluation |
| Client OS | Windows 10 Pro (22H2) |
| Directory Service | Active Directory Domain Services (AD DS) |
| DNS | Microsoft DNS Server |
| Authentication | Kerberos |
| Directory Protocol | LDAP |
| Network | Host-Only (VMnet1) |
| Domain | `cyberjourney.lab` |

---

# Lab Architecture

```text
                    VMware Workstation

                    VMnet1 (Host-Only)
                  192.168.150.0/24
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
+----------------------+       +----------------------+
|         DC01         |       |         WS01         |
|----------------------|       |----------------------|
| Windows Server 2022  |       | Windows 10 Pro       |
| Active Directory     |       | Domain Member        |
| DNS Server           |       | Enterprise Client    |
| Domain Controller    |       |                      |
|                      |       |                      |
| 192.168.150.10       |       | 192.168.150.20       |
+----------------------+       +----------------------+

            Active Directory Domain
               cyberjourney.lab
```

---

# Network Design

Before deploying any virtual machines, a dedicated **Host-Only** virtual network was created using VMware Workstation's Virtual Network Editor.

Using a Host-Only network provides an isolated environment where all virtual machines can communicate with each other without exposing the lab to the physical network or the Internet. This approach closely resembles how isolated testing environments are commonly built for enterprise administration, penetration testing, and cybersecurity research.

The chosen subnet for this lab was:

| Setting | Value |
|---------|-------|
| Network Type | Host-Only |
| VMware Network | VMnet1 |
| Network Address | 192.168.150.0/24 |
| Domain Controller | 192.168.150.10 |
| Windows Client | 192.168.150.20 |

> **Screenshot 1 — VMware VMnet1 Configuration**
>
> `image/01-vmware-network-editor.png`

---

# Virtual Machine Deployment

With the virtual network configured, the next step was creating the virtual machines that would represent the enterprise environment.

The first virtual machine was configured as the Domain Controller (DC01), which would later host Active Directory Domain Services and DNS. Standardized virtual hardware was assigned to provide a lightweight yet functional enterprise lab suitable for learning Windows Server administration.

The Windows 10 workstation (WS01) would be deployed later and connected to the same Host-Only network, allowing secure communication with the Domain Controller throughout the project.

> **Screenshot 2 — Windows Server Virtual Machine Configuration**
>
> `image/02-server-vm-creation.png`

---

**Next:** Part 2 – Deploying Windows Server 2022 and Preparing the Domain Controller

# Part 2 - Deploying Windows Server 2022

With the virtual environment prepared, the next phase focused on deploying the first server that would become the foundation of the entire enterprise infrastructure.

A Windows Server 2022 virtual machine was installed and configured to serve as the future Domain Controller. Before Active Directory could be deployed, the operating system needed to be installed, configured, and prepared for server roles.

---

# Installing Windows Server 2022

Windows Server 2022 Standard Evaluation was installed using Microsoft's official ISO image.

During the installation process, the default language and regional settings were selected before proceeding with the operating system deployment.

> **Screenshot 3 — Windows Server Setup**
>
> `image/03-server-installation-language.png`

---

# Selecting the Windows Edition

The **Windows Server 2022 Standard Evaluation (Desktop Experience)** edition was selected.

The Desktop Experience edition includes the traditional graphical user interface (GUI), making it ideal for learning Windows Server administration and Active Directory concepts.

Although many production servers are administered remotely or through PowerShell, the graphical interface provides an excellent learning environment for understanding Microsoft's management tools.

> **Screenshot 4 — Windows Server Edition Selection**
>
> `image/04-server-installation-edition.png`

---

# Preparing the Virtual Disk

A new virtual disk was allocated to the server during installation.

The operating system was installed on a dedicated virtual disk, providing sufficient storage for Windows Server, Active Directory, DNS, and future enterprise services that will be added in later projects.

> **Screenshot 5 — Disk Selection**
>
> `image/05-server-installation-disk.png`

---

# Completing the Installation

After the installation completed, Windows Server booted into the desktop environment for the first time.

At this stage, the operating system had not yet been configured as a Domain Controller. It was simply functioning as a standalone Windows Server waiting for additional configuration.

An Administrator password was configured during setup, allowing access to the local server for further administration.

---

# Initial Server Configuration

After logging in, **Server Manager** automatically launched.

Server Manager is the central management console used to administer Windows Server. From this interface administrators can:

- Install server roles and features
- Configure networking
- Manage storage
- Monitor server health
- Deploy Active Directory
- Install DNS
- Manage local and remote servers

Most Windows Server administration begins from this console.

> **Screenshot 6 — Server Manager**
>
> `image/06-server-manager.png`

---

# Why Windows Server?

Unlike Windows 10 or Windows 11, Windows Server is specifically designed to provide centralized services for enterprise environments.

Instead of acting as an individual workstation, Windows Server can host services that support hundreds or even thousands of computers across an organization.

Some common server roles include:

- Active Directory Domain Services (AD DS)
- DNS Server
- DHCP Server
- File Server
- Print Server
- Certificate Services
- Hyper-V
- Windows Deployment Services

For this project, the server would primarily function as both the **Domain Controller** and **DNS Server** for the entire Active Directory environment.

---

# Preparing for Active Directory

Before Active Directory could be installed, the server environment was reviewed to ensure it was ready for deployment.

At this stage:

- Windows Server installation was completed.
- Local Administrator access was verified.
- Server Manager was available.
- The virtual network had already been configured using VMnet1.
- The server was ready to receive the Active Directory Domain Services role.

The next phase of the project focuses on transforming this standalone Windows Server into the first Domain Controller of the **cyberjourney.lab** domain.

---

# Key Takeaways

- Windows Server 2022 provides the foundation for enterprise infrastructure.
- The Desktop Experience edition simplifies learning and administration through a graphical interface.
- Server Manager acts as the primary management console for Windows Server.
- At this stage, the server is still a standalone machine and has not yet been promoted to a Domain Controller.
- Installing the operating system is only the first step toward building an Active Directory environment.

---

**Next:** Part 3 – Installing Active Directory Domain Services (AD DS) and Promoting the Server to a Domain Controller

# Part 3 - Deploying Active Directory Domain Services (AD DS)

With Windows Server successfully installed, the next objective was to transform the standalone server into the first Domain Controller within the enterprise environment.

This process involved installing **Active Directory Domain Services (AD DS)**, promoting the server to a Domain Controller, creating a new Active Directory forest, and configuring the integrated DNS service.

These steps establish the foundation for centralized authentication, authorization, and directory management.

---

# What is Active Directory Domain Services?

Active Directory Domain Services (AD DS) is Microsoft's directory service used to centrally manage users, computers, groups, and security policies within an organization.

Instead of storing user accounts locally on each workstation, AD DS maintains a centralized database that allows all domain-joined devices to authenticate against a single trusted authority.

This enables organizations to:

- Centralize user management
- Simplify authentication
- Apply Group Policies
- Manage enterprise resources
- Enforce security policies
- Delegate administrative responsibilities

Without AD DS, every workstation would manage its own local users independently, making administration increasingly difficult as the environment grows.

---

# Installing Active Directory Domain Services

Using **Server Manager**, the **Add Roles and Features Wizard** was launched to install the **Active Directory Domain Services** role.

During installation, Windows automatically selected the required management tools, including:

- Active Directory Users and Computers
- Active Directory Administrative Center
- Active Directory Module for Windows PowerShell
- Group Policy Management

These tools provide administrators with everything required to manage an Active Directory environment.

> **Screenshot 7 — Installing Active Directory Domain Services**
>
> `image/07-add-ad-ds-installation.png`

---

# Promoting the Server to a Domain Controller

Installing the AD DS role alone does not create a Domain Controller.

After the installation completed, the server was promoted using the **Active Directory Domain Services Configuration Wizard**.

A new forest was created with the following root domain:

```text
cyberjourney.lab
```

Since this was the first Domain Controller in the environment, there were no existing forests or domains to join.

> **Screenshot 8 — Domain Controller Promotion**
>
> `image/08-domain-controller-promotion.png`

---

# Creating the First Forest

Active Directory is organized into logical structures.

For this project, a completely new forest was created.

```text
Forest
│
└── Domain
    │
    └── cyberjourney.lab
```

The forest represents the highest security and administrative boundary within Active Directory.

Although this lab currently contains only a single domain, enterprise environments often contain multiple domains within a single forest.

---

# Active Directory and DNS

One of the most important aspects of Active Directory deployment is DNS integration.

During the promotion process, Windows automatically configured the Microsoft DNS Server role.

DNS is responsible for allowing domain-joined computers to locate services such as:

- Domain Controllers
- Kerberos Key Distribution Center (KDC)
- LDAP Services
- Global Catalog

Without DNS, Windows clients cannot locate the Domain Controller, making domain authentication impossible.

This dependency is one of the most common causes of Active Directory deployment and domain join failures.

---

# Prerequisite Check

Before completing the promotion process, Windows automatically performed a comprehensive prerequisite validation.

The validation confirmed that:

- Required server roles were installed.
- Forest configuration was valid.
- DNS configuration was acceptable.
- No blocking errors were detected.
- The server was ready to become a Domain Controller.

The successful completion of this validation ensured that the promotion process could proceed safely.

> **Screenshot 9 — Active Directory Prerequisite Check**
>
> `image/09-prerequisites-check.png`

---

# Completing the Promotion

After the prerequisite checks passed, Windows completed the promotion process and automatically restarted the server.

Following the reboot, the server assumed several critical enterprise roles simultaneously:

- Domain Controller
- Active Directory Server
- DNS Server
- Kerberos Key Distribution Center (KDC)

At this point, the standalone Windows Server had been fully transformed into the first Domain Controller for the **cyberjourney.lab** domain.

---

# Why This Step Is Important

Promoting a server to a Domain Controller fundamentally changes its role within the network.

Instead of simply operating as a standalone computer, the server now becomes responsible for:

- Authenticating users
- Authenticating computers
- Managing directory objects
- Storing the Active Directory database
- Processing authentication requests
- Providing DNS services
- Enforcing centralized identity management

Every future workstation, server, and user account added to the environment will rely on this Domain Controller for authentication and directory services.

---

# Key Takeaways

- Installing AD DS does not automatically create a Domain Controller.
- Promoting the server creates the first Active Directory forest and domain.
- DNS is tightly integrated with Active Directory and is essential for domain communication.
- Windows performs prerequisite validation before promotion to ensure a successful deployment.
- After promotion, the server becomes the central authentication and directory service provider for the entire enterprise environment.

---

**Next:** Part 4 – Designing the Active Directory Environment (Organizational Units, Users, and Security Groups)

# Part 4 - Designing the Active Directory Environment

With the Domain Controller successfully deployed, the next phase focused on organizing the Active Directory environment.

A well-designed directory structure is essential for efficient administration, scalability, and security. Rather than storing every object inside the default Active Directory containers, a custom Organizational Unit (OU) hierarchy was created to simulate how users and departments are typically organized in a real enterprise.

This design also prepares the environment for future projects involving Group Policy, delegated administration, and access control.

---

# Designing the Organizational Unit Structure

Organizational Units (OUs) are logical containers used to organize Active Directory objects such as users, computers, and groups.

Unlike folders, OUs are administrative boundaries that allow administrators to:

- Organize resources logically
- Delegate administrative permissions
- Apply Group Policy Objects (GPOs)
- Simplify enterprise management
- Improve scalability

Instead of using the default **Users** container, a custom OU structure was created for better organization and future expansion.

The following hierarchy was implemented:

```text
cyberjourney.lab

├── Employees
│   ├── IT
│   ├── HR
│   └── Finance
│
├── Groups
│
├── Servers
│
└── Workstations
```

> **Screenshot 10 — Active Directory Organizational Structure**
>
> `image/10-active-directory-structure.png`

---

# Why This Structure?

Although this project represents a small enterprise environment, organizing Active Directory correctly from the beginning provides several advantages.

Each Organizational Unit has a specific purpose:

| Organizational Unit | Purpose |
|---------------------|---------|
| Employees | Stores all employee user accounts |
| IT | IT department users |
| HR | Human Resources users |
| Finance | Finance department users |
| Groups | Security Groups |
| Servers | Domain Controllers and future servers |
| Workstations | Domain-joined client computers |

This structure closely resembles how many organizations organize Active Directory, making future administration significantly easier.

---

# Creating Security Groups

Instead of assigning permissions directly to individual users, department-based Global Security Groups were created.

The following groups were added:

| Group Name | Purpose |
|------------|---------|
| GG_IT_ADMIN | IT Department |
| GG_HR_USERS | Human Resources |
| GG_FINANCE_USERS | Finance Department |

Using Security Groups instead of assigning permissions directly to users greatly simplifies administration as the environment grows.

For example, if a new employee joins the IT department, administrators only need to add the user to **GG_IT_ADMIN** rather than manually configuring permissions across multiple resources.

This approach follows Microsoft's recommended security administration practices.

---

# Creating Domain Users

Sample employee accounts were created to simulate users from different departments.

Example accounts include:

| Username | Department |
|----------|------------|
| john.admin | IT |
| sarah.hr | HR |
| michael.finance | Finance |

Each user account was placed inside its corresponding Organizational Unit.

This separation provides a clean and scalable directory structure while making it easier to apply department-specific policies in future projects.

---

# Assigning Users to Security Groups

After creating the user accounts, each employee was added to the appropriate department Security Group.

For example:

```text
john.admin
        │
        ▼
GG_IT_ADMIN
```

```text
sarah.hr
        │
        ▼
GG_HR_USERS
```

```text
michael.finance
               │
               ▼
GG_FINANCE_USERS
```

Rather than assigning permissions directly to user accounts, permissions can later be assigned to Security Groups, allowing administrators to manage access more efficiently.

---

# Enterprise Best Practice

This project follows Microsoft's recommended identity management model:

```text
Accounts
      │
      ▼
Global Groups
      │
      ▼
Permissions
```

As the environment expands, Domain Local Groups can later be introduced to implement the complete **AGDLP** model:

```text
Accounts
      │
      ▼
Global Groups
      │
      ▼
Domain Local Groups
      │
      ▼
Permissions
```

Using this model improves scalability, simplifies administration, and reduces the complexity of permission management.

---

# Preparing for Domain Clients

With the Organizational Units, users, and Security Groups in place, the Active Directory environment was now ready to support domain-joined workstations.

Future client computers would no longer rely on local user accounts. Instead, they would authenticate directly against the Domain Controller using centrally managed domain accounts.

This transition marks the shift from standalone Windows systems to a centrally managed enterprise environment.

---

# Key Takeaways

- Organizational Units provide logical organization and administrative boundaries.
- Department-based OUs simplify enterprise management.
- Security Groups should be used instead of assigning permissions directly to users.
- Proper Active Directory design improves scalability and maintainability.
- A well-structured directory prepares the environment for future Group Policy deployment, delegated administration, and enterprise resource management.

---

**Next:** Part 5 – Deploying the Windows 10 Client, Joining the Domain, and Validating the Environment

# Part 5 - Deploying the Windows 10 Client and Joining the Domain

With the Active Directory infrastructure fully configured, the final deployment phase focused on adding a client workstation to the enterprise environment.

A Windows 10 Pro virtual machine (WS01) was deployed to simulate a standard employee workstation. The objective was to configure networking, join the workstation to the Active Directory domain, and verify that centralized authentication was functioning correctly.

At this stage, the lab transitioned from a standalone server deployment into a functioning enterprise environment where users could authenticate using domain credentials.

---

# Deploying the Windows Workstation

A Windows 10 Pro virtual machine named **WS01** was created using VMware Workstation.

The workstation was connected to the same Host-Only network (**VMnet1**) as the Domain Controller, allowing communication while keeping the lab isolated from the host network.

The workstation specifications were:

| Setting | Value |
|----------|-------|
| Hostname | WS01 |
| Operating System | Windows 10 Pro 22H2 |
| IP Address | 192.168.150.20 |
| Network | VMnet1 (Host-Only) |

After installation, the workstation initially functioned as a standalone Windows machine.

---

# Configuring Network Settings

Because the lab environment does not include a DHCP server, the workstation could not automatically obtain an IPv4 address.

A static network configuration was therefore applied.

| Setting | Value |
|----------|-------|
| IP Address | 192.168.150.20 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | None |
| Preferred DNS | 192.168.150.10 |

The DNS server was intentionally configured to point to the Domain Controller.

This configuration is critical because Active Directory relies on DNS to locate authentication and directory services.

> **Screenshot 11 — Static IP Configuration**
>
> `image/11-static-ip-ws01.png`

---

# Joining the Active Directory Domain

After verifying the network configuration, the workstation was joined to the newly created domain:

```text
cyberjourney.lab
```

Administrative credentials from the Domain Controller were supplied during the domain join process.

Windows successfully completed the operation and displayed the confirmation message:

```text
Welcome to the cyberjourney.lab domain.
```

This message confirmed that:

- The workstation successfully located the Domain Controller.
- DNS resolution was functioning correctly.
- Authentication was successful.
- A secure trust relationship had been established.

> **Screenshot 12 — Domain Join Successful**
>
> `image/12-domain-join-success.png`

---

# What Happens During a Domain Join?

Although joining a domain appears to be a simple operation, several processes occur automatically behind the scenes.

```text
WS01
   │
   ▼
Query DNS Server
   │
   ▼
Locate Domain Controller
   │
   ▼
Authenticate Administrator Credentials
   │
   ▼
Create Computer Account in Active Directory
   │
   ▼
Establish Secure Trust Relationship
   │
   ▼
Restart Workstation
```

After restarting, the workstation officially became a managed member of the Active Directory domain.

---

# Logging in with a Domain Account

Instead of using a local Windows account, authentication was performed using a domain user account.

Example:

```text
CYBERJOURNEY\john.admin
```

Successful authentication confirmed that Kerberos was functioning correctly and that the workstation was communicating with the Domain Controller.

This is the same authentication workflow commonly used in enterprise Windows environments.

> **Screenshot 13 — Domain User Login**
>
> `image/13-domain-login.png`

---

# Validating Network Connectivity

To verify communication between the workstation and the Domain Controller, several networking tests were performed.

The first test confirmed basic IP connectivity.

```cmd
ping 192.168.150.10
```

Next, DNS resolution was verified.

```cmd
nslookup cyberjourney.lab
```

Both commands completed successfully, confirming that:

- The workstation could communicate with the Domain Controller.
- DNS services were functioning correctly.
- The Active Directory domain was reachable.

> **Screenshot 14 — Network Validation**
>
> `image/14-network-validation.png`

---

# Verifying Active Directory Registration

Once the workstation joined the domain, Active Directory automatically created a corresponding computer object.

The new workstation appeared inside **Active Directory Users and Computers (ADUC)**, confirming that the computer had been successfully registered within the domain.

This registration enables centralized management through:

- Group Policy
- Security Policies
- Remote Administration
- Enterprise Authentication
- Software Deployment

The successful creation of the computer object completed the workstation integration process.

> **Screenshot 15 — Computer Object Registered**
>
> `image/15-computer-object.png`

---

# Final Environment

After completing the deployment, the lab environment consisted of:

```text
cyberjourney.lab

├── DC01
│   ├── Active Directory
│   ├── DNS
│   └── Domain Controller
│
└── WS01
    ├── Windows 10 Pro
    ├── Domain Member
    └── Authenticated using Domain Account
```

The environment now supports centralized authentication, user management, and future enterprise services.

---

# Key Takeaways

- Windows clients rely on DNS to locate Domain Controllers.
- Static IP addressing is required when DHCP is unavailable.
- Joining a workstation automatically creates a computer object in Active Directory.
- Domain authentication uses Kerberos rather than local account validation.
- Successfully joining a workstation validates that networking, DNS, Active Directory, and authentication services are all functioning correctly.

---

**Next:** Part 6 – Validation, Challenges Encountered, Lessons Learned, Future Improvements, and Conclusion

# Part 6 - Validation, Challenges, Lessons Learned, and Future Improvements

With both the Domain Controller and the Windows 10 workstation successfully deployed, the Active Directory environment was fully operational.

The project achieved its primary objective of building a small enterprise Active Directory infrastructure capable of centralized authentication, user management, and domain administration.

More importantly, this lab established a reusable foundation for future Windows administration, Blue Team, and Active Directory penetration testing projects.

---

# Project Validation

The following components were successfully implemented and verified throughout the deployment.

| Component | Status |
|-----------|--------|
| VMware Host-Only Network | ✅ |
| Windows Server 2022 Installation | ✅ |
| Windows Server Configuration | ✅ |
| Active Directory Domain Services (AD DS) | ✅ |
| DNS Server Configuration | ✅ |
| Domain Controller Promotion | ✅ |
| New Forest Creation | ✅ |
| Organizational Unit Design | ✅ |
| Security Group Creation | ✅ |
| Domain User Management | ✅ |
| Windows 10 Workstation Deployment | ✅ |
| Static IP Configuration | ✅ |
| Domain Join | ✅ |
| Domain User Authentication | ✅ |
| Computer Object Registration | ✅ |
| Network Connectivity Validation | ✅ |

Every planned objective for this version of the project was successfully completed.

---

# Commands Used

The following commands were used to validate network connectivity and Active Directory functionality.

## Display Network Configuration

```cmd
ipconfig
```

**Purpose**

Displays the workstation's current TCP/IP configuration.

Used to verify:

- IPv4 Address
- Subnet Mask
- DNS Server
- Network Adapter

---

## Test Network Connectivity

```cmd
ping 192.168.150.10
```

**Purpose**

Verifies communication between the workstation and the Domain Controller.

Successful replies confirm that Layer 3 connectivity is functioning correctly.

---

## Verify DNS Resolution

```cmd
nslookup cyberjourney.lab
```

**Purpose**

Confirms that the workstation can resolve the Active Directory domain through the configured DNS server.

---

## Verify Current User

```cmd
whoami
```

**Purpose**

Displays the identity of the currently authenticated user.

Expected output:

```text
cyberjourney\john.admin
```

---

## Verify Authentication Server

```cmd
echo %LOGONSERVER%
```

**Purpose**

Displays the Domain Controller responsible for authenticating the current user.

Expected output:

```text
\\DC01
```

---

# Challenges Encountered

No infrastructure deployment is complete without troubleshooting. Several issues were encountered during this project, providing valuable learning opportunities.

## Challenge 1 — APIPA Address (169.254.x.x)

During the initial deployment of WS01, Windows assigned an Automatic Private IP Address (APIPA).

### Root Cause

The isolated lab environment did not include a DHCP server.

As a result, Windows could not automatically obtain an IPv4 address.

### Resolution

A static IPv4 configuration was manually assigned.

```text
IP Address:
192.168.150.20

Subnet Mask:
255.255.255.0

DNS:
192.168.150.10
```

---

## Challenge 2 — Domain Join Requirements

Initially, the workstation was unable to communicate with the Active Directory environment correctly.

### Root Cause

The workstation must use the Domain Controller as its DNS server.

Without proper DNS configuration, Windows cannot locate the Domain Controller.

### Resolution

Configured the Preferred DNS Server as:

```text
192.168.150.10
```

After updating the DNS configuration, the workstation successfully joined the domain.

---

## Challenge 3 — Organizational Unit Protection

While reorganizing the Active Directory structure, Organizational Units could not be moved or deleted.

### Root Cause

The **Protect object from accidental deletion** option had been enabled when the OUs were created.

### Resolution

Enabled **Advanced Features** in Active Directory Users and Computers and removed the protection attribute before modifying the OU structure.

---

# Skills Demonstrated

This project demonstrates practical experience in several enterprise technologies.

## Windows Server Administration

- Installing Windows Server 2022
- Configuring server roles
- Basic server administration

---

## Active Directory Administration

- Installing AD DS
- Promoting a Domain Controller
- Creating a Forest
- Managing Organizational Units
- Managing Users
- Managing Security Groups

---

## Windows Networking

- Static IPv4 configuration
- DNS configuration
- Host-Only virtual networking
- Network troubleshooting

---

## Enterprise Administration

- Domain user management
- Centralized authentication
- Security group administration
- Enterprise directory organization

---

## VMware Virtualization

- Virtual machine deployment
- Virtual network configuration
- Multi-machine lab environment

---

# Lessons Learned

This project reinforced several key concepts about enterprise Windows environments.

- Active Directory is heavily dependent on DNS.
- A Domain Controller should always use a static IP address.
- Proper Organizational Unit design improves scalability and administration.
- Security Groups provide a more efficient way to manage permissions than assigning access directly to users.
- Successful domain authentication depends on multiple services working together, including networking, DNS, Kerberos, and Active Directory.

Perhaps the most valuable lesson learned was understanding that Active Directory is far more than a database of users and computers.

It is an integrated ecosystem where networking, authentication, directory services, and security all work together to provide centralized identity management.

---

# Future Improvements

This project serves as Version 1 of a much larger enterprise home lab.

Future enhancements will include:

- Deploy additional Windows workstations
- Create multiple departments and user accounts
- Implement Group Policy Objects (GPO)
- Deploy a Windows File Server
- Configure NTFS and SMB permissions
- Implement password and account lockout policies
- Configure Windows Event Forwarding (WEF)
- Deploy Microsoft LAPS
- Perform Active Directory enumeration with BloodHound
- Simulate Kerberoasting attacks
- Explore privilege escalation techniques
- Integrate SIEM solutions for monitoring and detection

These additions will gradually transform the environment into a realistic enterprise infrastructure suitable for both system administration and cybersecurity research.

---

# Conclusion

This project successfully demonstrated the end-to-end deployment of a small enterprise Active Directory environment using Windows Server 2022, Windows 10 Pro, and VMware Workstation.

Starting from an empty virtual environment, a dedicated Host-Only network was configured, a Domain Controller was deployed, Active Directory Domain Services and DNS were installed, Organizational Units and Security Groups were designed, domain users were created, and a Windows 10 workstation was successfully joined to the domain.

Beyond simply completing the deployment, this project provided hands-on experience with the core technologies that underpin enterprise Windows environments.

The completed lab now serves as a strong foundation for future projects involving Group Policy, Windows Server administration, enterprise networking, Active Directory security, and offensive security techniques such as BloodHound, Kerberoasting, and privilege escalation.

Building this environment from scratch significantly improved my understanding of how enterprise identity management works and established a practical platform for continued learning in both Windows administration and cybersecurity.