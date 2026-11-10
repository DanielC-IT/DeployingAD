# Deploying Active Directory Domain Services (AD DS)

<img width="640" height="400" alt="image" src="https://github.com/user-attachments/assets/b055026a-fa5a-4da4-947f-d579d7d251fb" />

## Project Overview

In this project, I deployed and configured **Active Directory Domain Services (AD DS)** in a Windows Server environment using virtual machines (VMs).

The objective was to establish a Windows domain, promote a server to a Domain Controller, create Organizational Units (OUs), configure administrative accounts, and join a Windows client machine to the domain.

This lab provided hands-on experience with essential Windows Server administration tasks and foundational Active Directory management.

## Technologies & Tools Used

- Windows Server
- Windows Client
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Remote Desktop Connection (RDP)
- Server Manager
- Virtual Machines (VMs)

## Lab Environment

| Machine | Role |
|---|---|
| DC-1 | Domain Controller |
| CLIENT-1 | Domain-joined client workstation |

**Domain Name:** `mydomain.com`

---

## Project Walkthrough

### 1. Install Active Directory Domain Services

1. Log into the `DC-1` virtual machine using Remote Desktop Connection (RDP).
2. Open **Server Manager**.
3. Select **Add Roles and Features**.
4. Proceed through the installation wizard until you reach **Server Selection.**
5. Confirm that `DC-1` is selected.
6. Under Server Roles, select **Active Directory Domain Services**.
7. Continue through the wizard.
8. Under Confirmation, select **Restart the destination server automatically if required**.
9. Click Install and allow the installation to complete.

<img width="1905" height="1020" alt="1" src="https://github.com/user-attachments/assets/db0f36ba-05c9-4e4f-8c96-a59775ff97fb" />

<img width="784" height="559" alt="2" src="https://github.com/user-attachments/assets/fe5fddda-4517-414d-881b-4d3833863c14" />

<img width="785" height="561" alt="3" src="https://github.com/user-attachments/assets/84fc014e-a86d-4dfd-87f3-d9a9a72c518b" />

<img width="781" height="557" alt="5" src="https://github.com/user-attachments/assets/53aaaf44-4625-4d9f-9d26-19293e52572c" />

### 2. Promote DC-1 to a Domain Controller

1. Once AD DS installation is complete, return to Server Manager.
2. Click the notification flag in the upper-right corner.
3. Select **Promote this server to a domain controller**.
4. Under Deployment Configuration, select **Add a new forest**.
5. Enter the root domain name:

   `mydomain.com`

6. Proceed to Domain Controller Options.
7. Configure the Directory Services Restore Mode (DSRM) password.
8. Continue through the wizard until Prerequisites Check.
9. Click Install.

After installation, `DC-1` will automatically restart.

<img width="1903" height="1019" alt="6" src="https://github.com/user-attachments/assets/5be970f9-88a2-4019-acc3-9db23874c67e" />

<img width="758" height="562" alt="7" src="https://github.com/user-attachments/assets/3d2efdfb-bfba-4c46-9d26-27627e0fdfaa" />

<img width="759" height="557" alt="8" src="https://github.com/user-attachments/assets/c6160ddc-a25c-451a-81e0-04c0d720d62f" />

<img width="758" height="556" alt="9" src="https://github.com/user-attachments/assets/893b4f6c-049d-406f-b4a8-d904c138b278" />


### 3. Log Into the New Domain

After the server restarts, reconnect through Remote Desktop Connection using the domain credentials:

`mydomain.com\labuser`

<img width="455" height="578" alt="10" src="https://github.com/user-attachments/assets/33504fd5-f15b-462c-b886-503d90f59d75" />

### 4. Create Organizational Units (OUs)

1. Click on the Start button
2. Navigate to the **Windows Administrative Tools folder** and click **Active Directory Users and Computers**
3. Expand the `mydomain.com` domain.
4. Right-click the domain and select **New > Organizational Unit**.
5. Create the following OUs:

   - `_EMPLOYEES`
   - `_ADMINS`

These Organizational Units provide a structured way to organize employee and administrative accounts within Active Directory.

<img width="644" height="415" alt="11" src="https://github.com/user-attachments/assets/58f123a1-3b50-4020-8598-952ebc786bad" />

<img width="753" height="529" alt="12" src="https://github.com/user-attachments/assets/fb4b6490-01bb-43ad-8e7a-28a3ce921f1e" />


### 5. Create an Administrative User

1. Right-click the `_ADMINS` OU.
2. Select **New > User**.
3. Enter the following account information:

| Field | Value |
|---|---|
| First Name | Jane |
| Last Name | Doe |
| User Logon Name | jane_admin |

4. Create a password for the account.
5. Complete the user creation wizard.

<img width="435" height="378" alt="13" src="https://github.com/user-attachments/assets/9fb97985-16c4-45d0-89f3-fd49b45f3244" />

<img width="435" height="377" alt="14" src="https://github.com/user-attachments/assets/a455f46c-1656-424d-814e-1f9d6786d75b" />

<img width="434" height="375" alt="15" src="https://github.com/user-attachments/assets/c4fa043b-f93e-4fd3-afb8-cb29496d2fae" />

<img width="751" height="527" alt="16" src="https://github.com/user-attachments/assets/d4291d78-8d84-4820-b77b-682a28219f4a" />


### 6. Assign Domain Administrator Permissions

To grant the newly created account administrative privileges:

1. Right-click the `jane_admin` account.
2. Select **Properties**.
3. Navigate to the **Member Of** tab.
4. Click Add.
5. Enter `Domain Admins`.
6. Click **Check Names** to validate the group.
7. Click OK and Apply.

The `jane_admin` account is now a member of the Domain Admins security group.

**Security Note:** Domain Admin privileges provide extensive control over the domain. Administrative accounts should only be used when elevated permissions are required.

<img width="408" height="535" alt="17" src="https://github.com/user-attachments/assets/6fd65d7e-d23a-4476-922f-e91cf1aa4fee" />

<img width="452" height="251" alt="18" src="https://github.com/user-attachments/assets/310a836b-bea9-44f3-90d4-2463a55809ef" />

### 7. Verify Administrative Access

1. Log out of the current DC-1 session.
2. Reconnect using the newly created administrative account:

   `mydomain.com\jane_admin`

3. Verify that the account can successfully authenticate to the domain controller.

<img width="455" height="580" alt="19" src="https://github.com/user-attachments/assets/77208f5f-8976-4383-b0d1-037a9d59d858" />


### 8. Join CLIENT-1 to the Domain

1. Log into the `CLIENT-1` virtual machine using the original local lab account.
2. Right-click the Start button and select **System**.
3. Navigate to the option to change the computer's domain or workgroup.
4. Click **Change**.
5. Under Member of, select **Domain**.
6. Enter:

   `mydomain.com`

7. Provide authorized domain credentials when prompted.
8. Confirm the successful domain join when the following message appears:

   **Welcome to the mydomain.com domain.**

9. Restart CLIENT-1 to complete the domain join process.

<img width="1197" height="861" alt="20" src="https://github.com/user-attachments/assets/b9ca9fe1-ece8-4423-8c1f-1e4aabf04179" />

<img width="412" height="468" alt="21" src="https://github.com/user-attachments/assets/a1f95713-f24b-41c5-803b-8f408bfab847" />

<img width="320" height="391" alt="22" src="https://github.com/user-attachments/assets/85bec76b-7f0e-4c0f-80cd-d1451e34ee60" />

<img width="298" height="149" alt="23" src="https://github.com/user-attachments/assets/cb9900c8-0c88-4f8a-8d70-9c007de90e43" />


### 9. Verify CLIENT-1 in Active Directory

1. Return to `DC-1` and log in using `jane_admin`.
2. Open **Active Directory Users and Computers**.
3. Expand the `mydomain.com` domain.
4. Select the default **Computers** container.
5. Verify that `CLIENT-1` appears in the list.

This confirms that the client workstation successfully joined the domain.

<img width="750" height="497" alt="24" src="https://github.com/user-attachments/assets/1b0fb450-3e00-448f-98a6-5243698cf4bc" />


### 10. Create a CLIENTS Organizational Unit

To further organize domain-joined workstations:

1. Right-click the `mydomain.com` domain.
2. Select **New > Organizational Unit**.
3. Name the new OU:

   `_CLIENTS`

4. Locate `CLIENT-1` inside the default Computers container.
5. Right-click the computer account and select **Move**.
6. Select the `_CLIENTS` Organizational Unit.
7. Confirm the move.

<img width="753" height="488" alt="26" src="https://github.com/user-attachments/assets/2bd1fcbb-f7d7-4ea4-b76f-e7b4219311bd" />


---

## Final Active Directory Structure

After completing the lab, the domain should contain the following organizational structure:

```text
mydomain.com
│
├── _EMPLOYEES
│
├── _ADMINS
│   └── jane_admin
│       └── Member of: Domain Admins
│
├── _CLIENTS
│   └── CLIENT-1
│
└── Computers
    └── (Default container)
