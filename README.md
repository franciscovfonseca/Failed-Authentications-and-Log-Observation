<br>

# Percursor to Security Operations

<br>


![Banner](https://github.com/user-attachments/assets/74cda7d0-a70b-4459-b53a-70078edb326f)
<br />
<br />

In this lab we're going to create a 3ʳᵈ Virtual Machine called ```Attack-VM``` and we're going to perform actions against our current environment.

So from the **Attack-VM** we're going to act as an attacker and attemp to log into the **SQL Server Database**, the **Windows Vm** & the **Linux VM with SSH**.

After that we'll observe the **Logs Generated** from those actions.


<br />

<details close> 
<summary> <h2> 1️⃣ Create our Attack Virtual Machine</h2> </summary>
<br>

The first thing we're going to do in this lab is **Create another Windows VM outside of the US** to simulate an attack from anyhere in the world.

Go to the **Azure Portal** > Click on **Virtual Machines** > **Create a Virtual Machine**:

![azure portal](https://github.com/user-attachments/assets/4032bf0f-95dd-4edd-a1a2-c714da69ba61)


- Name the Resource Group: ```RG-Cyber-Lab-Attacker```
- Region:  Outside the US ➜ ```Australia Central``` for example
- Virtual Machine Name: ```attack-vm```
- Image: ```Image 10 Pro```
- Size: at least ```2vcpus```
- Username: ```labuser```
- Password: ```Cyberlab123!```

![azure portal](https://github.com/user-attachments/assets/259c4d7a-ca98-4046-93a0-3472a839f47d)

For the **Networking Tab** ➜ Name the **Virtual Network**: ```Lab-VNet-Attacker```

Then click **Review + Create**

![azure portal](https://github.com/user-attachments/assets/fe92eaa7-a8b2-4cee-a0bb-6f7dd36b1450)

<h2></h2>

<br>

Now we're going to log into the Attack VM to make sure it works:

- Copy the Attack Vm's **Public IP Address**:

![azure portal](https://github.com/user-attachments/assets/f614eae5-0760-45a9-8d83-4f5f8ec6f258)

- We're going to open **Microsoft Remote Desktop** > Add a New PC > Paste the **IP Address for the Attack VM**

![azure portal](https://github.com/user-attachments/assets/217832f9-3203-43a2-9b7b-524f0253f7b0)

- Double Click the **"New Added PC"** and type in the **Username & Password Credentials** we set up earlier:

  - Username: ```labuser```
  - Password: ```Cyberlab123!```

![azure portal](https://github.com/user-attachments/assets/b6622b8a-2af8-4c19-9823-c1f4594ae7e1)

✅ We were able to log in to the VM

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Generate some Failed RDP Logs against the windows-vm</h2> </summary>
<br>

> From within the **Attack Vm**, we're going to **attemp to RDP connect to the Windows VM**.
> 
> The logins will fail, but some Logs will be generated for us to look at later inside the Windows VM.

<br>

Go back to the **Azure Portal** and copy the **Public IP Address of the Windows VM**:

![azure portal](https://github.com/user-attachments/assets/55a2c962-8229-4fa1-89df-d7a9264e9e45)

Now inside the **attack-vm** > open **Remote Desktop Connection** > Paste the **Public IP Address of the Windows VM** and connect

![azure portal](https://github.com/user-attachments/assets/b1790beb-344f-4d2d-af49-9a6f30f282f8)

For the **Credentials** ➜ use some random made up **Username & Password** ➜ username ```Josh``` for example

Since this user does not exist in the Windows Vm ➜ the **Log In will Fail**

![azure portal](https://github.com/user-attachments/assets/ba4a924d-eb07-474c-8020-21e4ce0ad6d7)

Repeat the **Failed Log In** 2 more times with the same **Wrong Username & Password**

<br>

✅ So 3 Logs have been Generated on the Windows VM ➜ which we will analyse later

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Generate some Failed SQL Server Authentication Logs against the windows-vm</h2> </summary>
<br>

> Remember that we installed the SQL Server Database in the Windows VM ➜ so we're going to attempt to log into it now.
> 
> Still within the **Attack VM**, we're going to install **SSMS** ➜ which we'll use to attempt to log into the SQL Server

<br>

Go back to the **Attack VM** > Using **Microsoft Edge** > You can **[Download SSMS through this link](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)**

Open the **SSMS-Setup-ENU.exe** File from the Downloads > **Install** it

![azure portal](https://github.com/user-attachments/assets/ef8cd4ad-57df-40d6-b5b3-abca01b901bc)

![azure portal](https://github.com/user-attachments/assets/3688d252-582b-4dd5-8cc9-8949bbd2f757)


>   <details close> 
>   
> **<summary> 💡 Note</summary>**
> 
> We're going to use this **SSMS** to **Connect to the SQL Server in our Windows VM**.
> 
> Once the **Installation is Completed** ➜ we'll take the **Windows VM's Public IP Address** (which is where the **SQL Server** is) ➜ and we're going to **Generate some Logs** by attempting to Log Into it as a bad actor.
> 
> And then at the end of this lab we'll log back into the **Windows VM** again and **Inspect the Logs**
> 
> We'll also log into the **Linux VM** and **Inspect the Logs** in there as well.
>   </details>

<br>

<h2></h2>

<br>

Now we're going to open **SSMS**:

![azure portal](https://github.com/user-attachments/assets/8be34c69-6216-4aa7-b2f3-d633f2d82c2b)

Then we'll copy the **IP Address of the Windows VM** ➜ which has the **SQL Server**

- in Server Name: we'll Paste the **IP Address**
- Authentication: **SQL Server Authentication**
- then our Real **Username** is ```sa``` & **Password** is ```Cyberlab123!```

  - but we're going to login with a user that doesn't exist ➜ so we can **Generate Failed Logs**

![azure portal](https://github.com/user-attachments/assets/edb1c19c-5959-4764-a183-c74a9e5d6e76)

We'll **Attemp and Fail** to Login 2 more times to **Generate a total of 3 Failed Logins**

Then we'll "Login For Real" with the correct **Username** & **Password** just to show that we can Login from **Australia (Attack VM)**

![azure portal](https://github.com/user-attachments/assets/f70930a6-50b1-49f5-8fad-7b56571eaed1)

We can then disconnect from the Server:

![azure portal](https://github.com/user-attachments/assets/8d0f297f-b5ba-4933-8597-903be7144b25)

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Generated some Failed SSH Logs against the linux-vm</h2> </summary>
<br>

> Lastly, we're going to induce some **Failed Authentications against the Linux Server**.
> 
> This will allow us to **Generate some Logs in our Linux VM** & and analyse them later as well

<br>

Going back to the **Azure Portal** > Go to **Virtual machines** > Open the ```linux-vm``` > copy its **Public IP Address** 

  ![VM create](https://github.com/user-attachments/assets/cf625627-4567-452b-b13e-d07a8f4cb4b9)

  ![VM create](https://github.com/user-attachments/assets/4668916b-869d-4065-8274-99063e0625ba)

Then inside the Attack VM ➜ we'll open **Powershell**

  ![VM create](https://github.com/user-attachments/assets/8426567a-374f-42f1-b2bf-bf01ad2c2f9e)

Now we can **SSH** ➜ use a **Fake Username** ```josh``` ➜ this Username doesn't exist on the **Linux VM**

So we'll type: ```ssh "FAKE USERNAME"@"DESTINATION (which is the Linux VM)"```  ➜ press "Enter" to attempt to connect:

  ![VM create](https://github.com/user-attachments/assets/7fb6f2d1-c152-43e3-b771-f1bc10716e71)

Type **"yes"** to Accept the Certificate:

  ![VM create](hhttps://github.com/user-attachments/assets/bf5dc7e4-245f-4864-b849-5105d1fa1b4d)

Then it'll ask us to **Enter our Password**

This **User doesn't even exist**: so whatever Password we put ➜ it's going to **Fail to Login & Create a Log**.

Basically we're attempting to **Brute-Force into the Linux VM**  ➜ do it 3 times to **Generate 3 Failed Logins**:

  ![VM create](https://github.com/user-attachments/assets/fab202f7-a7a0-4007-a1ae-df4dc01e9097)

Then we can actually Shut Down our **Attack VM** ➜ we won't be using it anymore for this Lab ➜ and go back to our own Computer

  ![VM create](https://github.com/user-attachments/assets/6922b5a7-bf0e-46b9-ac0b-b79be1074f97)

  </details>

<h2></h2>
<details close>
  
<summary> <h2>5️⃣ Enable Logging for SQL Server to be ported into Windows Event Viewer</h2> </summary>
<br>

> So now we're going to connect back to the **Windows VM** ➜ take a look at the **Event Viewer** ➜ and look at the **Logs we Generated**
> 
> We'll do the same thing with the **Linux VM** ➜ look at the **Logs we Generated** by attempting to Log into it

<br>
















> The next thing we're going to do is **Enable Logging for SQL Server**, in order to send the logs to the **Windows Event Log**.
> 
> This part is a bit troublesome to do ➜ there's a few steps we have to do to **Enable Logging for SQL Server**.

<br>

You can **[Follow this Link to Write SQL Server Audit Events to the Security log](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16)**

<br>

  <details close> 
  
**<summary> 📝 Summary</summary>**

We can view all the logs for the Windows VM through the **Event Viewer**.

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

For example if we go to Windows **Logs** > **Security** > click on one of the **Events / Logs** ➜ we can se the details: ***"An account was successfully logged on."***

Whenever someone fails a login, or has a succesful login ➜ that's going to be recorded in the **Event Viewer** and we can see it:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

<br>

> Basically what we're doing right now is set up the **SQL Server** ➜ so that when somebody **Fails to Authenticate** against it, we'll be able to see the logs for that in the **Event Viewer**
> 
> And to achieve that we first need to provide full permissions for the SQL Server service account to the registry hive (**Registry Editor**).
> 
> The **Windows Registry** is a place in the computer where we can make a lot of granular configurations to affect the way the OS behaves.

  </details>

<br>

<br>

First we're going to open the **Registry Editor**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Paste the following Registry path inside it (instead of browsing to it):

```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security```

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Then we'll right-click the **Security** key > click on **Permissions**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Add the ```NETWORK SERVICE``` account to the permission > and thick the ☑ boxes **Full Control** and **Read**

Click **"Apply"** and then **"OK"**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

<br>

<h2></h2>

<br>

Now we'll **Enable Auditing from SQL Server**

From the Start menu > type **cmd** > right-click on **Command Prompt** and **Run as administrator**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Paste the following **statement** > press **"Enter"** > and you can see that the command was **successfully executed** ✔️

```auditpol /set /subcategory:"application generated" /success:enable /failure:enable```

<br>

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

<br>

<h2></h2>

<br>

The next thing to do is open the **SSMS**, log into it and **Enable Auditing**.

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

To Connect to the SQL Server ➜  we can select **"SQL Server Authentication"** and use the **SQL Server system administrator credentials** we had set up earlier:

- Username: ```sa```
- Password: ```Cyberlab123!```

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Then we'll go to the **Properties** of the Server we just connected to > go to **"Login auditing"** > and check ◉ **Both failed and successful logins**

This way all the login attempts can be logged to the **Event Log**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

And finally we just have to **Restart** the Server ➜ right-click on the **windows-VM SQL Server** and click on **"Restart"**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

  </details>

<h2></h2>

<details close> 
<summary> <h2>6️⃣ Test SQL Logging to make sure it is Working Properly</h2> </summary>
<br>

We'll now "attempt" to reconnect to the SQL Server **Intentionally Using a Wrong Password**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Then close **SSMS** and open the **Event Viewer**.

Under the **Windows Logs** > **Application Log** ➜ ⚠️ this is where the **SQL Server Login Attempts** are going to be recorded.

You can see bellow the Event of the **Login failed** we intentionally generated using a Wrong Pasword:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

✅ We can confirm that this is working properly.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>7️⃣ Test Ping & Logging into the Linux VM via SSH</h2> </summary>
<br>

> The next thing we have to do is Ping the Linux Virtual Machine ➜ make sure it is pingable:
> 
> We'll log into the Linux VM with Secure Shell (SSH) Protocol
>
> <br>
> 
>   <details close> 
>   
> **<summary> 💡 Note</summary>**
> 
> In our case here there's no Linux interface by default ➜ so when we log in to our VM we're just going to use a Command Line Interface.
> 
>   </details>

<br>

Back to the Azure Portal, we'll go to the ```linux-vm``` > copy the **Public IP Address**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

- Then if you're running Windows ➜ open **Powershell**-
- But if you're running Mac ➜ open **Terminal**

We're going to attempt to ping our **Linux VM** > type in the **IP Address**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

✅ We can confirm that we were able to successfully **Ping the Linux VM**:

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

Now to SSH into the Virtual Machine, we have our ```linux-vm``` Username & Password:

- Username: ```labuser```
- Password: ```Cyberlab123!```

To connect into a machine with SSH we just type:

```commandline
ssh USERNAME@IPADDRESS
```
And then we press "Enter"

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

<br>

>   <details close> 
>   
> **<summary> 💡 Note</summary>**
> 
> Then it'll ask us if we want to trust the certificate that the Virtual Machine is "offering" to establish the SSH connection:
> 
> - So we're just going to say **"Yes"** to it.
> 
>   ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)
> 
> Then we'll import the certification into our computer, and our computer will trust it ✔️
> 
>   </details>

<br>

Then we'll just have type in our **Password** for SSH to log into the **Linux VM**.

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

You'll see that your prompt changed to **labuser@linux-vm**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

To confirm you´re logged in you can:

- Type ```uname -a``` and it will tell you what Operating System it is running: **Linux**
- And you can also type ```id``` and it will tell you **labuser**

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

✅ This is how you know you were **Successfully Logged into the Linux VM**.

<br>

 <details close> 

> **<summary> 💡 Note</summary>**
> 
> We're not going to be doing much with this Linux VM in this lab ➜ it's just another Endpoint for people to attack
> 
> Later we'll intall something in the Linux Machine that will let us forward the Logs into our Log Repository.
> 
> But for now I just wanted to make sure it is running ➜ and it is ✔️
> 
>  </details>

<br>

<br>

  </details>

<h2></h2>

<br>

# Conclusion

<br>

In this lab we:

- Disabled the **Internal Firewall on the Windows Computer**.

- Installed the **SQL Server Database into the Windows Computer**.

- Logged into the **Linux Computer with SSH**.

<br />

Now we will leave our Virtual Machines ON for the at least 24 hours

➡️ This will give bad actors, bots or attackers a chance to **Discover our VMs** and **Generate Logs**.

 
<br />

<br />

<br />  

<br /> 

<br />

<br />  

<br /> 
 
