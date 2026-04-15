# Cloud-Based-Active-Directory-Setup-and-User-Management
This project provides hands-on experience setting up and managing a cloud-based Active Directory environment, from provisioning virtual machines to user administration.

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/b8540cab2b28d305fd721ff6d08b54a326b01e0f/images/intro.webp)



Required Tools / Prerequisites

To complete this project, learners will need:

● Cloud provider free tier (Azure, AWS, or Google Cloud Platform). Azure offers a
  12-month free tier including Windows VMs.
  
● Two Windows Server VMs (can be trial versions, e.g., Windows Server 2022
  Evaluation).
  
● Remote Desktop (RDP) to access the VMs.

● Basic knowledge of Windows Server and networking.

## Common Azure Free Account Blockers (Quick Tips)

During Azure free account creation and initial VM deployment, learners may encounter the
following common blockers. These quick tips help prevent setup delays.

● Card Verification Issues
Issue: Card is declined during signup.
Workaround: Use a supported card, either credit or debit (Visa, Mastercard,
1
American Express, JCB), and ensure online and international transactions are
enabled. Corporate or virtual cards may not be accepted.

● Subscription or Region Not Available
Issue: Selected region or subscription does not allow VM creation.
Workaround: Choose a nearby region (e.g., Southeast Asia) and retry deployment if
temporary capacity limits are encountered.

● VM Quota or vCPU Limits
Issue: VM creation fails due to quota or core limits in the free-tier subscription.
Workaround: Use B-series VM sizes as specified in this lab and stop or deallocate
unused VMs to free resources.


## Instructions

Before creating your Windows VMs, you must set up your Azure environment. Azure’s free
tier gives you enough credits and resources for this project.

Step A: Create a Microsoft Azure Free Account
1. Go to: https://azure.microsoft.com/free
2. Click Try Azure for Free

  ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/1500f523377fd0958ffc1b97272db8524be1cc49/images/1.png)

   
3. Sign in using a Microsoft account

● If you don’t have one, click Create one!

  ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/1500f523377fd0958ffc1b97272db8524be1cc49/images/2.png)

4. Provide:
   
● Use of Azure Account: For Personal Use
● Country
● Name
● Phone number (SMS verification required)
● Address Information


5. Enter payment information
● Only for identity verification.
● Only Accepts the following Cards (Credit or Debit)
         ○ (Visa, Mastercard, American Express, JCB).
● You will NOT be charged unless you manually upgrade.

6. Accept terms → Click Sign Up

Step B: Access the Azure Portal

1. Go to https://portal.azure.com
   
2. Sign in to your account
   
3. You should now see the Azure dashboard

   ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/916306209c62c1d3304a974ef1989b5f2acaf0d1/images/3.png)


## Step C: Create a Resource Group

1. Search for Resource Groups

 ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/6e8cbffd87023c7303d1c7b85db3e319cb09f00a/images/4.png)

   
2. Click Create
3. Configure:
   
      ○ Subscription: Free Trial
   
      ○ Resource Group Name: AD-Lab
   
      ○ Region: your nearest region (e.g., Southeast Asia)
   
4. Click Review + Create → Create

 ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/6e8cbffd87023c7303d1c7b85db3e319cb09f00a/images/5.png)


## Step D: Create a Virtual Network (VNet)

Both VMs must be on the same private network.

1. Search for Virtual Network
   
2. Click Create

  
3. Configure:
   
          ○ Resource Group: AD-Lab
   
          ○ Name: AD-VNet
   
          ○ Region: same as Resource Group
  
4. Leave the default Address Space and Subnet
   
5. Click Review + Create → Create

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/a7e630f25a6889b0c926e985b0a4fa53bc44b6c4/images/6.png)
With the Resource Group and VNet ready, you can now proceed to Step 1, where you will
deploy the Domain Controller and client VM



# Step 1: Provision Cloud Virtual Machines

Create two Windows Server VMs in your account:

## 1. VM1 – Domain Controller (DC01)

* **Purpose:** Hosts Active Directory Domain Services (AD DS), DNS, and serves as the primary domain controller for the lab environment.
* **Configuration:**
    * **OS:** Windows Server 2022
    * **Size:** B2als_v2
        * **vCPUs:** 2
        * **RAM:** 4 GB
        * **Temp Storage:** 4 GiB
        * **Network Bandwidth:** 3750 Mbps
        * **Cost Estimate:** ~$41.17/month
        * **Reason for Choosing:** Cost-efficient, enough CPU/RAM for AD DS + DNS in a lab, more affordable than B2s/B2as.
* **Network:** AD-VNet
* **Subnet:** default
* **Public IP:** Optional (recommended only for RDP access; use NSG restrictions)
* **Private IP:** Static (recommended for domain controllers)
* **Inbound Ports:** RDP (3389)
* **Notes:** Ensure time sync and DNS settings point back to the DC itself.

  ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/f3fd8eb5c05c12a2347c1b4816199495f924cf7c/images/7.png)

## 2. VM2 – Client Machine (CLIENT01)

* **Purpose:** Acts as a domain-joined workstation used to test authentication, Group Policies, and user account management.
* **Configuration:**
    * **OS:** Windows 10, Windows 11, or Windows Server 2022
    * **Size:** B2als_v2
        * **vCPUs:** 2
        * **RAM:** 4 GB
        * **Temp Storage:** 4 GiB
        * **Network Bandwidth:** 3750 Mbps
        * **Cost Estimate:** ~$41.17/month
        * **Reason for Choosing:** Matches DC performance, ensures smooth RDP usage, and is stable for client operations.
* **Network:** AD-VNet
* **Subnet:** default
* **Private IP:** Dynamic is acceptable
* **Inbound Ports:** RDP (3389)
* **DNS Settings:** Must point to DC01’s private IP before joining the domain
* **Notes:** Keep OS patched before domain join for secure configuration.

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/f3fd8eb5c05c12a2347c1b4816199495f924cf7c/images/8.png)

### **Important Requirement**

Both VMs must be in the same virtual network (**AD-VNet**) and the same region. This allows:

* Domain join
* DNS resolution
* Group Policy communication
* Realistic enterprise LAN simulation



# Step 2: Configure Domain Controller

### 1. Log in to VM1 (Domain Controller) via RDP

* Go to **Virtual Machines** → select **DC01** → click **Connect**.
    * Download the RDP file and open it.
    * Use the credentials you set when creating the VM.
    * The Remote Desktop session will open the Windows Server desktop.
 
     ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/fed512a746c1606c5b6d7fe3ae5f5808f87048c3/images/9.png)

### 2. Open Server Manager

* Launch **Server Manager** (usually starts automatically; if not, click **Start Menu** → **Server Manager**).
* Click **Add Roles and Features**.
* In the **Add Roles and Features Wizard**:

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/fed512a746c1606c5b6d7fe3ae5f5808f87048c3/images/10.png)


    * **Installation Type:** Select *Role-based or feature-based installation* → Next.
    * **Server Selection:** Select **DC01** → Next.
    * **Server Roles:** Check **Active Directory Domain Services** → a pop-up will ask to add features → click **Add Features** → Next.
    * **Features:** Leave default → Next.
    * **AD DS:** Read description → Next.
    * **Confirmation:** Check *"Restart the destination server automatically if required"* → click **Install**.
    * *Note: The installation might take a few minutes.*

### 3. Promote to Domain Controller

* After installation, click the **Notifications flag** in Server Manager → **Promote this server to a domain controller**.

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/fed512a746c1606c5b6d7fe3ae5f5808f87048c3/images/11.png)

  
* **Deployment Configuration:**
    * Choose **Add a new forest** → **Root domain name:** `corp.local` → Next.
* **Domain Controller Options:**
    * **Forest and Domain Functional Levels:** Default (Windows Server 2022).
    * Check **DNS Server** → Next.
    * Set **DSRM password** (Directory Services Restore Mode). *Remember this password for recovery* → Next.
* **Additional Options:** Leave default → Next.
* **Paths:** Leave default (`C:\Windows\NTDS`, etc.) → Next.
* **Review Options:** Confirm → Next.
* Click **Install**. The server will automatically reboot after promotion.
* After reboot, **DC01** is now the Domain Controller for the new forest `corp.local`.

> **Note:** During the domain setup, a domain administrator account named **"adminuser"** is created. This account has Domain Admin privileges and is used throughout the lab for administrative tasks.

### 4. Verify AD DS and DNS

* Log back into **DC01**.
* Open **Server Manager** → **Tools** → **Active Directory Users and Computers**.

![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/fed512a746c1606c5b6d7fe3ae5f5808f87048c3/images/12.png)

* You should see your domain `corp.local` listed.
* Open **DNS** from **Server Manager** → **Tools** → **DNS** to check that your domain zones are created automatically.


# Step 3: Join Client VM to the Domain

### 1. Log in to VM2 (Client machine)

* Follow the same RDP process used for VM1 to connect to **CLIENT01**.
* Use the local credentials set during the VM creation.
* **Configure DNS Settings:**
    * Ensure **CLIENT01’s** DNS Server is set to **DC01’s** private IP.

     ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/c1f7f29560073a917f8133335b22d85075053ac3/images/13.png)



    * Open **Settings** → **Network & Internet** → **Ethernet** → **Change adapter options**.
    * Right-click your adapter → **Properties** → **IPv4** → **Properties**.
    * Select **Use the following DNS server addresses** → **Preferred DNS server** → Enter **DC01's private IP** → **OK**.


      ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/c1f7f29560073a917f8133335b22d85075053ac3/images/14.png)



### 2. Join the Domain

* Open **System Properties**:
    * Right-click **This PC** → **Properties** → **Advanced system settings**.
    * Go to the **Computer Name** tab → click **Change…**


     ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/3d561c9638bdbbe9d21f60277f388d4a6b464614/images/15.png)


      
* **Member of:**
    * Select **Domain**.
    * Enter the domain: `corp.local` → Click **OK**.
* **Authentication:**
    * A pop-up will ask for credentials → enter the **Domain Admin** username (`adminuser`) and the password created for **VM1 (DC01)**.
    * Click **OK**.
* **Restart:**
    * A message will appear: *"Welcome to the corp.local domain."*
    * Click **OK** and then **Restart Now** to apply the changes.

### 3. Verify Domain Membership

* After the reboot, log in with domain credentials:
    * On the login screen, select **Other User**.
    * **Username:** `corp\adminuser`
    * **Password:** (The same password used for DC01).
* **Confirm Identity via Command Prompt:**
    * Open CMD and type: `whoami`
    * **Expected Output:** `corp\adminuser`

      ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/3d561c9638bdbbe9d21f60277f388d4a6b464614/images/16.png)


# Step 4: Create Users and Organizational Units (OUs)

### 1. Open Active Directory Users and Computers

* On **VM1 (DC01)**, open **Active Directory Users and Computers (ADUC)**.
* Path: **Server Manager** → **Tools** → **Active Directory Users and Computers**.

### 2. Create an Organizational Unit (OU)

* Right-click your domain (`corp.local`) → **New** → **Organizational Unit**.
* **Name:** `TestUsers`
  
    ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/165fbeb22758bbb028030891feec4a33ee1141e3/images/17.png)

### 3. Create New Users

Inside the **TestUsers** OU, create the following users:
* **Alice**
    * **First name:** Alice → **Last name:** N/A → **User logon name:** `alice` → **Next**.
    * Set password → Check **"User must change password at next logon"** → **Next** → **Finish**.
* **Bob**
    * Repeat the process for **Bob**.
* **Charlie**
    * Repeat the process for **Charlie**.

*Note: Ensure all users are added to the **Domain Users** group (this happens by default).*

 ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/165fbeb22758bbb028030891feec4a33ee1141e3/images/18.png)

### 4. Manage Group Membership

* In **ADUC**, right-click a user → **Properties** → **Member Of** tab.
* Click **Add** to include users in custom groups if needed for specific lab scenarios.

 ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/165fbeb22758bbb028030891feec4a33ee1141e3/images/19.png)
> This simulates user provisioning and organizational structure, both of which are critical areas monitored by SOC analysts during security investigations.


# Step 5: Reset Password & Add User to “Remote Desktop Users” Group

### 1. Reset User Password (on DC01)

* Open **Active Directory Users and Computers**:
    * **Server Manager** → **Tools** → **Active Directory Users and Computers**.
* Navigate to: `corp.local` → **TestUsers** → **alice**.
* Right-click **Alice** → **Reset Password**.
* **Set a new password.**
* **UNCHECK:** "User must change password at next logon".
* Click **OK**.

  ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/c8d33b98ba0cec1bdcafe78a89e7611e2b64c897/images/20.png)
  
* *Note: This resolves issues related to changing passwords during initial RDP sessions.*

### 2. Grant RDP Permissions (on CLIENT01)

* Log in to **CLIENT01** using an Administrator account (e.g., `corp\adminuser`).
* Open **System Properties**:
    * Press `Win + R`.
    * Type: `sysdm.cpl` and press **Enter**.
* Go to the **Remote** tab:
    * Click **Select Users…**.
    * Click **Add**.
    * Enter: `corp\alice`.
    * Click **Check Names** → **OK** → **OK**.

      ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/c8d33b98ba0cec1bdcafe78a89e7611e2b64c897/images/21.png)

> This process adds Alice to the local **Remote Desktop Users** group, allowing her to log in to the workstation remotely.


# Step 6: Test User Accounts

Ensure users can log in and communicate with domain resources.

### 1. Log in with a Domain Account

* On **CLIENT01 (VM2)**, log out of the **Administrator** account.
* Log in with a new domain account, e.g., `corp\alice`.
* Enter the username and password created in Step 4.

### 2. Verify Domain Access

* **Check Identity:**
    * Open **Command Prompt** → run: `whoami`

        ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/e66fa8dd930bbd800061df45946d2d06419798c5/images/22.png)


    * **Output:** `corp\alice`
* **Test Network Access:**
    * Press `Windows + E` to open **File Explorer**.
    * Click the address bar at the top and type: `\\DC01` → Press **Enter**.
    * *Note: You may see a blank window if no folders have been shared yet, but it should connect without an error.*
* **Test Connectivity:**
    * Open **Command Prompt**.
    * Type: `ping <DC01_IP_Address>`
    * **Expected Result:** You should receive successful replies from the Domain Controller.


        ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/e66fa8dd930bbd800061df45946d2d06419798c5/images/23.png)




# Step 7: Monitoring

### 1. Review Security Logs on the Domain Controller

* Log in to **DC01** as a domain administrator: `corp\adminuser`.
* Open **Event Viewer**:
    * Click **Start Menu** → Search for **Event Viewer** → Open the application.
* Navigate to: **Event Viewer** → **Windows Logs** → **Security**.


    ![images alt](https://github.com/salimelh94/Cloud-Based-Active-Directory-Setup-and-User-Management/blob/590be65bdfd328abded77868d39476565e9da043/images/24.png)

  
* *Note: This log contains authentication events, account management activity, and policy changes, which are critical for SOC investigations.*

### 2. Identify Key Active Directory Security Events

Use **Filter Current Log…** (right panel) to search for the following Event IDs:

#### **Authentication Events**
| Event ID | Description | SOC Relevance |
| :--- | :--- | :--- |
| 4624 | Successful logon | Confirms valid user authentication |
| 4625 | Failed logon | Detects brute-force or password guessing |
| 4648 | Logon using explicit credentials | Indicates lateral movement attempts |

#### **Account Management Events**
| Event ID | Description | SOC Relevance |
| :--- | :--- | :--- |
| 4720 | User account created | Tracks user provisioning |
| 4722 | User account enabled | Detects reactivated accounts |
| 4723 / 4724 | Password change/reset | Monitors credential changes |
| 4725 | User account disabled | Tracks access revocation |
| 4726 | User account deleted | Identifies account removal |

#### **Domain & Group Changes**
| Event ID | Description | SOC Relevance |
| :--- | :--- | :--- |
| 4732 | User added to a security group | Privilege escalation monitoring |
| 4733 | User removed from a security group | Access change tracking |
| 4735 | Security group modified | Detects group tampering |

> **Important Note: Event ID Variations**
> Some Windows Security Event IDs may vary depending on the Windows Server version, Client OS, Domain Functional Level, and Audit Policy configuration. For example, authentication events may appear on both the client and DC but with different Logon Types.

### 3. Correlate Events With Lab Actions

Map events to the actions performed earlier in this lab:

| Lab Action | Expected Event |
| :--- | :--- |
| Password reset | 4724 |
| Account logs in | 4624 |
| Scheduled Task was Updated | 4702 |
| Special privileges assigned to new logon | 4672 |
| Group Membership Information | 4627 |
| A new process has been created | 4688 |

*Note: If an expected Event ID is not found, verify the action was successful and that you are reviewing logs on the correct system (DC01 vs CLIENT01).*

### 4. Export Logs for Analysis

* In **Event Viewer** → **Security**:
    * Right-click **Security**.
    * Click **Save All Events As…**.
    * Save as: `DC01-SecurityLogs.evtx`.

*Analysts often export logs to SIEM platforms such as Splunk, Sentinel, or ELK for centralized analysis and alerting.*


## Optional Azure Security Tools (Free-Tier Friendly)

In addition to the native Windows Event Viewer, Azure provides built-in security and
monitoring tools that can be explored within the free-tier subscription. These tools help
learners understand how cloud-native security controls integrate with Active Directory
environments.

Explore additional Azure security features within free-tier credits.

1. Microsoft Defender for Cloud
   
Purpose: Cloud security posture management and basic threat protection.
What can be explored:

● View security recommendations for virtual machines

● Identify exposed management ports (e.g., RDP 3389)

● Review baseline security posture and compliance suggestions

SOC Relevance:

Introduces how security teams identify misconfigurations and prioritize risks in
cloud-hosted Windows environments. The free tier includes foundational security
posture assessments. Advanced workload protection is optional and not required
for this lab.

2. Azure Network Security Groups (NSG)

Purpose: Network-level traffic filtering for cloud resources.
What can be explored:

● Restrict RDP access to a specific IP range

● Understand inbound vs outbound security rules

● Simulate basic perimeter hardening for domain controllers

SOC Relevance:

Demonstrates how access control and network segmentation reduce attack
surfaces and support incident response. NSGs are included at no additional cost
and are fully usable within free-tier subscriptions.

## Summary

By completing this project, learners will:

● Provision and configure a cloud-based Active Directory environment using Microsoft
Azure, including virtual networks, resource groups, and Windows virtual machines.

● Install and promote a Windows Server to a Domain Controller, creating a new Active
Directory forest and configuring DNS for enterprise identity management.

● Join a client machine to the domain and verify domain membership, authentication,
and network communication.

● Create and manage Organizational Units (OUs) and user accounts, simulating
real-world user provisioning and access management workflows.

● Troubleshoot common Active Directory issues such as DNS misconfigurations,
domain join failures, and user login problems.

● Develop foundational SOC-relevant skills by understanding how centralized identity,
authentication events, and domain activity are used to support security monitoring
and incident investigations.


