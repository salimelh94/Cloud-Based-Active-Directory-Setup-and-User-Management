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















