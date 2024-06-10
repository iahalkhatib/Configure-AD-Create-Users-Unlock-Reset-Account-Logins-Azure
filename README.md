# Configure-AD-Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/bc6d241f-d979-47d0-a704-f4c7220381fe)



# 1. Task 1. Set Up Rescources in Azure 

I will create the Domain Controller VM (Windows Server 2022) named "DC-1" and take note of the Resource Group and Virtual Network (VNet) that get created at this time.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/82829f83-5707-4a06-b6b0-fbe91a54aab7)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/9b66045d-a300-4ede-9f44-8d9ebfcd1e32)

I will set the Domain Controller's NIC Private IP address to be static.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/082436bd-2256-4586-b60f-20d778e1d670)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/4386c158-4580-4693-8403-c81161920f3b)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/44ac1cdf-69f3-4f22-94e8-b079fdd2a7b6)


I will create the Client VM (Windows 10) named "Client-1" using the same Resource Group and VNet that was created earlier. I will ensure that both VMs are in the same VNet, and I can check the topology with Network Watcher.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/00b16f11-ff0d-4b2a-a10c-8fbd0c2e488d)

I will ensure connectivity between the client and Domain Controller by following these steps:

1. Login to Client-1 with Remote Desktop.
2. Ping DC-1's private IP address with a perpetual ping using the command `ping -t <ip address>`.
3. Login to the Domain Controller.
4. Enable ICMPv4 in the local Windows Firewall.
5. Check back at Client-1 to see if the ping succeeds.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/b97a32be-5fdb-43ca-b240-3f0142ae990f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f7551c32-7872-48ba-95fa-1f25d038a5b9)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/29f67032-f8d7-4ec8-a3df-bd357c13fc14)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/1c757ce7-a4c2-4599-8878-b3a9fcefd2ec)

I will use DC-1's private IP to ping from Client-1 and see it fail due to DC-1's firewall blocking ICMP traffic.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/d2271cdd-3bb8-4497-888a-3938ee58f656)

I will open a new Remote Desktop Protocol (RDP) session to connect to DC1 using the public IP of DC1.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/47783d14-3ffb-4a77-bd5e-b8780296438f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/812e6269-bfaa-44e0-80c4-93ce6bf0b9f4)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/29c9aa21-746e-46a0-8f8f-35ecf6d8d2cb)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f4c930f0-0282-4412-b779-367493bacb60)

I will check on Client-1 and see the ping going through to DC-1.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/24976979-3769-4cfd-ba90-dc0e04d6c833)

# 2. Task 2. Install Active Directory 

I will login to DC-1 and install Active Directory Domain Services.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f765efae-a00c-4467-9325-3a4db94eb7fa)

To verify that I am in DC-1, I can check the computer name or the IP address of the machine.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/e520df82-27b5-4dca-84ca-8f081f8dade3)

I will open Server Manager on DC-1 and go to "Add Roles and Features".

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/887c0eba-f5bd-4c97-8f55-bcf9ca2f344d)

I will select "Active Directory Domain Services" only from the list of roles and features to add on DC-1.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/a858c678-3178-4101-8b09-2ede8bd5f40c)

I will promote DC-1 as a domain controller and set up a new forest with the domain name "mydomain.com".

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f9de74e3-5aea-43b6-98bc-60b72fd2659f)

I will restart DC-1 and then log back in as the user "mydomain.com\labuser".

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2bb5d5c3-b849-4665-a668-31b74d82a2bf)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/6e26670f-7ed0-4375-9c7f-a9314abdbc2f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/1f8b1e4b-7afa-40b2-8937-5daf29efebe1)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/51ccffea-b45f-4d90-b720-b89b770c826f)

I will create an admin and a normal user account in Active Directory.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/791b6876-e78a-4895-9eef-28e0d8eb931b) 

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/791b6876-e78a-4895-9eef-28e0d8eb931b 

 I will create an Organizational Unit (OU) called "_EMPLOYEES" in Active Directory Users and Computers (ADUC).

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/1058c813-b90f-487f-9f34-f417c3d47ac8)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/326cf341-38b2-4154-a4c9-8b16d0209458)


I will create a new Organizational Unit (OU) named "_ADMINS" in Active Directory Users and Computers (ADUC).

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/40b0b72e-2972-4e07-aa55-2ece916edfad)


I will create a new employee named "Jane Doe" with the username "jane_admin" and the same password.
 
![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8187183d-2392-4a16-ad5d-ef0a83eeeca8) 

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8e9161b4-f970-4755-8dd0-c686373be31b)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/1c8bceb5-a75d-4a7d-b068-834ec5b971c6)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/436265ef-872e-4976-80d7-f85aa205444a)

I will add "jane_admin" to the "Domain Admins" security group.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/c1a794fe-939e-4798-a5b3-8fab1f7a11ff)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/6f498d48-853c-4150-98db-771b828ea8b1)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/a9a0059c-b7b2-441d-ae71-ff0a8ef075a8)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2b0432d1-11e7-4c2a-b25b-ff52e29dd835)

I will log out/close the Remote Desktop connection to DC-1 and log back in as "mydomain.com\jane_admin".

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/4b6c3b04-d240-4a84-89cb-46cc34b98ec3)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/ba539d00-867e-4809-84ba-dd6108765026)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/9185d58d-ad83-45d6-b027-08746dd7435c)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8a80c651-b9a8-4afb-a8ad-89ba2bf4b151)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/aadd0669-5425-42f7-8203-e94c7fe76822)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/723a6f59-3bbd-4c02-8681-d950c964a797)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/4b060f97-f4d4-4cf8-b4b0-597cdb24dcc5)

I will use "jane_admin" as my admin account from now on.

# 3. Task 3. I will join Client-1 to the domain "mydomain.com" and perform the necessary steps:

1. From the Azure Portal, I will set Client-1's DNS settings to the DC's Private IP address.
2. I will restart Client-1 from the Azure Portal.
3. I will log in to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (which will cause the computer to restart).
4. I will log in to the Domain Controller (Remote Desktop) and verify that Client-1 shows up in Active Directory Users and Computers (ADUC) inside the "Computers" container on the root of the domain.
5. I will create a new Organizational Unit (OU) named "_CLIENTS" and drag Client-1 into there (this step is not really necessary, just for organizational purposes).

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/473ad7d4-5708-493c-8513-838f815464f1)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/fe787b86-94d7-4915-8022-d9b29e26cfdd)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/0769de44-b812-4859-a33a-0081ae4989c3)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/a3c1323f-61de-4cdd-9856-6f6ce83dac5a)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/03526f33-535f-4a09-a0ba-98dbfea81808)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/cdbde3ec-ec46-4f65-9bee-a931493eefb1)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/db0cc014-fd67-41d6-b286-52b3ff017513)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/9049cec6-72ff-4347-b643-eadc1ea6e801)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/edc26c7e-3652-4083-93a7-17d220c6fd5f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/53c1521e-4a60-4cf4-b86a-33eb3cc847a6)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/949a16b3-9e66-4c2a-8379-e60c3d9e294a)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/7c89c971-f5d9-40f7-b610-c1aa44deeb62)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/9ad9c317-1447-4146-833e-f95a9567b125)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/686c3b5d-459b-44fc-afe6-e57ca1c99a9c)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8e80bd6d-a91a-407c-97cc-eaac01f9dc19)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/ed10ba0c-b2fd-446e-b685-0cfca4f14cc4)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/13817ce3-db93-4cd4-85da-120bbb9f2f0f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8f138bf7-4b84-4ad8-8acf-691b2e2c7e95)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/e67efac5-df7f-432e-bece-25e68343c84c)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/894bd058-dea1-422b-b9fc-0e4d7a5451d8)


# 4. Task 4. To set up Remote Desktop for non-administrative users on Client-1, I'll follow these steps:

1. I'll log into Client-1 as mydomain.com\jane_admin.
2. I'll open System Properties.
3. I'll click on "Remote Desktop".
4. I'll allow "domain users" access to remote desktop.
5. Now, I can log into Client-1 as a normal, non-administrative user.


![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2da45718-38f2-48db-88d4-d958983126c4)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/d0d818f4-431f-4478-b8da-5020e8e6f860)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/33f851fb-c98d-4bad-9d56-e6b20610056f)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/9fb09b1c-ae2f-4da7-80fb-4877afa53fee)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f002f792-823c-4e1b-ab7e-4c46b7524e2e)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/e80b688c-9679-4f9f-9641-36c61fabee5b)

# 5. Task 5. To create a bunch of additional users and attempt to log into Client-1 with one of the users, I'll follow these steps:

1. Log in to DC-1 as jane_admin.
2. Open PowerShell_ISE as an administrator.
3. Create a new file and paste the contents of the script into it from the provided GitHub link.
4. Run the script and observe the accounts being created.
5. When finished, open ADUC and observe the accounts in the appropriate OU.
6. Attempt to log into Client-1 with one of the accounts (take note of the password in the script).

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/fa7998d9-883a-4a8b-90c3-52142715267c)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/479b664f-87c5-48f9-b5ef-f6790015ff2b)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/ef52fae9-206b-4a17-bb95-7475c481d2b0)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/7896c1e0-2f45-4066-baf6-f11bf05429b2)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/98317b31-7aa8-475d-bc36-444a16293b66)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2aaafb10-31d3-4ce4-88b4-59f27644764d)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/02005f3a-c625-438c-af03-b267e0f0038d)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/0761005f-2dd2-40c4-9365-2986f6531443)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/94c7f9ba-767b-4bd6-9aa1-f16d40be6038)

I'll re-run PowerShell as an administrator and then re-run the script to create the additional users.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/b73b22d3-e1b3-44e6-ab02-55a938772ffc)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/fd7fe7ce-b3d3-476c-a6c3-0b50736493b7)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/57b3365f-6d8c-40e0-a75f-13303e0b6947)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/cca2c2c6-6465-4654-ae11-bbc77e769a54)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/ee57d946-bc75-4d5c-ae6a-37c9fac0aab1)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/6da97252-9d5a-4e83-a5c9-c4cec10553eb)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f93482b4-e378-45bb-a1e1-8f120457338e)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2f0b8c2a-c0cd-40df-94f1-da9089330d0a)

I'll log out of Client-1 as jane_admin and then use one of the newly created users to verify the login.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/8d96a9e1-55b7-4a03-83a6-3742e3fe9093)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/e5fe94db-a8ed-4cff-afba-2a4500443f2b)

To log in, I will use the password that was used for the newly created user in the script.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/2ef362be-33e4-4348-b34c-ae78a770587a)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/d46cde75-9dfa-4bff-88f2-1a6fba567840)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/6483ddbc-9899-480d-9df1-bc4be7a5d327)

To unlock an account that has been locked due to too many incorrect password attempts, I can use the Active Directory Users and Computers (ADUC) console on the Domain Controller. Here's how I can do it:

1. I'll log in to the Domain Controller as an administrator.
2. I'll open Active Directory Users and Computers.
3. In the console tree, I'll navigate to the domain and then to "Users".
4. I'll right-click on the locked user account and select "Properties".
5. In the Properties dialog box, I'll go to the "Account" tab.
6. I'll check the box next to "Unlock account".
7. Finally, I'll click "OK" to unlock the account.

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/f824b7ad-0c78-44ff-931e-758e8d6ccdbd)

![image](https://github.com/iahalkhatib/Configure-AD-Azure/assets/170050432/c08cbca0-fe82-4c12-8bbe-ecddc9c4c91c)

Here is an example of how to reset a password for a user account in Active Directory
