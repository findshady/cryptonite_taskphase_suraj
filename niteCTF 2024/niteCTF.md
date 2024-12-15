
# freakada 3301

**Flag: nite{4_wannabe_1nt3rn3t_my5tery}**

**Thought Process**
* As soon as I got the .png file, i ran `exiftool` on it and sure enough I found some cipher text
```
ZIT HQZI OL GWLEXKTR, WXZ ZIT ZKXZI SOTL OF ZIT LIQRGVL. LTTA ZIT HQZZTKFL. QDGFU ZIT EIQGL, Q WTQEGF QVQOZL: izzhl://woz.sn/yktqatlztof0. GFSN ZIT VGKZIN VOSS LTT ZIT XFLTTF. ZIT PGXKFTN IQL PXLZ WTUXF
```

which translated to (Mono-alphabetic Substitution)

```
THE PATH IS OBSCURED, BUT THE TRUTH LIES IN THE SHADOWS. SEEK THE PATTERNS. AMONG THE CHAOS, A BEACON AWAITS: HTTPS://BIT.LY/FREAKESTEIN0. ONLY THE WORTHY WILL SEE THE UNSEEN. THE JOURNEY HAS JUST BEGUN
```

The link led to a download link for .jpg file that had a picture of a duck.

* Ran `exiftool` and then `steghide extract -sf` on it to reveal a `secrets.txt` having the link that pointed to a GitHub repo which consisted of 3 things:
* 2 files that looked seemingly random and a file that had the second half of the flag in cipher text.

* The title of the two files were in alien language and hinted towards something forgotten and someone already being there.
* After checking the history of the aforementioned 2 files, I come across a long ass para along with some numbers in an x:y:z format. 
* I knew the numbers had to refer to the position of the characters within the para(x: line, y: word, z: character)
* I input all of this to ChatGPT and it gives me the following output

![WhatsApp Image 2024-12-15 at 20 43 56_8b56b806](https://github.com/user-attachments/assets/ef7b4a70-a8f9-49d0-98da-57860bd5d6a6)


Obviously a discord link, that after some fixing became:  https://discord.gg/AHj7R2Qd

* Joined the server and got baited into listening to the worst song of the decade which repulsed me enough to find a bot that had an interesting description that asked us to give it a prime and it would give us a location.
* Again, obviously, the prime number had to be **3301** and then it asked us to multiply it with 2 "integral" parts of the original image, which were the height and width.
* After providing it with the answer, it gave us coords that led to a very interestingly named place in our college campus, which had a QR code in it's reviews.

* Upon scanning the QR code, we are blessed with a .wav file that my teammate thought sounded creepy and upon uploading it in Audacity and turning on the option for **Muti-View** (basically had morse code)

![Pasted image 20241215214216](https://github.com/user-attachments/assets/13f73f91-84bb-4d6e-bfe2-1bf4d2ae347e)


The morse below the part-flag translates to **freekey**, which is a keyword to decipher the first part of our flag 

![Pasted image 20241215214311](https://github.com/user-attachments/assets/38062566-aead-415c-801a-56aebc6ab824)



# Ancient Ahh Display

**Flag: nite{tr0UbLe_bUbbLe_L0L}**

**Thought Process**
* We're given an excel sheet and a .txt file with data in it and the challenge hints towards the final output being on a Basys 3 Artix-7 display, which is basically a 7 segment display.
* Since the text file only mentioned these, i removed the rest from the Excel sheet:

![Pasted image 20241215215313](https://github.com/user-attachments/assets/63cdf375-645b-4669-8061-c2817b07cfd7)

* I ended up with:



![Pasted image 20241215215502](https://github.com/user-attachments/assets/ae9fae7d-1a1c-49ce-9356-ed283c6fdf08)


Now, the .txt file gives us this table to convert 5 bit binaries to 7 bit binaries ( I used default cases for `11111`)

![Pasted image 20241215215614](https://github.com/user-attachments/assets/df8b5b2f-dd22-4460-9aa7-369bb90b0aca)


Following this, I ended up with this :

![image](https://github.com/user-attachments/assets/36509427-dafa-4e39-a9e4-09e8c9bc361a)



* Tried to decipher them, but nothing made sense.
* Realized that this was in little endian and I had to convert it to big endian. 
* Finally ended up with :

```
0101011
1101111
0000111
0000100
0111111
1000110
0000111
0101111
1000000
1000001
0000011
1000111
0000100
1110111
0000011
1000001
0000011
0000011
1000111
0000100
1110111
1000111
1000000
1000111
1110000
0111111
```

* Now, after inputting this into my favorite website which had a 7-Segment Decoder, I got the following output

![Pasted image 20241215221233](https://github.com/user-attachments/assets/2d10d42d-de57-4938-b357-2b5a28926ebe)

**The Switch On/Off LEDs option had to be ticked, I spent a while thinking I had the wrong numbers**

Now, from here I had the basic flag that was `nite{trouble_bubble_lol}`, after some minor adjustments and like 10 different variations, the correct flag was `nite{tr0UbLe_bUbbLe_L0L}`


**References**
* https://www.dcode.fr/7-segment-display?__r=1.59e60746d52737bf7b9f3c5969ba28c5
* https://blockchain-academy.hs-mittweida.de/litte-big-endian-converter/


# U ARe T Detective

**Flag: nite{n0n_std_b4ud_r4t3s_ftw}**

**Thought Process**
* This first time in my life opening a .sr file, so it was a completely new challenge to me.
* After looking at some online tutorials, I downloaded PulseView and added a **UART** detector as hinted by the chall name.
* I was a stuck here for a while, tried changing a ton of values. (Deleted all the others as they had no data).
* Again doing some research online, I found out that ASCII values usually use **7 data bits**.
* After setting my data bits to 7 and data format to ACSII, I still had to figure out the baud rate, which seemed crucial to get the flag characters, because all I had gotten until now were frame errors.


![Pasted image 20241215224135](https://github.com/user-attachments/assets/5db681ca-3d74-4cd0-81e6-2e482d926084)

* After simply adding 0s to the end of the preset value for baud bits, I start to see different colored boxes appear (RX data and RX data bits). I also see the start and stop bit pop up.
* Now all i had to do was find the right baud rate, which took a lot of trial and error.
* I prompted ChatGPT with a screenshot and asked it to calculate the bit time (which is the inverse of baud rate), and it gave me the value  **2,000,000**.

Upon trying it, I see this:

![Pasted image 20241215231050](https://github.com/user-attachments/assets/9892fd21-1066-4f45-a52e-922b62cc0f24)

* I knew I had to make the stop bit align with the end of the signal, just like how the start bit aligns with the start, so I started increased it and I got closed to the perfect value.

* Since I started with 2 million, as I went from 5 million to 6 million, the stop bit of the first signal started overlapping with the start bit of the second signal. (My software crashed a total of 12 times during these shenanigans).

![Pasted image 20241215231803](https://github.com/user-attachments/assets/27b7ccf4-2fba-4f0d-af79-87d822d1bd4b)

* Since the I got the first character, and because of the overlap, i now know it's gotta be between 5 and 6 mil, and sure enough, 5.5 million was where it was at.

![Pasted image 20241215232101](https://github.com/user-attachments/assets/07ec0860-7446-42f4-b2ea-7b4bfcb5d630)


#  Glitch, Please!

**Flag: nite{4_R34L_0DD84LL}**

**Thought Process**
* Since the challenge mentioned that the accused players had exploited the game to **overflow their inventory limits**, upon applying conditional formatting to filter out players that have more than 150 items, exactly 20 players were flagged.
* Now, obviously the profile picture column of each player had the hex values of an image that spelt out the flag.
* One of my teammates wrote a python script to extract the images of all 20 accused players and once we had the images, it was only a matter of sorting them in a certain way that spelt out the flag.


![Pasted image 20241215233348](https://github.com/user-attachments/assets/1f33fa11-f40d-47fe-9978-5a2df3331b9f)

* We created a new excel sheet that had the data of only the accused. 

* Since we know that the flag started with **nite{**  the images that corresponded to those letters were numbered 10,16,8,4 and 7 (Referring to the very first row starting with 0).
![Pasted image 20241215234324](https://github.com/user-attachments/assets/db7e4d6b-027c-4296-8863-5a33eaa54d6c)


* We had to sort these in a way that we'd get these serial numbers in the same order.
* After a while, I finally found that when the players are sorted according to their scores in the ascending order, I got the numbers 10,16,8,4 and 7 in order. (Basically getting the first 5 characters of the flag).

![Pasted image 20241215233856](https://github.com/user-attachments/assets/8be5cce9-fb5d-4095-9086-39f076cd0fbc)

* From here on, we just arranged the pictures that we had in the order that we see here to spell out the flag.


# BuckSpeak

**Flag: nite{w3_b0th_kn0w_wh0_i5_ug1y_h3r3_ma1f0y}**

**Thought Process**
* The challenge started with a .wav file that looked like this when opened in Audacity and with spectrogram enabled

![Pasted image 20241216001031](https://github.com/user-attachments/assets/143330fc-a546-48a2-9f81-88b2990f9ad6)

* Furthermore, we tried putting the .wav file into **DeepSound**, as hinted,  but it kept prompting for a password.

* Using the hint, we come across a cipher named Bücking cipher, and the website converted plaintext to that cipher.

![Pasted image 20241216001550](https://github.com/user-attachments/assets/30659289-3ff4-4938-999c-6a794c057f03)

* My music nerd teammate manually listened to the the .wav file multiple times and using her knowledge of music notes, extracted characters individually.
* Finally, she found the plaintext to be `usephrasetruefansreadthebookstohearsomethingdeep`
* Obviously, the password for the DeepSound had to be `truefansreadthebooks`

![Pasted image 20241216001913](https://github.com/user-attachments/assets/193cc17b-37c4-497a-8a4f-edc44f78283b)

* DeepSound yielded 2 secret files, one was a .txt file so they don't get sued and the other was a .mkv file that was just a video of Malfoy getting bodied by BuckBeak.


* Next, I ran the command `mkvinfo screech.kmv` to find 
```bash
+ Attachments
| + Attached
|  + File name: Buckbeak.otf
|  + MIME type: application/x-truetype-font
|  + File data: size 109032
|  + File UID: 17636095151199042832
| + Attached
|  + File name: Mechanical-g5Y5.otf
|  + MIME type: application/x-truetype-font
|  + File data: size 112136
|  + File UID: 4317686372283026958
```

* I installed the .otf using the command
```bash
mkvextract attachments screech.mkv 1:Buckbeak.otf
```

* Once the font was installed, it took me a while to find the ciphertext, I analyzed all the files that we'd gotten from the beginning of the challenge and ran a bunch of commands on them to extract something hidden but in vain.
* While scanning the output of the `mkvinfo` command, I notice that there also exists Track Number 2 and 3. 
![Pasted image 20241216002827](https://github.com/user-attachments/assets/a30b9237-e0bf-4632-b505-6180e9fe1ee5)


* I extract track 2 and 3 using 
```bash
mkvextract screech.mkv tracks 2:./track_3.wav
mkvextract screech.mkv tracks 2:./track_2.wav
```

* I quickly realized that the extracted files were not actually .wav files so i accessed their contents using `cat` and i saw 

```bash
﻿[Script Info]
; Script generated by Aegisub 3.3.3
; http://www.aegisub.org/
Title: Default Aegisub file
ScriptType: v4.00+
WrapStyle: 0
ScaledBorderAndShadow: yes
YCbCr Matrix: None
PlayResX: 1280
PlayResY: 720

[Aegisub Project Garbage]
Last Style Storage: Default
Video File: ?dummy:23.976000:40000:1280:720:47:163:254:
Video AR Value: 1.777778
Video Zoom Percent: 1.125000

[V4+ Styles]
Format: Name, Fontname, Fontsize, PrimaryColour, SecondaryColour, OutlineColour, BackColour, Bold, Italic, Underline, StrikeOut, ScaleX, ScaleY, Spacing, Angle, BorderStyle, Outline, Shadow, Alignment, MarginL, MarginR, MarginV, Encoding
Style: Default,Noto Sans,48,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100,100,0,0,1,2,2,2,10,10,10,1
Style: CJK,Noto Sans TC,48,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100,100,0,0,1,2,2,2,10,10,10,1

[Events]
Format: Layer, Start, End, Style, Name, MarginL, MarginR, MarginV, Effect, Text
Dialogue: 0,0:00:00.00,0:00:08.89,CJK,,0,0,0,,UnderstaNDAblE Buckbēaĸ SCRéècHiɳg-ɲoǐşēЅz
```

* The last few words intrigued me, and so I went to notepad and I changed my font to Buckbeak and I pasted the last string in notepad using that font and the flag was a little off.
* Then I went to Windows settings and Font preview and finally got the perfect flag.

![Pasted image 20241216003803](https://github.com/user-attachments/assets/c8beef35-3c53-45aa-9911-d900a9a43078)


**References**
* https://legacy.wmich.edu/mus-theo/ciphers/bucking.html


# Le Casa de Papel 

**Flag: nite{El_Pr0f3_0f_Prec1s10n_Pl4ns}**

**Thought Process**
* Once we got the instance up and running we see a sick looking menu with 4 options. 
* Upon selecting the first option, it encodes whatever we give it to Base64 and returns the value to us.
* All we had to do was decode the Base64 and use it as the input for the 2nd option, after prompting our name as **Bob** (as hinted by the challenge description).
* Once we do that, we're given the vault code `G0t_Th3_G0ld_B3rl1nale`.

* Upon selecting option 3 and inputting the vault code in, we're given the flag.


![Pasted image 20241216004932](https://github.com/user-attachments/assets/a6c10ff6-d573-4949-ad91-a55f18bfd10a)

**References**
* https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=WWpSbE1HRTRNREkwTWpoallqTTFaalk1WXpCbE9UVXlaRGsyTVRjeVpEWT0

