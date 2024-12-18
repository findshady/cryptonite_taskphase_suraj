
# tunn3l v1s10n

**Flag:** `picoCTF{qu1t3_a_v13w_2020}`

**Thought Process**

* Since the file that is given to us has no extension, I put it in on https://hexed.it/ and looked up it's header file to find out that the file given is a `.bmp`or a **bitmap** file. So i renamed it to add the extension to the end.
* Since I had done a similar challenge in **OASIS CTF**, I knew I had to compare it's header to the header of another bitmap file so I did.
* I copied almost exactly what my sample bitmap image's header file.

For reference, here is the header file of the provided file (renamed to `tv.bmp`):

![Pasted image 20241106230908](https://github.com/user-attachments/assets/71cddea2-5cf0-4990-9daa-d39c4b3ef163)


and here is the header file of a sample bitmap image I got off the internet:

![Pasted image 20241106230846](https://github.com/user-attachments/assets/402ce9fc-b051-485b-8f24-89f749a7a82a)


After making some changes and making our header file look similar to the header file of the sample image, we get: 

![Pasted image 20241106231258](https://github.com/user-attachments/assets/24d35915-1e3a-4725-957e-315dd0d5a9dc)


Our `.bmp` file is finally viewable and looks like this

![Pasted image 20241106231531](https://github.com/user-attachments/assets/8f92e26a-3e40-4901-a065-17d595ff4f94)
*tv(1).bmp*

***bummer.***

* Since this image seems to be in a weird resolution, I put myself on a mission to change it's dimensions in order to see if the flag is in one of the corners or somewhere in the actual image itself.

In order to change the dimensions, i ran an `exiftool` command on the image to find out it's current dimensions:

```bash
exiftool tv.bmp
ExifTool Version Number         : 12.40
File Name                       : tv.bmp
Directory                       : .
File Size                       : 2.8 MiB
File Modification Date/Time     : 2024:11:06 23:05:29+05:30
File Access Date/Time           : 2024:11:06 23:15:01+05:30
File Inode Change Date/Time     : 2024:11:06 23:05:29+05:30
File Permissions                : -rwxrwxrwx
File Type                       : BMP
File Type Extension             : bmp
MIME Type                       : image/bmp
BMP Version                     : Unknown (53434)
Image Width                     : 1134
Image Height                    : 306
Planes                          : 1
Bit Depth                       : 24
Compression                     : None
Image Length                    : 2893400
Pixels Per Meter X              : 5669
Pixels Per Meter Y              : 5669
Num Colors                      : Use BitDepth
Num Important Colors            : All
Red Mask                        : 0x27171a23
Green Mask                      : 0x20291b1e
Blue Mask                       : 0x1e212a1d
Alpha Mask                      : 0x311a1d26
Color Space                     : Unknown (,5%()
Rendering Intent                : Unknown (826103054)
Image Size                      : 1134x306
Megapixels                      : 0.347
```

Now I know that the file dimensions are **1134x311**. I also know that in the hex editor, I need to convert the number 1134 to hex, which comes out to be **046E**. Now, we know that in the hex editor, we look for it in the order `6E 04`. 

Furthermore, I learnt that this is at the address `0x00000012`which is designated to storing the image width, and `0x00000016` is the address where image height is stored. Since I saw a weird column on the right side in *tv(1).bmp*, I'm gonna assume the flag is somewhere on top or on the bottom. This now means I need to increase the height of the image. 

To know what value I needed to increase the height to, i knew it needed to be a value way above `306`. I tried a bunch of values but got hit with either corrupted files or weird hazy images. Finally, I found the perfect size to be **850** pixels. This, when converted to hex was **0352**, so we're gonna have to replace the current height with **"5203"**. 

Our new hex header looks like this

![Pasted image 20241106235710](https://github.com/user-attachments/assets/8b13ca56-57e8-470f-85b2-83c4023928a6)


And the image was this

![Pasted image 20241106235751](https://github.com/user-attachments/assets/50c4e1e7-eaa0-4410-88a6-161099745711)

 

**New Things Learnt**

* A LOT about bitmap image files: their headers, what address corresponds to what property, tweaking headers of bitmap image files.

* Dealing with a lot of broken `.bmp` files who's errors couldn't be traced back easily.

* Since I tried to extract the original `tv` file using `binwalk`, it gave me a file with the extension `.sit`. I spent a solid hour doing research on that extension and how to extract files that have it.

* Changing the resolution of a .bmp file.

* Getting comfortable with cryptography related tools such as `exiftool` and `binwalk`. 

* That when inputting hex data into the hex editor, the characters of the hex data must be in a different order.

  ***Example:*** 
			To input the number 1134, it's hex value is 0x46e.
			but when I input it into the hex editor, I'm supposed to input it in the format `6E 04`


**Mistakes Made**

* Initially mistook this challenge for a challenge that had a file embedded inside a file, spent a ton of time trying to extract a file that doesn't exist with a weird extension (`.sat`) I had never heard of. 
```bash
binwalk -e tv.bmp
```


* Messed up a lot while trying to change the dimensions of the `.bmp` file. Any small change made would give me a corrupted file.

* Finding the correct height (850 pixels in my case) took me a lot of time, there must be a shorter way to go about this, and I will look into it. 

* In my final header, I had to change the 2 existing input values from a hexadecimal value to decimal values, because for some reason hexadecimal values gave me corrupted files.

**Resources**

* https://stackoverflow.com/questions/1973903/reading-and-parsing-the-width-height-property-of-a-bitmap
* https://www.rapidtables.com/convert/number/binary-to-hex.html

# m00nwalk

**Flag: `picoCTF{beep_boop_im_in_space}`**

**Thought Process**

* Looked up the hint and it was related to SSTV tech for which i tried looking for decoders online but i could barely find any.
* Ended up downloading a mobile app called [Robot36](https://play.google.com/store/apps/details?id=xdsopl.robot36&hl=en_IN). 
* Used `Scotty 1` as the other hint suggested.
* Placed my headphones to my phone's mic and it gave me this .png file.

![WhatsApp Image 2024-12-03 at 16 42 55_f18be0c0](https://github.com/user-attachments/assets/94feb516-ac30-400b-843e-e79e9f49d0bd)

**New Things Learnt**

* SSTV decoding


#  Trivial Flag Transfer Protocol

**Flag:`picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`**

**Thought Process**

* Since the given file was of the format .pcapng and I had dealt with this in an earlier challenge, I already had Wireshark installed.
* I opened the file in Wireshark and came across this-

![image](https://github.com/user-attachments/assets/5a34cc5c-94d3-4062-9271-b7a08f9a3652)

* After doing some online research on how to go about such challenges, one of the tips online recommended downloading objects by going to File->Export Objects->TFTP(which I assumed as the majority of the file protocols were TFTP).

![image](https://github.com/user-attachments/assets/5d0030f0-398b-4253-b173-12dd316cef1f)

* After downloading the files, we're met with 3 pictures in .bmp format, an instructions .txt file, and 2 other files. Let's start with the contents of the other file, which when combined, became cipher text that was `IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS` when decoded [ROT-13]. The instructions.txt file also had cipher text inside of it that translated to `TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN`. 
* This made it more than clear that we're supposed to look thru the photos to find our flag.

* After reading the hint, I submitted the photos in [Aperi'Solve](https://www.aperisolve.com/), but didn't find anything useful for some reason. So I ran `steghide` on it and it asked for a passkey, which I guessed would be `DUEDILIGENCE` based off the decoded cipher texts.

* It then generated a file flag.txt that contained the flag.


#  Matryoshka doll

**Flag: `picoCTF{ac0072c423ee13bfc0b166af72e25b61}`**

**Thought Process**

* We're given a .jpg file that I used `binwalk `on to find a folder with a file named `2_c.jpg`.
* I used `binwalk` on that and then it gave me a file named `3_c.jpg`
* Used `binwalk` on that which gave me a `4_c.jpg` along with a `flag.txt` that contained the flag.

# St3g0

**Flag: `picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}`**

**Thought Process**
* Uploaded the image to https://www.aperisolve.com/ and saw this

![Pasted image 20241217141140](https://github.com/user-attachments/assets/7c7c7322-9e25-4fc1-93f1-40681e42dc13)

# hideme

**Flag: `picoCTF{Hiddinng_An_Imag3_within_@n_ima9e_96539bea}`**

**Thought Process**
* Literally just binwalk again lol.


# PcapPoisoning

**Flag: `picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}`**

**Thought Process**

* Opened the .pcap file in WireShark and instantly came across this

![Pasted image 20241217173355](https://github.com/user-attachments/assets/beaa431c-4955-451a-a32a-761dde9116e4)

* I knew the flag had to be in one of the packets, So i used **Ctrl+F** and searched for the string "pico" in **Packet Bytes** :

![Pasted image 20241217173818](https://github.com/user-attachments/assets/583846c2-6af6-4549-859b-461501d0ffee)

**Alternative Ways**
* There had to be a way to do this using just the terminal, so I thought of using the **strings** command to grep the flag, and it worked.

```bash
strings trace.pcap | grep "picoCTF"
```

![Pasted image 20241217174257](https://github.com/user-attachments/assets/82285cb9-7620-4cca-b2f3-b708f48a2ca2)
**New Things Learnt**
* Learnt that you can grep the contents of Packet bytes of a .pcap file.

# Packets Primer

**Flag: `picoCTF{p4ck37_5h4rk_01b0a0d6}`**

**Thought Process**

* Literally the same process as above, although the flag didn't show up in search as it was spaced out, simply going thru them got me the flag:

![Pasted image 20241217174502](https://github.com/user-attachments/assets/f02d7f97-cce2-4d4c-a938-73624fa2095d)

* Yep this can be grepped using bash just like the previous one, provided you know that the string is spaced out:

![Pasted image 20241217174625](https://github.com/user-attachments/assets/5900ed1a-02c5-4c44-a912-2a1e98663bbb)

# Eavesdrop

**Flag:** `picoCTF{nc_73115_411_0ee7267a}`

**Thought Process**
* Since we have another .pcap file, I tried to look for the flag string but there was nothing.
* What I did see, was a conversation between 2 people who were using it to communicate and transfer a file.
* I then used the command 

```bash
strings flag.pcap
```

and in the results i came across these lines:

![Pasted image 20241217180300](https://github.com/user-attachments/assets/d09ce730-6da5-419b-8795-3d47d625168f)

![Pasted image 20241217180247](https://github.com/user-attachments/assets/caedda0e-a928-4cff-8a05-b4ade09422b9)

* We see two things, a command and a port (9002).
* So I went over to 9002 and found :

![Pasted image 20241217180352](https://github.com/user-attachments/assets/6b7070f5-f8e6-4ae5-8e57-92a415a8b93a)

* The command mentioned a file `file.des3`, so I created a file with the same name and extension and inside the file I put the ASCII string I found above, and then I ran the command on my shell, to get hit with this

![Pasted image 20241217175648](https://github.com/user-attachments/assets/a746ceae-e52f-4446-908f-68e513eb1267)

* I replaced the ASCII string with it's corresponding HEX string, still the same error.

* After spending an hour troubleshooting, I tried using this command instead of merely creating a file named "file.des3" and pasting the HEX string into it.

```bash
xxd -r -p input.txt file.des3
```

where input.txt had my HEX string and file.des3 was an empty file.

* This gave me a new message:

![Pasted image 20241217183219](https://github.com/user-attachments/assets/e8d4ba83-3f35-46da-99a3-4c01fbbd1d52)

and when I checked my folder, `file.txt` had the flag.


**New Things Learnt**
* A better understanding of navigating thru .pcap files.

# endianness-v2

**Flag:**`picoCTF{cert!f1Ed_iNd!4n_s0rrY_3nDian_94cc03f3}`

**Thought Process**

* We have a file with no extension, no context except that it was recovered from a 32-bit system.
* Using the `file` command on it gives us:

```bash
$ file challengefile
challengefile: data
```

* Using the `strings` command was unyielding aswell.

* I even tried running `gdb` on the file and turns out there are no defined functions.

```bash
gef➤  info functions
All defined functions:
```

* Finally ran `exiftool` on it to see

```bash
$ exiftool file
ExifTool Version Number         : 12.40
File Name                       : file
Directory                       : .
File Size                       : 3.3 KiB
File Modification Date/Time     : 2024:12:17 18:36:02+05:30
File Access Date/Time           : 2024:12:17 18:39:39+05:30
File Inode Change Date/Time     : 2024:12:17 18:39:39+05:30
File Permissions                : -rwxrwxrwx
Warning                         : Processing JPEG-like data after unknown 1-byte 
```

* Then, I opened it in https://hexed.it/ and upon reading the first line 

![Pasted image 20241217185949](https://github.com/user-attachments/assets/ef29512c-897c-4ddb-82be-9c7a84206077)

* Upon comparing the header to a list of known header files, I knew this had to be a JFIF file but with a twist.

![Pasted image 20241217190149](https://github.com/user-attachments/assets/a3fe14b8-fc52-427a-bf4c-a80618861a54)


 * As the name suggested, it had to do something with endian-ness, which refers to the order in which data is stored. For example, Let us consider 0x12345678
     * In little endian, it would be stored as follows: (LSB first, as the name suggests):
     78 56 34 12
     * In big endian, it would be stored in the format:
     12 34 56 78

* My first instinct was to convert the file from little endian (we know that since 32-bit files are usually in little endian) to big endian.

There are multiple ways to do this.

1. The easiest way I found to convert a file from Little endian to Big endian was [CyberChef](https://gchq.github.io/CyberChef/#recipe=To_Hex('Space',0)Swap_endianness('Hex',4,true)From_Hex('Auto')Render_Image('Raw')) using the following settings.

![Pasted image 20241217190645](https://github.com/user-attachments/assets/c710217a-d1fb-4493-8e39-4dd95b2ee268)

2. I came across this command 

```bash
hexdump -v -e '1/4 "%08x" '-e' "\n"' input_file | xxd -r -p > output_file
```

where:
* `-v`: Forces `hexdump` to display the full content, including repeating values.
* `-e`: Specifies output format:
* `1/4` refers to reading and displaying in chunks of 4 bytes.
* `"%08x" ensures that each 4 byte chunk is displayed in hexadecimal format, even if it means padding with zeros.
* **-e ' "\n" '**   specifies the endline character to be appended after every 4 byte chunk, so that each 4 byte chunk will be printed in a new line.

* `xxd` paired with `-r` refers to **reversing** a hex-dump (converting a hex-dump to binary).
* `-p` just makes sure the whole hex-dump is considered to be one long string, it ignores any spaces and newline characters.

3. You could write a python script to do the same.


**New Things Learnt**

* Converting a file from little endian to Big Endian.



# FindAndOpen

**Flag:**`picoCTF{R34DING_LOKd_fil56_succ3ss_0f2afb1a}` 

**Thought Process**

* We're given 2 files, a .pcap and a .zip file that is password protected, we're told tto analyze the .pcap file to get the password.

* Upon opening the .pcap file in SurfShark, we see a bunch of messages like:

![Pasted image 20241217193202](https://github.com/user-attachments/assets/af211d77-b4e1-4faf-b23e-0892715ff629)

![Pasted image 20241217193213](https://github.com/user-attachments/assets/896ea86c-4776-46ac-874e-55596216e0c6)

*  Interestingly, in frame #48, we see this Base64 encoded string.

![Pasted image 20241217193320](https://github.com/user-attachments/assets/65658f10-e663-4874-95a3-e4ad0efa9242)

Converting it, we get the following:

![Pasted image 20241217193639](https://github.com/user-attachments/assets/8e158871-8da1-48c4-b287-f821f5a0825d)

`This is the secret: picoCTF{R34DING_LOKd_`

Since we have the first part of the flag, this is probably the password.

Upon entering the password and unzipping the given .zip, we get a `flag` file containing the full flag.

# MSB

**Flag:**`picoCTF{15_y0ur_que57_qu1x071c_0r_h3r01c_24d55bee}`

**Thought Process**

* Upon reading the challenge name, I knew I had to somehow extract the data MSB wise from the given image.
* Upon google searching, the first thing that popped up was a tool called **Stegsolve**.
* Using this, I used the settings set to 

![Pasted image 20241217200409](https://github.com/user-attachments/assets/1eddaa3d-dc71-4642-bf7d-1e54b4403fd1)


 And then clicked on "Save Text" which generated a file with all the strings within the .png, now in order of MSB first.

* After opening the outputted file and using **Ctrl+F** to look for the word "pico", we come across the flag.

![Pasted image 20241217201232](https://github.com/user-attachments/assets/042629c4-4057-43af-b7a6-d26327d56ef6)


# advanced-potion-making

**Flag:**`picoCTF{w1z4rdry}`

**Thought Process**
* We're given a file with no extension. Upon putting this jawn in https://hexed.it/, I notice that the header file is almost like a .png file, So I pull up an example .png file and make the first line of our file exactly like the example .png.

![Pasted image 20241218170323](https://github.com/user-attachments/assets/7f3f8f46-2c2b-444a-845a-cfc9f57aaa09)

* We end up with an image that's just the solid color red.

![Pasted image 20241218170347](https://github.com/user-attachments/assets/67eb8ba1-ab30-4e95-95fd-da891a0d4845)

* I knew the flag had to be in this image, I tried a bunch of steganography websites untill i found [this](https://incoherency.co.uk/image-steganography/#unhide) .


![Pasted image 20241218170926](https://github.com/user-attachments/assets/2a600850-ff9f-4cb6-92a2-4fe2fc1d84b6)



# Lookey here

**Flag:**`picoCTF{gr3p_15_@w3s0m3_2116b979}`


**Thought Process**
* Literally just 

![Pasted image 20241218195200](https://github.com/user-attachments/assets/59bfba09-9a6e-495e-8dc0-e7f5b9fd88c8)


# Sleuthkit Intro

**Flag:**`picoCTF{mm15_f7w!}`

**Thought Process**
* Downloaded the `disk.img` and used the command 
```bash
$ mmls disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)
```

and then connected to the instance 

```bash
$ nc saturn.picoctf.net 59582
What is the size of the Linux partition in the given disk image?
Length in sectors: 0000202752
0000202752
Great work!
picoCTF{mm15_f7w!}
```

**References**
* https://wiki.sleuthkit.org/index.php?title=Mmls


# Sleuthkit Apprentice

**Flag:**`picoCTF{by73_5urf3r_adac6cb4}`

**Thought Process**
* After following the same instructions as the above chall for this file, I come across this:

```bash
 mmls flag.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)
```

* I look up commands to navigate thru the given files and come across the `fls` command. (-o imgoffset)

* Initially, I tried navigating thru the offset at the start of the `Linux Swap` but that didn't work.

```bash
fls flag.img -o 0000206848
Cannot determine file system type
```

* Then I realized I had to look for the sector offset where the file system starts, which is `0000360448`. So modifying my command, I now see

```bash
fls flag.img -o  0000360448
d/d 451:        home
d/d 11: lost+found
d/d 12: boot
d/d 1985:       etc
d/d 1986:       proc
d/d 1987:       dev
d/d 1988:       tmp
d/d 1989:       lib
d/d 1990:       var
d/d 3969:       usr
d/d 3970:       bin
d/d 1991:       sbin
d/d 1992:       media
d/d 1993:       mnt
d/d 1994:       opt
d/d 1995:       root
d/d 1996:       run
d/d 1997:       srv
d/d 1998:       sys
d/d 2358:       swap
V/V 31745:      $OrphanFiles
```

* Then, I noticed the `-r` argument along with the command, which generated like 4 million lines of text, almost crashing my pc.
* I knew the flag file had to somewhere in this so I grepped for "flag".

```bash
fls flag.img -r -o 0000360448 | grep "flag"
++ r/r * 2082(realloc): flag.txt
++ r/r 2371:    flag.uni.txt
```

* To open flag.txt, I used the command 
```bash
fls flag.img -r -o 0000360448 2082 | grep "pico"
Error extracting file from image (ext2fs_dir_open_meta: Error reading directory contents: 2082
)
```

so obviously that didn't work.

* After scanning thru the help page, On the bottom I found the `icat` command that is used to output the contents of a file based on it's inode number 

* Finally after using the command, turns out the flag wasn't in `flag.txt` but rather in `flag.uni.txt`. 

```bash
icat flag.img -o 0000360448 2371
picoCTF{by73_5urf3r_adac6cb4}
```



**New Things Learnt**
* A better understanding of .img files.
* Navigating thru .img files.


**References**
* https://www.sleuthkit.org/sleuthkit/man/fls.html
* https://www.sleuthkit.org/sleuthkit/man/icat.html


# Dear Diary

**Flag:** `picoCTF{1_533_n4m35_80d24b30}`

**Thought Process**
* I do the exact same method as the previous challenge, display the partitions, list the directories:

```bash
$ fls flag.img -o  0001140736
d/d 32513:      home
d/d 11: lost+found
d/d 32385:      boot
d/d 64769:      etc
d/d 32386:      proc
d/d 13: dev
d/d 32387:      tmp
d/d 14: lib
d/d 32388:      var
d/d 21: usr
d/d 32393:      bin
d/d 32395:      sbin
d/d 32539:      media
d/d 203:        mnt
d/d 32543:      opt
d/d 204:        root
d/d 32544:      run
d/d 205:        srv
d/d 32545:      sys
d/d 32530:      swap
V/V 119417:     $OrphanFiles
```


* I then see this 

```bash
icat flag.img -o 0001140736 1842
2
 .�
force-wait.sh4(innocuous-file.txt5�its-all-in-the-name
                                                            �f��
```

* Since we see the mention of a file `innocuous-file.txt`, lets grep for it

```bash
shady@WIN-3CTPHJ9NI00:/mnt/d/downloads/pico$ strings flag.img | grep "innocuous-file.txt"
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
innocuous-file.txt
```

* I then used the command 

```bash
grep "innocuous-file.txt" --text flag.img
```

and it gave me a huge load of data but it had the flags in parts.

![Pasted image 20241218232520](https://github.com/user-attachments/assets/53d9b47e-5c91-4728-88f6-b37aa3a5e4ca)

**Wrong Tangents**
* Misunderstood the challenge and thought I had to download a software (as the hint mentioned that terminals would lead to misdirection when it came to raw binary data), which I did (Autopsy). Tried solving the chall thru that and it was pretty much the same.
