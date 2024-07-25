<br>

<h1 align="center">Failed Authentications & Log Observation</h1>

<p align="center">
<img width="800" src="https://github.com/user-attachments/assets/10bb4eb4-124b-41ff-85c9-7a56d87ac47c" alt="Banner"/>
<br />

<br />

In this lab we're going to create a 3ʳᵈ Virtual Machine called ```attack-vm``` and we're going to perform actions against our current environment.

From the **Attack-VM** we're going to act as an attacker and attemp to log into the **SQL Server Database**, the **Windows Vm** & the **Linux VM with SSH**.

After that we'll observe the **Logs Generated** from those actions.

We are basically simulating someone trying to break into our environmet, and then see what logs are created, just to get a good sense of what happens behind the scenes.

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

For the **Credentials** ➜ use some random made up **Username & Password** ➜ username ```josh``` for example

Since this user does not exist in the Windows VM ➜ the **Log In will Fail**

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

  ![VM create](https://github.com/user-attachments/assets/bf5dc7e4-245f-4864-b849-5105d1fa1b4d)

Then it'll ask us to **Enter our Password**

This **User doesn't even exist**: so whatever Password we put ➜ it's going to **Fail to Login & Create a Log**.

Basically we're attempting to **Brute-Force into the Linux VM**  ➜ do it 3 times to **Generate 3 Failed Logins**:

  ![VM create](https://github.com/user-attachments/assets/fab202f7-a7a0-4007-a1ae-df4dc01e9097)

Then we can actually Shut Down our **Attack VM** ➜ we won't be using it anymore for this Lab ➜ and go back to our own Computer

  ![VM create](https://github.com/user-attachments/assets/6922b5a7-bf0e-46b9-ac0b-b79be1074f97)

  </details>

<h2></h2>
<details close>
  
<summary> <h2>5️⃣ Inspect the Logs for RDP & SQL Server in the Windows VM</h2> </summary>
<br>

> So now we're going to connect back to the **Windows VM** ➜ take a look at the **Event Viewer** ➜ and look at the **Logs we Generated**
> 
> We'll do the same thing with the **Linux VM** after ➜ look at the **Logs we Generated** by attempting to Log into it

<br>

Inside our **Windows Vm** > Copy the **Public IP Address**:

  ![VM create](https://github.com/user-attachments/assets/8119aa20-9f4f-4424-a1f8-0630ec596de2)

Open **Remote Desktop** > connect to the **Windows VM**

  ![VM create](https://github.com/user-attachments/assets/7ad32938-0e9a-4b11-8c2b-c05a7ad42f10)

Then open **Event Viewer**:

  ![VM create](https://github.com/user-attachments/assets/926e5fa9-2c64-4711-b66d-5f887bf8a554)

Again, this is where the logs are ➜ First we're going to check out the **"Security"** ones.

These are the logs for when someone attempts to **Connect with Remote Desktop** or even try to **Map a Remote File Share**.

Event ```4625``` is the **Failed Logon Event** ➜ and we can see a whole bunch of them here:

  ![VM create](https://github.com/user-attachments/assets/4784cf0d-b0a6-456b-90d6-f4ad67433c18)

We didn't generate most of these.

If we **"Filter Current Log"** > and type ```4625``` just to see the **Failed Logons** ➜ it'll show us only the **Failed Logons**:

  ![VM create](https://github.com/user-attachments/assets/1cf93df3-ff56-4530-bde5-4d96953ba8c2)

We can see the the **"Account Name"** is different for each Event ➜ and these are **actual bad actors or bots** on the Internet.

These are not our **Intentional Failed Logons** ➜ these are other randon entities trying to **Logon to our VM**, since it's been on for the previous 10 hours.

  ![VM create](https://github.com/user-attachments/assets/ba3540e9-6dbe-4787-b3f1-c12cf654065f)

If we scroll down we can actually find **Our Own Failed Logon** with the **Username** ```josh```

  ![VM create](https://github.com/user-attachments/assets/369ab5b2-bc4f-454d-9920-e7715fdaea48)

⚠️ Aside from our 3 **Intentionally Generated Failed Logs**:
- We can see that there were almost **2000 different Attempts to Break Into our Windows VM**:

  ![VM create](https://github.com/user-attachments/assets/5f796671-51d0-456a-874f-4692309a73e9)

<br>

<h2></h2>

<br>

> Next, because SQL Server is actually installed on this Windows VM ➜ we're going to check the **"Application"** Logs.
> 
> In the Windows Event Viewer, the **SQL Logs** get registered in the **Application Tab** instead of the **Security Tab**.

<br>

Right away ➜  we can see our Successful Login with the User ```sa```:

  ![VM create](https://github.com/user-attachments/assets/e2da67ee-82d6-49f5-9ff5-9622555f97b8)

We're also able to see the **Failed Logins** into the **SQL Server** using the made up **Username** ```josh``` ➜  which does not exist in the **Windows VM**:

  ![VM create](https://github.com/user-attachments/assets/4669752d-99a4-4bac-8223-7ff7eccfc025)

<br>

>   <details close> 
>   
> **<summary> 💡 Summary</summary>**
> 
> From our own Computer ➜ we **RDP into the Windows VM**. 
> 
> We then inspected the **Login Failures and Successes** for both **RDP** as well as the **SQL Server**.
> 
> We looked at the event IDs:
>   - **4625** for the **RDP Failed Logins**
>   - **18456** for the **Failed Logins for the SQL Server**
> 
>   </details>

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>6️⃣ Inspect the Logs for the Linux VM</h2> </summary>
<br>

> The next thing we're going to do is **Login to the Linux VM**.
> 
> And then we're going to take a look at the **Failed Logs** and finish this lab.

<br>

To **Attempt to Connect to the Linux VM** ➜ we first need the get the **IP Address of the Linux VM**:

  ![VM create](https://github.com/user-attachments/assets/8e383e43-822b-406f-83d6-7ed9692019dc)

From our Computer:
- if you're using a **Mac** ➜ **open Terminal**
- if you're using **Windows** ➜ **open Powershell**

And type in the following:

```commandline
ssh labuser@PUBLIC IP ADDRESS OF THE LINUX VM
```
<br>

Press **"Enter"** > then type the **Password**: ```Cyberlab123!``` > then press **"Enter"** again

  ![VM create](https://github.com/user-attachments/assets/96d6e073-43de-413b-9371-97b70d671a65)

We are now Logged Into the **Linux VM**

To see the **Logs** we can type in the following **Linux Command**:

```commandline
cd /var/log
```
<br>

☝️ This will basically change our Directory to the **Log's Directory**

  ![VM create](https://github.com/user-attachments/assets/71f782a5-c731-4926-bc70-7af4558e9e4d)

And then we can type ```cat auth.log | grep password``` to pull out all the "lines" that have the word **"Password"** in it:

  ![VM create](https://github.com/user-attachments/assets/f73e017f-4b09-4685-bc6c-ea81a1ea614c)

We can see a whole bunch of **Failed Password for Invalid User** Events:
- Meaning ➜ some entities were trying to **Login to our Linux VM** from **Random IP Addresses** using **Wrong Credentials**.

We can also see our **Successful Logins** using the **Username** ```labuser```:

  ![VM create](https://github.com/user-attachments/assets/33d290c3-390f-4ead-bd48-f58e662a7eb5)

If we filter through:
- ```Accepted``` ➜  we can see that only we were able to **Successfully Login**:

```commandline
cat /var/log/auth.log | grep Accepted
```
<br>

- ```josh``` ➜  we can see all the **Unsuccessful Login Attempts** with the Username "josh":

```commandline
cat /var/log/auth.log | grep josh
```
<br>

  ![VM create](https://github.com/user-attachments/assets/0618dcc7-8548-4f5a-8a0a-bf95eb5759b0)


✅ This is Cybersecurity!


  </details>

<br>

<br>

<br>

# Conclusion


We have to keep in mind hat if we have, let's say, 1000 computers in our Environment ➜  it would be too laborious to just login into each one of them, and analyse the each log indivudually this way.

That's why we are going to **Automatically Ingest all the Logs into a Central Repository**.

We'll Set Up Queries to Automatically look through the Logs and Alert us on certain Events.

This is the end of our **Logging and Monitoring Lab**, and in the next phases we're going to Inspect Logs, Create Incidents & Respond to those Incidents.

<br />

<br />

<br />  

<br /> 

<br />

<br />  

<br /> 
 
