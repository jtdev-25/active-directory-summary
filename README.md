<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>

<h1>🛠️ Active Directory Setup Guide</h1>

<p>A complete step-by-step guide to set up and manage <strong>Active Directory</strong> on Windows Server. Includes installation, configuration, user and group management, and domain policies.</p>

<h2>❓ Overview ❓</h2>
<p>Active Directory Data Storage(ADDS) is a directory service developed by Microsoft for the sole purpose of providing companies with the ability to manage user accounts; This guide walks you through building an AD domain as well as a user-based virtual machine, managing users, devices, and applying policies.</p>

<h2> ✅ Prerequisites</h2>
<ul>
  <li>Windows Server 2016/2019/2022</li>
  <li>Static IP configured</li>
  <li>Administrator access</li>
  <li>Updated OS & patches</li>
  <li>Internet for updates</li>
</ul>
<img width="460" alt="Screenshot 2025-04-14 at 3 03 17 PM" src="https://github.com/user-attachments/assets/86d08715-19a4-4254-81ab-4eb8f0c9b630" />
<img width="520" alt="Screenshot 2025-04-14 at 3 12 14 PM" src="https://github.com/user-attachments/assets/823e593f-44eb-4d92-b06d-69d81e7a9cc7" />

<h2> 🖥️ Step 1: Preparing Active Directory</h2>
<ol>
  <li>Create both VMs: client = windows 10, domain = windows 2022 pro ➡️ both connected to same VNet and RscGrp.</li>
    </li>Set domain private IPv4(static); Networking ➡️ Network Settings ➡️ Ipconfig selection ➡️ “Static”</li>
<li> AFTER CREATING VMs: Adjust firewall inside of domain VM. Right click start select “Run” and input “wf.msc” ➡️ deactivate 3 firewalls via(Windows Defender Firewall Properties) and select apply then okay; disable: Domain,Private,Public.</li>
<li>	Change DNS settings for Client-1 to DC-1 ➡️ Copy DC-1 private IP ➡️ network settings for Client-1 ➡️ Ipconfig selection ➡️ “DNS servers” switch to “Custom” ➡️ paste DC-1 private IP</li>
<li>	Test for connectivity between VMs ➡️ open powershell in client ➡️ ping “10.0.0.4” then “ipconfig /all” ➡️ the 2nd command confirms the dns server</li>
</ol>

<h2>🧱 Step 2: Deploying Active Directory</h2>
  <ul>
  <li>	The goal of this portion of the lab is to create a system for using the active directory through both VMs.
</li>
  </ul>
<ol>
<li>	INSIDE DC: open server manager and select “2 add roles/feats” ➡️ next twice followed by “ADDS” 2nd from the top; restart option = yes; install ➡️ </li>
<li>	Select random flag ➡️ promote the whatever and add a new forest ➡️ “mydomain.com” w/Password1 ➡️ uncheck “create DNS deli”</li>
<ol>
  <li>	Be midful of the fact that when logging into DC you must now specify “mydomain.com/”+“user”; the reason for this is cuz it’ll default to local user instead of domain
  </li>
</ol>
<ol><li>	Create a domain admin user; Understand Organizational Unit</li></ol>
<li>	Access “Start” ➡️ Windows Admin Tools ➡️ AD Users/Computers ➡️ drop “mydomain.com” to examine content ➡️ right click to select “New” and create Organizational Unit ➡️ 2 new units are “_ADMINS”, ”_EMPLOYEES”; underscores shift order to top separating from other junk.</li>
<li>	Right click “_ADMINS” to access “new” ➡️ Jane Doe “jane_admin” creation ➡️ password setting: “never expires”</li>
<li>	Admin status is attained via addition of user to “Admin Security Group” ➡️ right click jane account ➡️ select properties then “member of” ➡️ enter “domain admins” then check names ➡️ apply and COMPLETE</li>
<li>	Perform cool logoff method ➡️ hit “Start” ➡️ “Run” and enter “logoff”</li>
<ol><li>	Join client 1(computer) to domain: </li></ol>
<li>	Log on to DC-1 as jane ➡️ make sure steps in are done ➡️ right-click “start” then select “systems” ➡️ on “Computer name” tab select “change” to join domain and leave WRKGRP ➡️ enter “mydomain.com” when it presents then sign in as jane ➡️ </li>
</ol>

<img width="451" alt="Screenshot 2025-04-14 at 4 25 40 PM" src="https://github.com/user-attachments/assets/4031ca9f-1164-45a7-ad29-96ce4206c5c3" />
v.	Creating Users with PowerShell: Logging on as a domain admin to create users and manage access-pathways
1.	Start w/ “start” and “System” to get to “Remote desktop”  Focus on users accounts
a.	Go to PowerShell ISE then copy script from lab sources. Must create “new file” what the script

<h2 id="references">📚 References</h2>
<ul>
  <li><a href="https://r0ttenbeef.github.io/Active-Directory-Local-Lab-Environment-Setup/">Active Directory Local Lab Environment Setup </a></li>
  <li><a href="https://www.windows-active-directory.com/object-permissions-active-directory.html">Active Directory Object permissions: Step-by-Step guide to managing permissions using GPOs, ADUC, and PowerShell</a></li>
  <li><a href="https://rootdse.org/posts/active-directory-lab-setup-add-data/">Active Directory Lab: AD Domain</a></li>
  <li><a href ="https://www.youtube.com/watch?v=qYwzrarxmws"> How to view and modify object permissions </a></li>
</ul>

</p>
<p>💬 <em>Feel free to contribute or report issues via GitHub!</em></p>

</body>
</html>
