[
](https://camo.githubusercontent.com/e38ed1b3f8f0cff62ee2117afc871396adea4a1266dbebb482b5080db92184af/68747470733a2f2f692e696d6775722e636f6d2f705535413538532e706e67)![image](https://github.com/user-attachments/assets/087b8612-4e6a-4764-883d-92a61afe5941)

On-premises Active Directory Deployed in the Cloud (Azure)

Project Summary:
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

Environments and Technologies Used:
- Microsoft Azure (Virtual Machines/Compute)
- Active Directory Domain Services
- Remote Desktop
-Powershell

Operating Systems Used:
- Windows Server 
- Windows 10

High-Level Deployment and Configuration Steps
-Create 2 Virtual Machines in Azure (Domain Control-1(DC-1), Client-1) 
-Install Active Directory on (DC-1)
-Join Client-1 to Main Domain
-Create a list of User Accounts

Deployment and Configuration Steps

![image](https://github.com/user-attachments/assets/51c37a5d-6ffc-4c45-9668-1905584b8442)

Create your first VM. Name it DC-1 for Domain Controller and use Windows server (use 2 VCPUs at least) Click Create.

-Create another VM Naming it Client-1 making sure it is within the same network as your first VM using Windows 10. Click Create.
-Set the Domain Controler(DC-1)VM IP address to static. Go to Networking -> Go to Network Interface -> IP Configurations -> from dynamic to static
-Ensure both VMs are connected.
-Go to Client-1 on the remote desktop and log in. Run command prompt and Ping DC-1 private IP address
- Log in to DC-1 with remote desktop. Click start and search firewall. Open Windows firewall with advanced settings
- -> inbound rules. Sort protocol Section -> Look for icmpv4 and allow them by clicking enable rule
- ping should work on the Client-1 VM.

[
](https://camo.githubusercontent.com/f9cd2369070e73d169dcb32e1b6e15261e8a73b07237df08266ac54bcb971b3e/68747470733a2f2f692e696d6775722e636f6d2f523332586b75372e706e67)![image](https://github.com/user-attachments/assets/6012e0be-664e-4728-a248-414c2017cfce)

Installing Active Directory on DC-1
- Open DC-1 VM remote Desktop. Open Service Manager -> go to add roles and features   -> Click next -> Click next -> next -> click Active Directory Domain Services-> click next all the way to install then click install.
- In the service manager, Click the yellow flag at the top of the page so you can set as domain controler and finish installing acitive directory.
- Name the domain server mydomain.com -> set password and remember it->click next all the way through. -Computer will shut down and you will have to reconnect to the DC-1 VM. Log in with the domain controler name. mydomain.com/"username" if you cant log in the normal way
- Click start search active directory. Right click mydomain.com -> new-> organizational unit call it _EMPLOYEES press ok. Do samething making a folder for _ADMIN. Refresh the directory.
- Go to _ADMIN. Create new user title it an admin account make a password uncheck the change password setting. Click ok.
- Right click the user account just made -> add domain -> check names -> domain admins group -> apply -> ok
- Log out of DC_1 and log back in under the admin account that you just made.
[
](https://camo.githubusercontent.com/0619a1ec9fd1e880a19e3de33e8268aeb6792cea66b7b1112711462a3786cc65/68747470733a2f2f692e696d6775722e636f6d2f434279565230302e706e67)![image](https://github.com/user-attachments/assets/9f713766-4693-423c-8532-fc6fade7c62f)

- In the Azure portal go to virtual machines and click DC-1 -> networking -> you will see NIC Private IP copy it.
- Still in the Azure portal go back to you VM's and click Client-1 -> networking -> Click the network interface ID. -> DNS Servers on the side panel -> Custom and paste the copied private ip address from DC-1 into the box. Click save.
- Go back to the vm's page in Azure and click Client-1 -> Restart
- Log into Client-1 for remote desktop as your local admin account(the one before you mad the new admin account)
- Open Command Prompt onCLient-1 remote desktop-> type in: ipconfig/all should see DC-1 ip address
- On Client-1 VM ->Right click Start menu -> system -> rename this pc advanced-> click change -> click domain and type in domain.com.-> click ok, a new window should pop up for Computer Name/ Domain Changes. Use the domain admin log in you created the second time (not your local admin account).
- Computer will need to restart to join the domain.

  [
](https://camo.githubusercontent.com/0664d2236792b7435cfe9375789833bc2d3bb97043b39d94eb6bc472ec256e79/68747470733a2f2f7777772e746f702d70617373776f72642e636f6d2f626c6f672f77702d636f6e74656e742f75706c6f6164732f323031392f30322f72656d6f74652d6465736b746f702d73657474696e67732e706e67)![image](https://github.com/user-attachments/assets/3037ff0d-6435-41f6-919d-bebb1f80cb70)

- You should be able to log in on the Client-1 computer as the Admin account you created for domains (not local admin account from first step).
- Right click start menu-> system-> remote desktop-> click select users that can remotely access this PC. ->Click add type in domain users->check names-> click ok
- on DC-1 Start menu type in Active Directory click users and computers one.
- click user folder and click domain users anyone in that folder should be able to log into either computer.
- Create a bunch of additional users and attempt to log into client-1 with one of the users Login to DC-1 as the Domain Controler Admin account Open PowerShell_ise as an administrator Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) Run the script and observe the accounts being created When finished, open ADUC and observe the accounts in the appropriate OU attempt to log into Client-1 with one of the accounts (take note of the password in the script)
