###

###

###

###

### SERVER NAMING STANDARDS

###

|||
| --- | --- |
| **Name** | Server Naming Standards |
| **Version** | 1.0 |
| **Date** | 7/11/19 |
| **Produced by** | Douglas Jewell |
| **Reviewed by** |   |
1\. **Introduction**

1.1 **Presentation**

The goal of this document is to provide an update on the current Server naming convention and include in the updated naming convention described here under additional elements to have a future proof naming convention.

To follow market best practices on the subject, this naming convention has taken the following elements into account:

1. **Server location:** In a today naming convention, the recommendation is not to include any location information in the server name.  Indeed, with the arrival of virtualization technologies inside the datacenter, we can easily have a machine that is today located in a datacenter that moves to another location.  In this case, the server location contained in the name will not be accurate anymore.  If we would have the location information for each server, it is recommended to use one of the Active Directory field or another tool to put location information.  Doing this is like treating the server location as a parameter or attribute that can be changed without impact.  Indeed, changing a server name is technically feasible but will have consequences and for some products, it means full reinstallation.  To align with this recommendation, no locations information have been included in this naming convention.
2. **Server name length:** During discussions, we agreed that the server name should not be too long.  As a best practice, the total server name will start with exactly 7 characters.  All virtual machines will then be assigned a sequential 3 digit number.  
3. **Special characters:** Special Characters are generating issues when scripting is used.  Indeed, in most scripting languages like PowerShell or Perl, special characters like &quot;-&quot; are interpreted as &quot;minus&quot; signs and therefore scripts are failing.  The best practice is to only use standard letters and numbers in a Windows Active Directory based server / desktop naming convention.  All special characters have therefore been removed from the naming convention.



1.2 **Change history**

| Version | Nature of change | Date |
| --- | --- | --- |
| 1.0 | First version | 7/11/2019 |
| 1.1 | Revsion  | 4/8/2020  |

2.&nbsp;**Naming Convention Structure**

2.1 **The Structure**

The naming convention is divided into five blocks.  Each block plays a specific role in the naming convention structure:

- Block 1: Provides the type of server with 3 characters
- Block 2: Provides the environment with 1 character
- Block 3: Provides the operating system with 1 character
- Block 4: Provides the functionality of the server with 2 characters
- Block 5: Provides the 3-digit server number or application name, created in sequential order

With these five blocks, we end-up with a name that always starts with 7 characters and ends with either a sequential server number or hosted application name.

2.2 **The naming in itself**

A virtual machine will always have the following structure:

**&quot;TTTEO00111&quot;** where we can see that each server has a name in 10 positions.

-  The **&quot;TTT&quot;** is representing the Type.
  - phy = Physical
  - azr = Azure
  - aws = Amazon Web Services
  - vmw = VMWare
  - hpv = HyperV
-  The **&quot;E&quot;** is representing the Environment.
  - p = Production
  - q = QA
  - d = Development
-  The **&quot;O&quot;** is representing the operating system.
  - w = Windows
  - l = Linux
  - u = Unix
  - m = MacOS
  - o = Other/Proprietary
-  The **&quot;00&quot;** are describing the services are being provided. (see 2.3)
-  The **&quot;111&quot;** at the end of the machine name is a sequentially increasing number.

Azure PaaS Services will replace the sequentially increasing server number with the application name.  It will be kept the shortest possible and not contain any special characters.

2.3  **List and numbering of provided services**

| Function Code | Description |
| --- | --- |
| 10 | Internal API/Service PaaS Web Server |
| 11 | Internal API/Service IaaS Web Server |
| 12 | Internal Presentation PaaS Server |
| 13 | Internal Presentation IaaS Server |
| 14 | External Presentation PaaS Server |
| 15 | External Presentation IaaS Server |
| 20 | IaaS Database Server |
| 21 | PaaS Database Server |
| 72 | Active Directory Domain Controllers |
| 75 | Certificate Authorities |
| 80 | Jumpboxes/Server Workstations |
| 85 | Tools, Utilities and Monitoring |

2.4 **Naming Examples**

You&#39;ll find here under some examples built using the new naming convention as example and references:

Physical Windows SQL Production Server:

- 3 positions = **phy** as it&#39;s a Physical server
- 4th position = **p** as it&#39;s a Production server
- 5th position = **w** as it&#39;s a Windows operating system
- 6th to 7th position = **20** This is a SQL server IaaS virtual machine. (see 2.3)
- 8th to 10th position = **000** for first sequential number of this configuration
  - Ex. phypw20000

Development Azure Linux External Web Server:

- 3 positions = **azr** as it&#39;s hosted in Azure
- 4th position = **d** as it&#39;s a Development server
- 5th position = **l** as it&#39;s a Linux operating system
- 6th to 7th position = **11** This a Linux IaaS virtual machine. (see 2.3)
- 8th to 10th position = **000** for first sequential number of this configuration.
  - Ex. azrdl11000

