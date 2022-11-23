<p align="center">
<img src="https://i.imgur.com/VnVoqzC.png" height="20%" width="20%" alt="Group Policy Photo"/>
</p>

<h1>Software Deployment Using Group Policy</h1>
This tutorial explains how to use group policy for software deployment.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Group Policy Management

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Steps</h2>

- Create a network share folder.
- Set folder permissions and add MSI files to the network share folder.
- Use Group Policy Management tools to create a GPO.
- Install software onto one of the user's accounts via the client virtual machine. 

<h2>Software Deployment</h2>

<p>
For this tutorial, Chrome will be deployed to users using Group Policy, and there will be two virtual machines utilized. The first virtual machine (DC-1) will run Windows Server 2022 as its base operating system. The second virtual machine (Client-1) will run Windows 10 as its base operating system. Both virtual machines will be under the same resource group and virtual network (Vnet). This can be confirmed by viewing the topology available under Network Watcher in Azure. On the first virtual machine, a domain controller will be set up under the name "mydomain.com," and then the second virtual machine (Client-1) will be added to the domain "mydomain.com." The set-up for this is shown here:<h3><a href="https://github.com/maya-boro/configure-ad">Configuring Active Directory within Azure VMs</a></h3>
</p>

<p>
First, we will log in to the domain controller and create a network share folder by going to File Explorer, selecting "This PC" from the left-hand side menu, and then selecting the C: drive. Once inside the C: drive, right-click and select "New Folder," and enter the folder name "IT." After creating the "IT" folder, click on the "IT" folder and create another folder named "Software." Right-click on the "Software" folder and select "Properties."
</p>
<p>
<img src="https://i.imgur.com/lOCR3FX.png" height="70%" width="70%" alt="Deploy Part One"/>
</p>
<br />

<p>
Now, from the pop-up window, select the "Sharing" tab followed by the "Advanced Sharing" button. Once in the advanced settings, select the "Share this folder" button. Then click on the "Permissions" button within the second pop-up window and remove the group "Everyone."
</p>
<p>
<img src="https://i.imgur.com/G8AyLuC.png" height="70%" width="70%" alt="Deploy Part Two"/>
</p>
<br />

<p>
After, add domain users and domain computers and change the permissions to read. If necessary, additional security groups may be created to restrict the permissions to particular individuals and machines. 
</p>
<p>
<img src="https://i.imgur.com/8bk4Hiu.png" height="70%" width="70%" alt=" Deploy Part Three"/>
</p>
<br />

<p>
Once these changes have been made under the "Sharing" tab, the permissions will also need to be changed under the "Security" tab within the same "Properties" window. Once on the "Security" tab, if listed, remove the group "Everyone" and then add domain users and domain computers. The permissions for these two groups should be set to read and execute.
</p>
<p>
<img src="https://i.imgur.com/zn9g0sn.png" height="70%" width="70%" alt="Deploy Part Four"/>
</p>
<br />

<p>
Now we will copy the Chrome Installer MSI file into the software folder. After, we will check to see if the "Software" folder is available for users by logging into a user account on the Client-1 virtual machine. Once logged into the user account, go to File Explorer, enter \\dc-1 in the top bar, and then select the network share folder "Software," which should be accessible and have the Chrome Installer MSI file listed.
</p>
<p>
<img src="https://i.imgur.com/U309PG0.png" height="70%" width="70%" alt="Deploy Part Five"/>
</p>
<br />

<p>
Go back to the domain controller and access Group Policy Management from the "Tools" section available on the server manager dashboard. Once in the Group Policy Management window, go to the "_Employee" organizational unit, right-click on the folder, select "Create a GPO in this domain and link it here," and name the GPO "Employees-Chrome Install," then right-click on the created GPO and select "Edit."
</p>
<p>
<img src="https://i.imgur.com/KSN19AM.png" height="70%" width="70%" alt="Deploy Part Six"/>
</p>
<br />

<p>
Now, within the Group Policy Management Editor window, go to "User Configuration," find the "Software Settings" folder, and look for "Software Installation." Then right-click on "Software installation," select "New," and click on "Package." From the pop-up window, enter \\dc-1\Software in the top bar, and then select the Chrome Installer MSI file listed and click "Open," and the "Deploy Software" window will appear. Select the "Published" option.
</p>
<p>
<img src="https://i.imgur.com/l9OeQEQ.png" height="70%" width="70%" alt="Deploy Part Seven"/>
</p>
<br />

<p>
The GPO setup is now complete. 
</p>
<p>
<img src="https://i.imgur.com/cQSHm65.png" height="70%" width="70%" alt="Group Policy Final Setting Chrome"/>
</p>
<br />

<p>
Lastly, log into a user account using the Client-1 virtual machine. Once logged in, click the start menu button, search for "Control Panel," and select "Open." From the Control Panel window, look for "Programs" and select "Get programs." Then select "Google Chrome" from the list and click "Install." Allow Chrome to install.
</p>
<p>
<img src="https://i.imgur.com/Y8xW4YJ.png" height="70%" width="70%" alt="Deploy Part Eight"/>
</p>
<br />

<p>
The Google Chrome browser is now successfully installed.
<p>
<img src="https://i.imgur.com/MTbj8Ov.png" height="70%" width="70%" alt="Successful Chrome Install"/>
</p>
<br />
