---
title: "Active Directory Domain Services Task"
datePublished: Sat Apr 26 2025 15:33:55 GMT+0000 (Coordinated Universal Time)
cuid: cm9ydruif000j09lec6kc919j
slug: active-directory-domain-services-task
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745681607670/e36ee114-3dba-499e-9d78-9407a195affc.png

---

Active Directory Domain Services works same as Active Directory in Micorsoft Azure. It is used for managing multiple users under one management hub. This task ensures that you are able to create new users in Active Directory and manage them. Also you are able to connect you machine to this directory.

## Task

**Create a Windows Server VM in azure and make it Domain Controller. Add 3 Organization Unit (Production, Engineer, Sales) and inside each OU create 1 User. Create a new Windows Vm and connect this VM to the Domain Controller and sync the users.**

## Steps

### Setting up Domain Controller

* Make your Windows Server a Domain Controller so that it become a Central Management Unit.
    
* Create a Virtual Machine in azure. Image should be Windows datacenter 2019 or 2022. This virtual machine will be our [Domain Controller](https://www.techtarget.com/searchwindowsserver/definition/domain-controller).
    
* Connect to your instance using RDP.
    
* Open Server Manager from start menu in your virtual machine after connecting to your VM.
    
* Now click on ‘**Add Roles and Features**’ tab. In add roles tab add **DNS** and A**ctive Directory Domain Services**.
    
* Now on top left corner , click on Flag Icon &lt; **Promote this server to Domain Controller.**
    
* A wizard will open , in **Deployment Configuration** choose ‘**add a new forest**’ and give your domain a name. Eg. - **spektra.com**
    
* In next step give your **Domain Controller** a password.
    
* Skip all other Config. Tabs and press **Next** and in last one click **Install**.
    
* Install these features and your virtual machine will restart.
    
* Open your Virtual Machine , now virtual machine is now configured as Domain Controller.
    

### Configuring Domain Controller and adding Users

* We are done with making our Virtual Machine a Domain Controller. Now we have to make Organizaion Unit and add Users in that Organization Unit and give them certain permission so that they can access theri account using RDP.
    
* Open your instance and go to **Server Manger**. On top left click on **Tools** and click on A**ctive Directory Users and Computers**.
    
* Now new tab will open and here you can see you domain name , which was **spektra.com**.
    
* Right Click on your domain name and click **New** &gt; **Organization Unit** . Give name ‘engineer’ and click **Ok**.
    
* Your OU will be created , right click on OU **engineer** &gt; **New** &gt; **User**. Provide **First and Last Name** of the user , **eg. - Akshat Verma**. In **Logon** name give ‘**engineer**‘. This will be your username for this user. Click Next and uncheck **User must change password on next logon** and give it a password. New user will be created.
    
* Now same way you created User , create a **Group** as well in that **engineer** OU.
    
* Give group name **Eng group** and press **Ok**.
    
* Double click on Group **Eng group &gt; Members &gt; Add &gt;Advanced &gt; Find Now**.
    
* Select your user here. Eg.- **Akshat**. Press **Apply** and **Ok**.
    
* In **Members** tab you user will be added now.
    
* Now go to **Member of &gt; Add &gt; Advanced &gt; Find Now.**
    
* Search for **Domain Users** and **Remote Desktop Users** and add them and **apply / ok** .
    
* Your permission would be added on **Members of** tab.
    
* Successfully added Users and assigned required permission to Users.
    

### Setting permissions for RDP access

* Created Users and assinged permissions. Now we have to add those permission to our Local Policy so that the Users having those permission can Login through RDP. By default only Administrator has the persmission of logging through RDP. Now we are changing that and adding some moe permission.
    
* Open **Server Manager &gt; Tools &gt; Local Security Policy**.
    
* Under **Secutity Settings &gt; Local Policies &gt; User Rights Assignments &gt; Allow Log on through Remote Desktop Services.**
    
* Double click and then **Add Users and Groups &gt; Advanced &gt; Find Now.**
    
* Add **Domain Users** and **Remote Desktop Users**.
    
* **Apply and Ok**.
    
* Now you will be able to login to new users using RDP.
    

### Connecting Client Vm to Domain Controller

* The Domain Controller part is over now we have to make a new Client VM part of our Domain Controller. This new Vm will have the access to the Users we created and from this Vm we can login to our Users which we created in Organization Unit. We have to connect our Client Vm to Domain Controller and sync Users to it.
    
* Create a new **Virtual Machine** (Windows 10 Pro) in same **Region** , same **Vnet** , same **Subnet** connect to it through **RDP**.
    
* You need to change your **DNS settings** so that you can connect to your **Domain Controller**.
    
* Open **Control Panel &gt; Network and Internet &gt; Network and Sharing Center &gt; Change Adapter Settings.**
    
* Right click on **Ethernet Adapter** and go to **Properties**.
    
* Open **Internet Protocol Version 4 &gt; Use the following DNS Address.**
    
* Add **Private IP** of your **Domain Controller** (Windows Server Vm) and 8.8.8.8 in alternative DNS.
    
* Then press **Ok**. Restart your VM from azure portal and connect again.
    
* Now you need to connect your **Windows 10** Vm to **Domain Controller** , so that this Vm can be a part of that **Domain**.
    
* Navigate to **Settings &gt; System &gt; About &gt; Advanced System Settings.**
    
* Under **Computer Name** tab, click on **Change**.
    
* New Config Tab will open. In **Member of** select **Domain**.
    
* Add Domain name here. Eg.- **spektra.com**
    
* Press ok. It will ask for Username and Password. Enter credentials of Windows Server.
    
* Click Ok and Restart your Vm form Azure Portal.
    
* Again Navigate to **Settings &gt; System &gt; About &gt; Advanced System Settings.**
    
* Under **Computer Name** tab you can see your **Domain** will be changed to spektra.com.
    
* Now you are done with connecting this Vm to Domain Controller. Now you need add all users you created on Domain Controller so that you can access them using this VM credentials.
    
* Navigate to Remote tab &gt; Select Users &gt; Add &gt; Advanced .
    
* It will ask for credentials. Now in **Username** you have to add your Server VMs (Domain Controller) username ‘@‘ and your Domain Name. Eg.- akshat\_verma@spektra.com. Here akshat\_verma is my username and spektra.com is my Domain Name. After that provide password of the Server VM.
    
* New tab will open. Click on **Find Now** and select all Users you create on your Domain Controller.
    
* Press ok and then exit.
    
* You successfully added your users to this Client VM.
    

### Testing

* You are done now. You just need to test your configurations now.
    
* Start Windows Client Vm and Windows Server Vm.
    
* Open RDP. Enter Public Ip of your Windows 10 Pro Client Vm.
    
* Click on Connect and you have to enter new username and password.
    
* In **Username**, ‘**Domain-Name\\logon-username**’ . Eg.- **spektra\\engineer**.
    
* Here **spektra** is my Domain Name and **engineer** is my logon username which i gave when i added my User in an Organization Unit.
    
* For **Password**, provide the password that you gave while creating this user.
    
* Click **connect**.
    
* You should able to login through your new User through Client Vm.
    
* And your task is completed.