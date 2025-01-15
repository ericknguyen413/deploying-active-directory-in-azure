<p align="center">
<img src="https://i.imgur.com/aMMGyHQ.jpeg" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />

<h1>Deploying Active Directory in Azure</h1>

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

To start, in the DC (Domain Controller), bring up the Server Manager dashboard, if it's not up already:

![image](https://github.com/user-attachments/assets/fc6b0e3b-f6cf-4a7e-ba25-9eab607396d0)

Here, click on "Add roles and features" > Next > Next > For server selection there should only be one to select:

![image](https://github.com/user-attachments/assets/01559a6a-4168-4d94-b81e-97c7e70d2b30)

Now, check "Active Directory Domain Services" > Click "Add Features":

![image](https://github.com/user-attachments/assets/6241e822-66f6-4023-8da5-d13bca966020)

Select "Active Directory Domain Services" > Next > Next > Next > and click "Install":

![image](https://github.com/user-attachments/assets/16da9844-b774-4f08-af31-8ccd23525149)

Next, I have to promote this machine as a DC by configuring it as a new forest as "mydomain.com" (this can be anything, mydomain.com is just easy to remember) So, to do this, I will click on the flag icon with the caution symbol, near the top-right of the Server Manager Dashboard and select "Promote this server to a domain controller":

![image](https://github.com/user-attachments/assets/2e1b6844-93cb-4de3-aa5a-4b81ca4ea79c)

Then select "Add new forest" and in "Root domain name" I'll type "mydomain.com". Then "Next":

![image](https://github.com/user-attachments/assets/82018778-fd57-467b-8f16-1141113d9d64)

Type in a password of your choosing on the next screen like so then hit "Next":

![image](https://github.com/user-attachments/assets/b11edebb-b7d4-4270-8a61-9423c7fedbee)

Leave "Create DNS delegation" unchecked and click "Next". Hit "Next" until you reach this screen. On this screen, click "Install". The machine will restart after the installation:

![image](https://github.com/user-attachments/assets/1a1eb664-ba4f-456c-a784-948665172d81)

It may take some time to restart, but once it does, sign back in using "mydomain.com\labuser" (if you're using the same names as me) as the username. Use the password you set for "labuser". Here we have to sign in as "mydomain.com\labuser" instead of just "labuser", because now we've promoted this machine to a DC and it needs to know the context of who we're siging in as (someone in the domain, or a local user). I want to sign in as a user on the domain, so I specify that with the leading "mydomain.com\" part at sign in:

![image](https://github.com/user-attachments/assets/60a0f0a9-1b3e-4a05-a89e-57e7fb275a45)

Now, we'll create an admin user. To do this, in the start search bar, search for "Active Directory Users and Computers":

![image](https://github.com/user-attachments/assets/efcf9b04-d7c4-46ae-98b8-1d78206bee0c)

Right click on "mydomain.com" and select "New" > "Organizational Unit" and name it EXACTLY "_EMPLOYEES" (In a future project we will run a script that uses this name to work). Do the same steps here to create an OU (Organizational Unit) named "_ADMINS":

![image](https://github.com/user-attachments/assets/289a731e-662d-44b6-9e25-77fe7606a68c)

![image](https://github.com/user-attachments/assets/c692310d-4a22-4b84-a6fc-b35966586a97)

Next, create a new user in "_ADMINS" by right-clicking on "_ADMINS" > New > User and fill it out like so:

![image](https://github.com/user-attachments/assets/58d96969-c9cd-4626-96d5-d032923bbe3c)

Then, create a password and uncheck "User must change password at next logon" and check "Password never expires" (You probably wouldn't do this in real life, but we'll do it for this lab where nothing's really at stake):

![image](https://github.com/user-attachments/assets/318b4834-a972-46a9-bd94-ec7fdfa348bc)

Jane Doe is now apart of the "_ADMINS" OU, but isn't actually an admin. To make her an admin right-click on her name > Properties > Member Of > Add... > Enter "domain admins" > Check Names > OK > Apply > OK:

![image](https://github.com/user-attachments/assets/e2f22a59-5c56-424a-834a-da5e6f550197)

Now, we can log out of DC and log back in using Jane's credentials:

![image](https://github.com/user-attachments/assets/a0accb69-d4d3-48b1-bef9-562df7bf1ca6)

Once logged in, log into client-1 VM, if not already. Here I'll join this client to the domain by right-clicking the start menu > Systems > Rename this PC (advanced) > Change > Select "Domain" and enter "mydomain.com" and hit "OK":

![image](https://github.com/user-attachments/assets/b864f1a9-a1c9-47e2-b103-e283fc3f34b5)

It'll ask for an account with permission to join the domain. We can use our admin's credentials for Jane here. Then a pop up saying welcome to the domain will appear and the machine will try and restart:

![image](https://github.com/user-attachments/assets/07466961-4b6d-4eb4-ac79-dc288ff9a769)

After the restart, client-1 is now a member of the domain. To check, in the DC machine start seach bar, search for "Active Directory Users and Computers" > mydomain.com > Computers > client-1 should be listed:

![image](https://github.com/user-attachments/assets/3483ff6d-d7f0-494b-8b9a-537d8d8a94dd)

Now we can create another OU as we did before and name it "_CLIENTS". Then drag and drop client-1 from "computers" to "_CLIENTS":

![image](https://github.com/user-attachments/assets/9f863873-5caa-4a46-aae8-c324821a4d51)

![image](https://github.com/user-attachments/assets/4e9e4e02-7a7c-4a22-a77e-47dc9cf99c89)

![image](https://github.com/user-attachments/assets/ea55c35b-e01c-40a4-a9c1-8c893b752aaa)

<h2>Active Directory is Deployed and Ready for Use! </h2>

<b>We've successfully installed AD on the domain controller, created a domain admin, and joined the client VM to the domain. Next, we'll create users in Powershell by running a script, and then manage the accounts and group policy!  </b>
<br />
