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

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setting up the domain controller by doing that is creating a resource group and a virtual network. When that is done you can create your domain controller (DC) virtual machine (VM) and make sure that you are using **Windows server 2022** for your DC VM. After that you can start doing Client VM using **Windows 10 Pro** and make sure that vnet is the same as the DC VM. While the vm is creating you can change the private ip address from dynamic to static that prevent the ip address from changing. Now what you want to do is login to DC VM. Once you are logged in you want disable the firewall (this is just for testing the connection). The way you access that is open the app on DC VM named run and on the search bar type "_wf.msc_" than you got firewall properties and turn them off. After doing that you can in client's setting and set it to DC's private ip address. Restart client vm and login so you can ping DC's private ip address to make you that it worked. To conclude this step in powershell want to type "ipconfig/all" this will show the DNS (DC's Private Ip address).
  
- Step 2: Installing the active directory domain services to start doing that is go back to DC VM and install it also deploying it. The only server selection should be is the DC VM's private ip address.  In order to begin the installing it you want to go to server manager -> add roles and features and there will be some options choose **active directory domain services** this will make that VM to be an actual domain controller. Next is to setup a new forest such as mydomain.com or anything else (remember the information used). When you have done that you go to a flag with a warning sign that would be located next to the flag so you can promote DC VM. The next screen is the where you going add a new forest (new domain root) and insert the information for the forest such as mydomain.com. When you finish you will see the VM restarting that means you have done it correctly. That part is log back but use mydomain.com\Labuser or whatever information you have inserted. Once you get log back in go to Active Directory Users and Computers (ADUC) and created some Organization Unit (OU) that would be named as **Employees** and **Admins**. When you create the Organization units/folders you want to create under mydomain.com section. The OUs are created you can add  and employee named Jane Doe whenever you get to the password you use the same password that you used to login to the VM that could make the job a little bit easier for yourself (this creation will be under admins the folder you just created). You create Jane but she is not an admin yet so have to make the user an admin right-click on Jane Doe-> Properties-> Member Of-> Under Member Of click Add-> Under Add a box named object names type "Domain Admins"-> click check names to see if the group exist-> click OK that how you can make someone an admin in the system. Next log off and log back in as Jane Doe. Now that you are logged back in as jane you want to go to Client VM and join it to the domain and it will restart the VM. While that client VM restart it go back to DC VM and check client VM shows up in ADUC and make another OU and call it Clients and drag client VM to that folder.
   
- Step 3: To begin this you need to login as Jane admin on the Client VM. Once logged in as Jane Doe on client vm you want to go to system -> remote desktop -> click on "select users that can remotely access this PC" that would be located  under Users Account -> click on the add box after that you will see a screen that will say "Select user or groups" and type on the big box that where you want to put object name such as Domain User. When you are done checking the name you can go back to DC VM and open powershell_ise as an administrator. Once the app is running you can create a new file and copy/paste a script that would be provided. Next you want to run the script and observe on the ADUC how is creating employees and pick one of the employees try to login on client vm.
  
- Step 4: To get started on this step is by picking an account that you created previously and put the wrong password 10 or 11 times and go back to dc vm. You will be to see that it did not work so you need to configure the Group Policy to Lockout when you have done . Now you will have to force the update policy in client vm and open the command line as an admin (command prompt) to type in gpupdate /force to verify that it worked type in gpresult / r. You want to go to client vm and put the wrong password 5 times now you will see that account is locked. Since that account is locked you want to go to dc vm and unlock that account so you can login with the correct credentials. Now that you are able to login you reset the password for the account that you used.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/UVkjEGb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  On the image above it shows the dns/ dc vm ip address that means that is linked to the domain control and you can do any reset or unlocked to that machine. What I did get to this page is configuring the client vm through azure portal like changing the dns to the private ip address. Once I did all that I typed in the powershell "ipconfig /all" just how it shows in the image.
</p>
<br />

<p>
<img src="https://i.imgur.com/9c3IMyJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
<img src="https://i.imgur.com/tEkqu0k.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  The image that shows powershell ise you can see that when paste the script that was provided and it will randomly create 10,000 employee but you can stop it when you feel you have enough users. Before you copy the script make sure you a made a new file or else it will show you like a normal powershell. The image of the ADUC you can create a new OU file just like how I have it above. Another use for this app is inserting such as admins, employee and clients also you can insert people's roles on each file that can make  things a lot easier to access.
</p>
<br />

<p>
<img src="https://i.imgur.com/6GcLnVk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This image show the confirmation of the group policy that has been updated. The way to update the group policy follow the step 4 instructions. The screen that shows above it also shows the last time the group policy was updated.
</p>
<br />
