
# Cookies

**Flag:`picoCTF{3v3ry1_l0v3s_c00k135_88acab36}`**

**Thought Process**

* We're given the website [http://mercury.picoctf.net:6418/](http://mercury.picoctf.net:6418/)
![image](https://github.com/user-attachments/assets/f778a142-4cf3-4c82-b107-eb92453b1690)

Website is a simple HTML based website with a search bar that invites us to enter the names of different kinds of cookies. 
* Since this obviously has something to do with cookies, I immediately went to a cookie editor extension that I use, and noticed that when I entered, let's say the suggested text "snickerdoodle" into the search bar, it redirected me to a second page with the message "That is a cookie! Not very special though...".
* When I inputted the above text, the `value` parameter of the existing cookie changed from **-1** to **0**. 
* Now, while on the second page, I change the `value` parameter to a **2** to see what happens and the name of the cookie changed to a different one. 
* From here I knew that one of the values would eventually output the flag, so I started going sequentially from **2** and eventually, at the number **18**, the website gave me the flag.

**Alternative Methods**
* Once I knew we had to keep trying different inputs, I looked up scripts/applications that did it for me and came across a software called **BurpSuite**, which I downloaded and re-attempted the challenge with. 
* Once installed, under the intruder section, I started an attack with payload options such that it would sequentially send inputs from **0** to **50**, with step value set to **1**. 
* Once the attack had started, it gave me the lengths of the output, and I went thru the responses of lengths that seemed different from other ones. This was obviously request number **18**. 
* **Alternatively,** there exists a `grep` option in the software where if I grepped for `pico{`, while the table of lengths was being generated, it created a column for the grep and in the **18th** request, it returned the value **1** which means we found a match.

**Resources**

* https://www.hotcleaner.com/cookie-editor/
* https://portswigger.net/burp/documentation/desktop/tools/intruder/configure-attack/settings

# Forbidden Paths

**Flag:**``picoCTF{7h3_p47h_70_5ucc355_6db46514}``

**Thought Process**

*  Our website is another HTML e-Web reader that has yet another search-box where you can input stuff and it looks for that file to read. We're also given 3 texts as samples.
* By inputting one of the example texts, it takes us to the contents of the texts.
* Now, by reading the hints, we can tell that these files are located at `/usr/share/nginx/html/`, however, our flag file is located at the **root** or just `/`. (It's path is /flag.txt).
* Now, to go one directory back in Linux or windows, we use the command `..`, so i assumed this would be something related to that.

* First, I tried changing the url to include  `../../../../flag.txt` (we're four directories in, so `../` four times to get to the home/root directory) at the end, but in vain.
	
	Next, I tried inputting the same into the given search box and it outputted the flag.

![image](https://github.com/user-attachments/assets/86380036-bacb-407e-9f60-9760984d4f36)


**Mistakes Made**
* Spent a lot of time in PowerShell because I thought this involved changing the path there using certain commands. 
* Tried appending the relative path to the URL instead of the textbox at first.


# SOAP

**Flag: `picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}`**

**Thought Process**

* Since I had done a challenge on XXE injections in **Advent of Cyber 2024**, this one should be pretty straightforward.
* Since the challenge is based on XXE injections, I opened up BurpSuite and in the browser, pasted the link of the instance to get the website running and ready to be intercepted. The website is a simple HTML website that has a "Details" button that can be interacted with.
* Once I clicked on the "Details" button, This is the POST request I intercepted and sent to Repeater (Ctrl+R)


![Pasted image 20241208154220](https://github.com/user-attachments/assets/7ddf3ab3-e0cb-48c2-a411-c2d0515ddf0f)


Doing a quick google search for an XXE payload script, i came across this

```html
<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]> <stockCheck><productId>&xxe;</productId></stockCheck>
```

So adding this line to our current HTML POST request, it would look something like this

```html
< ?xml version="1.0" encoding="UTF-8"? >
< !DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ] >
<data><ID>
2
</ID></data>
```


also, replace 2 with `&xxe;` as to match the script, and once we send the request, the flag is in the list of passwords.

![Pasted image 20241208154923](https://github.com/user-attachments/assets/0d3f99fe-ab72-43e1-9425-6844b426dfaa)



**References**
* https://portswigger.net/web-security/xxe