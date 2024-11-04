# #1 cat-not-the-pet-but-the-command

Learnt the command `cat` . Basically opens the file and kinda reads it out for us. So all the challenge is asking us to do is type in `cat flag` as they have mentioned that the flag file is in the root directory.

**pwn.college{I--CQ5ht0Da0U815LCLZGReeR9j.dFzN1QDL4kDN0czW}**

# #2 catting-absolute-paths

Basically exactly the first challenge but this time we're using the absolute path of the flag file which is just `/flag`

**pwn.college{Y2d0yWuGFBQEDJXQyP0e0PfoeHs.dlTM5QDL4kDN0czW}**

# #3 more-catting-practice

This challenge was about finding the flag file without using cd,and it was placed in a random ass directory. The directory was provided at the start so all I did was `cat <directory>` and the flag file opened.

**pwn.college{o_0nh7zsUK1BKI0m3ee_RME55Z_.dBjM5QDL4kDN0czW}**

# #4 grepping-for-a-needle-in-a-haystack

All we had to do was use the grep command and the keyword `pwn.college`(as every flag file starts with it) before typing out the absolute path of the given file.

**pwn.college{IDjGJXVaMvPxR38FCy1VHl0ETqv.ddTM4QDL4kDN0czW}**

# #5 listing-files

Introduction to the `ls` command which basically list out all the files in a given directory. All we had to do was list out the files in `/challenge` and the obviously renamed file was the run file which contained the flag.

**pwn.college{YF5llCHXn2n0DizuE9-fwp2OB70.dhjM4QDL4kDN0czW}**

# #6 touching-files

Basically creating new files, and the format is `touch <filename>`. Followed instructions and created the two prompted files and ran `challenge/run` to finish the challenge.

**pwn.college{4p_-MD08AAmPaY3mNiCWrzA1I8u.dBzM4QDL4kDN0czW}**

# #7 removing-files


Simple deletion of a file, in the format `rm <filename>`.
Challenge was to delete a file and run `/challenge/check`.

**pwn.college{Y86SiE2n6ucaa1a1zP6x473kF0X.dZTOwUDL4kDN0czW}**

# #8 hidden-files

Expansion of the `ls` command by following it up with a `-a` to display all the "hidden" files in that directory. Challenge involved using the `ls -a` command in the directory `/.` where we see a file that has the word "flag" in it. All we have to do next is use the `cat` command on that file to view the flag.

**pwn.college{k4UtREKbRZ12npHlPer3HOAb9Yg.dBTN4QDL4kDN0czW}**

# #9 an-epic-filesystem-quest

An extremely lengthy chase. Started off in the `/.` directory and used the `ls` command to look around. Used `cat` command to read the clue file. 

Now the real quest begins. The instructions tell us not to use the `cd`. How i bypassed this was to simple use `ls -a <directory-that-it-directed-us-to>` to see the contents of the directory we weren't allowed to `cd` to. 

It kept directing you to different directories, with a mix of being allowed to `cd` and not being allowed. After what seemed like the lifespan of my grandmother, it finally led to the flag file which could simply be read by using `cat`.

**pwn.college{IZ0S5gYJXfMROKpjFF_PRAGl6RH.dljM4QDL4kDN0czW}**

# #10 making-directories

Literally just making a new directory, the syntax is `mkdir <directory-name>`. Challenge involved making a directory and then a file inside it.

**pwn.college{E1mBz16KCVFZEt56y38teW_l3F0.dFzM4QDL4kDN0czW}**


# #11 finding-files

First look into the terminal version of CTRL+F. Challenge involved using the `find` command to look for the flag file. All one has to do is type in `find / -name flag` in the root directory and it'll show a bunch of denied permissions and then a bunch of files at the bottom that you have to trail-and-error to see which one contains the flag file(using the `cat` command, obviously).

**pwn.college{4gMS5A5Va6-M8RHRfObhtreslC4.dJzM4QDL4kDN0czW}**

# #11 linking-fles

For this challenge, we had a file `/challenge/catflag` that was linked to  `/home/hacker/not-the-flag`. But neither of those files contained the flag.

 For this one, I first deleted the file `/not-the-flag`, then created a symlink from `/home/hacker/not-the-flag` to `/flag` (which is where the flag actually is). 
 
 Now all I had to do was go to `challenge/catflag` which lead to `/home/hacker/not-the-flag` which inturn led to `/flag`.
 
 **pwn.college{cxMrExVgT69fS2XpITkDWz67k3C.dlTM1UDL4kDN0czW}**
 
 