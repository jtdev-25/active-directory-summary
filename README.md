<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>

<h1>🛠️ Active Directory Setup Guide</h1>

<p>A complete step-by-step guide to set up and manage <strong>Active Directory</strong> on Windows Server. Includes installation, configuration, user and group management, and domain policies.</p>

<h2>📁 Table of Contents</h2>
<ul>
  <li><a href="#overview">Overview</a></li>
  <li><a href="#prerequisites">Prerequisites</a></li>
  <li><a href="#environment-setup">Environment Setup</a></li>
  <li><a href="#step-1-install-active-directory-domain-services-ad-ds">Step 1: Install AD DS</a></li>
  <li><a href="#step-2-promote-server-to-domain-controller">Step 2: Promote to Domain Controller</a></li>
  <li><a href="#step-3-configure-dns-and-domain">Step 3: Configure DNS</a></li>
  <li><a href="#step-4-create-organizational-units-ous">Step 4: Create OUs</a></li>
  <li><a href="#step-5-user-account-management">Step 5: User Management</a></li>
  <li><a href="#step-6-group-management">Step 6: Group Management</a></li>
  <li><a href="#step-7-group-policy-objects-gpos">Step 7: GPOs</a></li>
  <li><a href="#step-8-best-practices--maintenance">Step 8: Best Practices</a></li>
  <li><a href="#references">References</a></li>
</ul>

<h2 id="overview">📌 Overview</h2>
<p>Active Directory (AD) is a directory service developed by Microsoft. This guide walks you through building an AD domain, managing users, devices, and applying policies using GUI and PowerShell.</p>

<h2 id="prerequisites">✅ Prerequisites</h2>
<ul>
  <li>Windows Server 2016/2019/2022</li>
  <li>Static IP configured</li>
  <li>Administrator access</li>
  <li>Updated OS & patches</li>
  <li>Internet for updates</li>
</ul>

<h2 id="environment-setup">🖥️ Environment Setup</h2>
<pre><code>Rename-Computer -NewName "AD-SERVER" -Restart</code></pre>

<h2 id="step-1-install-active-directory-domain-services-ad-ds">🧱 Step 1: Install AD DS</h2>
<h3>GUI Method</h3>
<ol>
  <li>Open <strong>Server Manager</strong></li>
  <li>Click <strong>Manage &gt; Add Roles and Features</strong></li>
  <li>Choose <em>Role-based installation</em></li>
  <li>Select <strong>Active Directory Domain Services</strong></li>
</ol>

<h3>PowerShell Method</h3>
<pre><code>Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools</code></pre>

<h2 id="step-2-promote-server-to-domain-controller">🚀 Step 2: Promote Server to Domain Controller</h2>
<h3>GUI Method</h3>
<ol>
  <li>Click notification flag in Server Manager</li>
  <li>Choose <strong>Promote this server to a domain controller</strong></li>
  <li>Create new forest: <code>corp.local</code></li>
</ol>

<h3>PowerShell Method</h3>
<pre><code>
Install-ADDSForest -DomainName "corp.local" -DomainNetbiosName "CORP" -InstallDNS -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS" -LogPath "C:\Windows\NTDS" -SysvolPath "C:\Windows\SYSVOL" -Force -SafeModeAdministratorPassword (ConvertTo-SecureString -AsPlainText "YourDSRMPassword" -Force)
</code></pre>

<h2 id="step-3-configure-dns-and-domain">🌐 Step 3: Configure DNS and Domain</h2>
<p>Ensure DNS points to local server IP. Test domain:</p>
<pre><code>nslookup
ping corp.local</code></pre>

<h2 id="step-4-create-organizational-units-ous">🗂️ Step 4: Create Organizational Units (OUs)</h2>
<pre><code>
New-ADOrganizationalUnit -Name "Users" -Path "DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Computers" -Path "DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Groups" -Path "DC=corp,DC=local"
</code></pre>

<h2 id="step-5-user-account-management">👤 Step 5: User Account Management</h2>

<h3>Create User</h3>
<pre><code>
New-ADUser -Name "Alice Smith" -GivenName "Alice" -Surname "Smith" -SamAccountName "asmith" -UserPrincipalName "asmith@corp.local" -Path "OU=Users,DC=corp,DC=local" -AccountPassword (ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force) -Enabled $true
</code></pre>

<h3>Reset Password</h3>
<pre><code>
Set-ADAccountPassword -Identity "asmith" -NewPassword (ConvertTo-SecureString "NewP@ssw0rd" -AsPlainText -Force) -Reset
</code></pre>

<h3>Enable / Disable User</h3>
<pre><code>
Disable-ADAccount -Identity "asmith"
Enable-ADAccount -Identity "asmith"
</code></pre>

<h2 id="step-6-group-management">👥 Step 6: Group Management</h2>

<h3>Create Group</h3>
<pre><code>
New-ADGroup -Name "HR Team" -GroupScope Global -Path "OU=Groups,DC=corp,DC=local" -GroupCategory Security
</code></pre>

<h3>Add User to Group</h3>
<pre><code>
Add-ADGroupMember -Identity "HR Team" -Members "asmith"
</code></pre>

<h2 id="step-7-group-policy-objects-gpos">🛡️ Step 7: Group Policy Objects (GPOs)</h2>

<h3>Create and Link GPO</h3>
<pre><code>
New-GPO -Name "SecurityPolicy"
New-GPLink -Name "SecurityPolicy" -Target "OU=Users,DC=corp,DC=local"
</code></pre>

<h3>Examples of Policies</h3>
<ul>
  <li>Password complexity</li>
  <li>Account lockout duration</li>
  <li>Desktop wallpaper</li>
  <li>Software restriction policies</li>
</ul>

<h2 id="step-8-best-practices--maintenance">🧠 Step 8: Best Practices & Maintenance</h2>
<ul>
  <li>Enable auditing</li>
  <li>Document changes</li>
  <li>Regular backups</li>
  <li>Secondary domain controller</li>
  <li>Review memberships periodically</li>
</ul>

<h2 id="references">📚 References</h2>
<ul>
  <li><a href="https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/">Microsoft Docs - AD DS</a></li>
  <li><a href="https://docs.microsoft.com/en-us/powershell/module/addsadministration/">PowerShell AD Cmdlets</a></li>
  <li><a href="https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/">Group Policy Settings</a></li>
</ul>

<p>💬 <em>Feel free to contribute or report issues via GitHub!</em></p>

</body>
</html>
