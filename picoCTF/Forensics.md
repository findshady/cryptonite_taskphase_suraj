
# tunn3l v1s10n

**Flag:** `picoCTF{qu1t3_a_v13w_2020}`

**Thought Process**

* Since the file that is given to us has no extension, I put it in on https://hexed.it/ and looked up it's header file to find out that the file given is a `.bmp`or a **bitmap** file. So i renamed it to add the extension to the end.
* Since I had done a similar challenge in OASIS CTF, I knew I had to compare it's header to the header of another bitmap file so I did.
* I copied almost exactly what my sample bitmap image's header file.

For reference, here is the header file of the provided file (renamed to `tv.bmp`):

![Pasted image 20241106230908](https://github.com/user-attachments/assets/71cddea2-5cf0-4990-9daa-d39c4b3ef163)


and here is the header file of a sample bitmap image I got off the internet:

![Pasted image 20241106230846](https://github.com/user-attachments/assets/402ce9fc-b051-485b-8f24-89f749a7a82a)


After making some changes and making our header file look similar to the header file of the sample image, we get: 

![Pasted image 20241106230548](https://github.com/user-attachments/assets/44790ea5-e9f4-4c4a-9313-76ea5bfe1e27)


Our `.bmp` file is finally viewable and looks like this

![Pasted image 20241106231531](https://github.com/user-attachments/assets/8f92e26a-3e40-4901-a065-17d595ff4f94)

*tv(1).bmp*

bummer.

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
	***Example:** 
			To input the number 1134, it's hex value is 0x46e.
			but when I input it into the hex editor, I'm supposed to input it in the format `6E 0`


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





