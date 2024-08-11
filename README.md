<p align="center">
<img src="https://i.imgur.com/PxCaRJQ.png" alt="Photo of DNS lab"/>
</p>

<h1>DNS Record Management and Resolution Lab</h1>
This lab focused on creating and managing DNS A-records and CNAME records, updating DNS entries, and handling local DNS cache on a network.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- PowerShell
- Active Directory

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- A-Record Exercise 
- Local DNS Cache Exercise
- CNAME Record Exercise

<h2>Configuring DNS</h2>

<h3>Setup Resources in Azure</h3>
<p>
<img src="https://i.imgur.com/zty4oHY.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
<img src="https://i.imgur.com/tzjjqp3.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
First I created a Domain Controller VM with Windows Server 2022 and named it "DC-1". During this process, I took note of the Resource Group and Virtual Network (Vnet) that were created. I then set the Domain Controller's NIC Private IP address to be static. Next, I created a Client VM with Windows 10 and named it "Client-1", ensuring it used the same Resource Group and Vnet as "DC-1". 
</p>
<br />

<h3>Ensure Connectivity between the client and Domain Controller</h3>
<p>
<img src="https://i.imgur.com/QxoDn64.jpeg" height="80%" width="80%" alt="Enable ICMP echo request"/>
</p>
<p>
<img src="https://i.imgur.com/NSB1HnV.jpeg" height="80%" width="80%" alt="CLI ping request"/>
</p>
<p>
I logged into Client-1 using Remote Desktop and initiated a perpetual ping to DC-1's private IP address using the command ping -t <ip address>. Then, I logged into the Domain Controller and enabled ICMPv4 on the local Windows Firewall. After doing this, I checked back at Client-1 and saw that the ping was successful.
</p>
<br />

<h3>Install Active Directory</h3>
<p>
<img src="https://i.imgur.com/ALxQCLm.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
<img src="https://i.imgur.com/j01wWrG.jpeg" height="80%" width="80%" alt="Login page for labuser3"/>
</p>
<p>
I logged into DC-1 and installed Active Directory Domain Services. Then, I pointed the server to a Domain Controller and set up a new forest with the domain name craigsdomain.com. After the installation, I restarted the server and logged back into DC-1 as the user craigsdomain.com\labuser3.
</p>
<br />

<h3>A-Record Exercise</h3>
<p>
<img src="https://i.imgur.com/aDUuXCL.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
<img src="https://i.imgur.com/NWKn1Nu.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
<img src="https://i.imgur.com/giMA5tF.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
<img src="https://i.imgur.com/nT41inC.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
I connected to DC-1 using my domain admin account (mydomain.com\craig_admin) and logged into Client-1 as an admin (craigsdomain\craig_admin). From Client-1, I attempted to ping "mainframe" and noticed that it failed. I then used nslookup for "mainframe" and observed that it also failed due to no DNS record. To resolve this, I created a DNS A-record on DC-1 for "mainframe" and pointed it to DC-1's private IP address. Returning to Client-1, I tried pinging "mainframe" again and observed that it now worked.
</p>
<br />

<h3>Local DNS Cache Exerciese</h3>
<p>
<img src="https://i.imgur.com/CQITBHJ.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
<img src="https://i.imgur.com/RRWEqWA.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
<img src="https://i.imgur.com/F4iFujX.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
I went back to DC-1 and changed the mainframe's record address to 8.8.8.8. Then, I returned to Client-1 and pinged "mainframe" again, noticing that it still pinged the old address. I observed the local DNS cache using `ipconfig /displaydns` and then flushed the DNS cache with `ipconfig /flushdns`, confirming that the cache was empty. Finally, I attempted to ping "mainframe" again and observed that the address of the new record was showing up.
</p>
<br />

<h3>CNAME Record Exercise</h3>
<p>
<img src="https://i.imgur.com/SgFlDFl.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
<img src="https://i.imgur.com/ZQwE2wH.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
I went back to DC-1 and created a CNAME record that pointed the host "search" to "www.google.com". Then, I returned to Client-1 and attempted to ping "search", observing the results of the CNAME record. On Client-1, I performed an nslookup for "search" and observed the results of the CNAME record.
</p>
<br />
