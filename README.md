<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" height="40%" width="70%"alt="Microsoft Active Directory Logo"/>
</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 

<h2>Deployment and Configuration Steps</h2>

<ol>
  <li>Create necessary resources in the environment</li>
  <li>Verify connectivity between the client machine and the Domain Controller</li>
  <li>Install and configure Active Directory</li>
  <li>Create administrative and standard user accounts in Active Directory</li>
  <li>Join Client-1 to the domain (e.g., myadproject.com)</li>
  <li>Configure Remote Desktop access for non-administrative users on Client-1</li>
  <li>Create additional user accounts and test login functionality on Client-1 with one of the new users</li>
</ol>

<h2>Deployment and Configuration Steps</h2>
<br />
<br />
<h3 align="center">Setup Resources in Azure</h3>
<br />
<p>
  <h2>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</h2>

<ol>
  <li>
    <strong>Launch VM Creation Wizard</strong>
    <ul>
      <li>Log in to your Azure portal or virtualization platform.</li>
      <li>Select the option to create a new Virtual Machine (VM).</li>
    </ul>
  </li>
  <li>
    <strong>Configure Basic Settings</strong>
    <ul>
      <li>Name the VM: <strong>DC-1</strong>.</li>
      <li>Choose the operating system: <strong>Windows Server 2022</strong>.</li>
      <li>Select the appropriate region, size, and subscription.</li>
    </ul>
  </li>
  <li>
    <strong>Network and Storage Settings</strong>
    <ul>
      <li>Attach the VM to a virtual network (VNet) that other machines will connect to.</li>
      <li>Configure storage for the OS disk and any additional data disks if required.</li>
    </ul>
  </li>
  <li>
    <strong>Authentication and Access</strong>
    <ul>
      <li>Set up an admin username and password for the VM.</li>
      <li>Enable ports such as <strong>RDP (3389)</strong> for remote access.</li>
    </ul>
  </li>
  <li>
    <strong>Review and Create</strong>
    <ul>
      <li>Review your settings, validate, and click <strong>Create</strong> to deploy the VM.</li>
    </ul>
  </li>
  <li>
    <strong>Post-Deployment Configuration</strong>
    <ul>
      <li>Once the VM is running, connect via <strong>Remote Desktop</strong>.</li>
      <li>Begin the setup process for Active Directory Domain Services to promote this VM as the domain controller.</li>
    </ul>
  </li>
</ol>

</p>
<p>
  <img src="https://i.imgur.com/gaAzjvb.png" height="75%" width="100%" alt="resource group"/>
  <img src="https://i.imgur.com/hubTfey.png" height="75%" width="100%" alt="vm ms server"/>
</p>

<h2>Create the Client VM (Windows 10) named “Client-1”</h2>

<p>Use the same Resource Group and VNet that was created in the previous step:</p>

<ol>
  <li>
    <strong>Launch VM Creation Wizard</strong>
    <ul>
      <li>Select the option to create a new Virtual Machine (VM).</li>
    </ul>
  </li>
  <li>
    <strong>Configure Basic Settings</strong>
    <ul>
      <li>Name the VM: <strong>Client-1</strong>.</li>
      <li>Choose the operating system: <strong>Windows 10</strong>.</li>
      <li>Select the same <strong>Resource Group</strong> and <strong>VNet</strong> used for "DC-1".</li>
    </ul>
  </li>
  <li>
    <strong>Network and Storage Settings</strong>
    <ul>
      <li>Ensure the VM is attached to the same virtual network (VNet) as "DC-1".</li>
      <li>Configure storage for the OS disk and any additional data disks if required.</li>
    </ul>
  </li>
  <li>
    <strong>Authentication and Access</strong>
    <ul>
      <li>Set up an admin username and password for the VM.</li>
      <li>Enable ports such as <strong>RDP (3389)</strong> for remote access.</li>
    </ul>
  </li>
  <li>
    <strong>Review and Create</strong>
    <ul>
      <li>Review your settings, validate, and click <strong>Create</strong> to deploy the VM.</li>
    </ul>
  </li>
  <li>
    <strong>Post-Deployment Configuration</strong>
    <ul>
      <li>Once the VM is running, connect via <strong>Remote Desktop</strong>.</li>
      <li>Verify network connectivity to the Domain Controller ("DC-1").</li>
    </ul>
  </li>
</ol>

<p>
  <img src="https://i.imgur.com/XyEmv8f.png" height="75%" width="100%" alt="vm windows"/>
</p>
<p>
  <h2>Set Domain Controller’s NIC Private IP Address to Static</h2>

<ol>
  <li>
    <strong>Access the Network Interface</strong>
    <ul>
      <li>Navigate to the <strong>Network Interface (NIC)</strong> associated with the Domain Controller ("DC-1").</li>
    </ul>
  </li>
  <li>
    <strong>Modify IP Configuration</strong>
    <ul>
      <li>In the NIC settings, select <strong>IP Configurations</strong>.</li>
      <li>Click on the existing IP address configuration to edit.</li>
    </ul>
  </li>
  <li>
    <strong>Set the IP Address to Static</strong>
    <ul>
      <li>Change the assignment from <strong>Dynamic</strong> to <strong>Static</strong>.</li>
      <li>Specify the desired private IP address. Ensure it is within the VNet’s address range and does not conflict with other devices.</li>
    </ul>
  </li>
  <li>
    <strong>Save Changes</strong>
    <ul>
      <li>Click <strong>Save</strong> to apply the changes.</li>
    </ul>
  </li>
  <li>
    <strong>Verify the Configuration</strong>
    <ul>
      <li>Check the NIC details to ensure the private IP address is now static.</li>
      <li>Ping the static IP address from another VM on the same network to confirm connectivity.</li>
    </ul>
  </li>
</ol>

</p>
<p>
  <img src="https://i.imgur.com/KHU9kC4.png" height="75%" width="100%" alt="static ip"/>
</p>

<p>
  Ensure that both VMs are in the same VNet. You can verify the network topology using 
  <strong>Network Watcher</strong>:
</p>
<ol>
  <li>
    <strong>Enable Network Watcher</strong>
    <ul>
      <li>Go to <strong>Network Watcher</strong> in the navigation menu and ensure it is enabled for your region.</li>
    </ul>
  </li>
  <li>
    <strong>Access Topology</strong>
    <ul>
      <li>In the <strong>Network Watcher</strong> menu, select <strong>Topology</strong>.</li>
      <li>Choose the subscription, resource group, and virtual network (VNet) containing your VMs.</li>
    </ul>
  </li>
  <li>
    <strong>Verify Connections</strong>
    <ul>
      <li>View the topology diagram to confirm that both VMs ("DC-1" and "Client-1") are connected to the same VNet.</li>
      <li>Ensure there are no connectivity issues or misconfigurations.</li>
    </ul>
  </li>
</ol>

![Screenshot 2024-12-30 023449](https://github.com/user-attachments/assets/81f07e41-3546-4e8b-944c-eff406c20058)
<h3 align="center">Ensure Connectivity between the client and Domain Controller</h3>
<br />
<p>
  Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping):
</p>
<p>
  <img src="https://i.imgur.com/bnPM9tX.png" height="75%" width="100%" alt="perpetual ping"/>
</p>
<p>
  Login to the Domain Controller and enable ICMPv4 in on the local windows firewall:
</p>
<p>
  <img src="https://i.imgur.com/ZpPyEkt.png" height="75%" width="100%" alt="enable ICMPv4"/>
</p>
<p>
  Check back at Client-1 to see the ping succeed:
</p>
<p>
  <img src="https://i.imgur.com/8o3OfjY.png" height="75%" width="100%" alt="ping success"/>
</p>
<br />
<br />
<h3 align="center">Install Active Directory</h3>
<br />
<p>
  Login to DC-1 and install Active Directory Domain Services:
</p>
<p>
  <img src="https://i.imgur.com/A1V9XJ5.png" height="75%" width="100%" alt="active directory install"/>
</p>
<p>
  Promote as a Domain Controller:
</p>
<p>
  <img src="https://i.imgur.com/zi15fw4.png" height="75%" width="100%" alt="domain controller promotion"/>
</p>
<p>
  Setup a new forest as myactivedirectory.com (can be anything, just remember what it is - I ultimately did set it up as myadproject.com which you'll see in the next pic):
</p>
<p>
  <img src="https://i.imgur.com/DCFUVrM.png" height="75%" width="100%" alt="set new forest"/>
</p>
<p>
  Restart and then log back into DC-1 as user: myadproject.com\labuser:
</p>
<br />
<br />
<h3 align="center">Create an Admin and Normal User Account in AD</h3>
<br />
<p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and another one called "_ADMINS":
</p>
<p>
  <img src="https://i.imgur.com/cYmv0r7.png" height="75%" width="100%" alt="organizational unit"/>
  <img src="https://i.imgur.com/v02CBPI.png" height="75%" width="100%" alt="organizational unit"/>
</p>
<p>
  Create a new employee named “Jane Doe” with the username of “jane_admin”:
</p>
<p>
  <img src="https://i.imgur.com/h546E6L.png" height="75%" width="100%" alt="admin creation"/>
</p>
<p>
  Add jane_admin to the “Domain Admins” Security Group:
</p>
<p>
  <img src="https://i.imgur.com/mnLwTgq.png" height="75%" width="100%" alt="security group"/>
</p>
<p>  
  Log out/close the Remote Desktop connection to DC-1 and log back in as “myadproject.com\jane_admin”. Use jane_admin as your admin account from now on:
</p>
<br />
<br />
<h3 align="center">Join Client-1 to your domain (myadproject.com)</h3>
<br />
<p>
  From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address:
</p>
<p>
  <img src="https://i.imgur.com/1KRsjI6.png" height="75%" width="100%" alt="client dns settings"/>
</p>
<p>
  From the Azure Portal, restart Client-1.
</p>
<p>
  Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart):
</p>
<p>
  <img src="https://i.imgur.com/50wszcP.png" height="75%" width="100%" alt="domain joining"/>
</p>
<p>
  Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
</p>
<p>
  Create a new OU named “_CLIENTS” and drag Client-1 into there:
</p>
<p>
  <img src="https://i.imgur.com/vB1n9m0.png" height="75%" width="100%" alt="active directory client verification"/>
</p>
<br />
<br />
<h3 align="center">Setup Remote Desktop for non-administrative users on Client-1</h3>
<br />
<p>
  Log into Client-1 as mydomain.com\jane_admin and open system properties.
</p>
<p>
  Click “Remote Desktop”.
</p>
<p>
  Allow “domain users” access to remote desktop.
</p>
<p>
  You can now log into Client-1 as a normal, non-administrative user now.
</p>
<p>
  Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab):
</p>
<p>
  <img src="https://i.imgur.com/8BfpT3s.png" height="75%" width="100%" alt="remote desktop setup"/>
</p>
<br />
<br />
<h3 align="center">Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>
<br />
<p>
  Login to DC-1 as jane_admin
</p>
<p>
  Open PowerShell_ise as an administrator.
</p> 
<p>  
  Create a new File and paste the contents of this script (https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) into it:
</p>
<p>
  <img src="https://i.imgur.com/0i8uApf.png" height="75%" width="100%" alt="create users script"/>
</p>
<p>
  Run the script and observe the accounts being created:
</p>
<p>
  <img src="https://i.imgur.com/6QOGzs6.png" height="75%" width="100%" alt="observe create users script"/>
</p>
<p>
  When finished, open ADUC and observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script):
</p>
<p>
  <img src="https://i.imgur.com/ZZCfiCp.png" height="75%" width="100%" alt="employee user accounts"/>
  <img src="https://i.imgur.com/7gBpNzN.png" height="75%" width="100%" alt="employee user selection"/>
  <img src="https://i.imgur.com/cqsddjn.png" height="75%" width="100%" alt="employee user login"/>
</p>
<br />
<br />
