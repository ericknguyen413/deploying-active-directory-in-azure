<p align="center">
<img src="https://i.imgur.com/aMMGyHQ.jpeg" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />

<h1>Deploying Active Directory in Azure</h1>

 ### [YouTube Demonstration](https://youtu.be/Zrb_DDrI_p4)

<h2>Description</h2>
For this portion of the Active Directory project, I install Active Directory on the domain controller, create a domain admin, and join the client VM to the domain.  <br/>
<br />

This project is a continuation of [Active Directory: Preparing Infrastructure in Azure](https://github.com/ericknguyen413/preparing-active-directory-infrastructure-in-azure), so this project picks up where I left off. <br />
<br />

<h2>Environments and Utilities Used</h2>

- <b>Microsoft Azure</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop Connection</b>
- <b>Active Directory</b>

<h2>Operating Systems Used </h2>

- <b>Windows Server </b>
- <b>Windows 10</b>

<h2>Project Walk-through:</h2>

To start, in the DC (Domain Controller), bring up the Server Manager dashboard, if it's not up already: <br/>

Here, click on "Add roles and features" > Next > Next > For server selection there should only be one to select: <br/>

Now, check "Active Directory Domain Services" > Click "Add Features":  <br/>

Select "Active Directory Domain Services" > Next > Next > Next > Now select "Restart the destination server automatically if required" and click "Install": <br/>

Next, I have to promote this machine as a DC by configuring it as a new forest as "mydomain.com" (this can be anything, mydomain.com is just easy to remember) So, to do this, I will click on the flag icon with the caution symbol, near the top-right of the Server Manager Dashboard and select "Promote this server to a domain controller":  <br/>

Then select "Add new forest" and in "Root domain name" I'll type "mydomain.com". Then "Next":  <br/>

Type in a password of your choosing on the next screen like so then hit "Next":  <br/>

Leave "Create DNS delegation" unchecked and click "Next". Hit "Next" until you reach this screen. On this screen, click "Install". The machine will restart after the installation:  <br/>

It may take some time to restart, but once it does, sign back in using "mydomain.com\labuser" (if you're using the same names as me) as the username. Use the password you set for "labuser". Here we have to sign in as "mydomain.com\labuser" instead of just "labuser", because now we've promoted this machine to a DC and it needs to know the context of who we're siging in as (someone in the domain, or a local user). I want to sign in as a user on the domain, so I specify that with the leading "mydomain.com\" part at sign in:  <br/>

Now, we'll create an admin user. To do this, in the start search bar, search for "Active Directory Users and Computers":  <br/>

Right click on "mydomain.com" and select "New" > "Organizational Unit" and name it EXACTLY "_EMPLOYEES" (In a future project we will run a script that uses this name to work). Do the same steps here to create an OU (Organizational Unit) named "_ADMINS":  <br/>

Next, create a new user in "_ADMINS" by right-clicking on "_ADMINS" > New > User and fill it out like so:  <br/>

Then, create a password and uncheck "User must change password at next logon" and check "Password never expires" (You probably wouldn't do this in real life, but we'll do it for this lab where nothing's really at stake):  <br/>

Jane Doe is now apart of the "_ADMINS" OU, but isn't actually an admin. To make her an admin right-click on her name > Properties > Member Of > Add... > Enter "domain admins" > Check Names > OK > Apply > OK:  <br/>

Now, we can log out of DC and log back in using Jane's credentials:  <br/>

Once logged in, log into client-1 VM, if not already. Here I'll join this client to the domain by right-clicking the start menu > Systems > Rename this PC (advanced) > Change > Select "Domain" and enter "mydomain.com" and hit "OK":  <br/>

It'll ask for an account with permission to join the domain. We can use our admin's credentials for Jane here. Then a pop up saying welcome to the domain will appear and the machine will try and restart:  <br/>

After the restart, client-1 is now a member of the domain. To check, in the DC machine start seach bar, search for "Active Directory Users and Computers" > mydomain.com > Computers > client-1 should be listed:  <br/>

Now we can create another OU as we did before and name it "_CLIENTS". Then drag and drop client-1 from "computers" to "_CLIENTS":  <br/>

<h2>Active Directory is Deployed and Ready for Use! </h2>

<b>We've successfully installed AD on the domain controller, created a domain admin, and joined the client VM to the domain. Next, we'll create users in Powershell by running a script, and then manage the accounts and group policy!  </b>
<br />
