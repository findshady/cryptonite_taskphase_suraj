# #1 learning-from-documentation

Now, we're learning that we can add arguments after a command as simple as fetching the challenge file. In this case, the argument was `--getflag`, to get the flag (duh). All we had to do was type in the command `/challenge/challenge` followed by the argument `--getflag`

**pwn.college{0GVMg8BAYfZoUCTi5BJOS7WbITE.dRjM5QDL4kDN0czW}**

# #2 learning-complex-usage

In this challenge, we had to read out the documentation for the `/challenge` file, which told us that the argument for this one was `--printfile` and the argument for that argument was the path of the flag file which is literally just `/flag`. 

Hence, the commands were just `/challenge/challenge --printfile /flag`.

**pwn.college{oZOkvDGmPMuHUNuU1UCrh4q8YNX.dVjM5QDL4kDN0czW}**

# #3 reading-manuals

Firstly, we open the manual for the `challenge` file by inputting `man challenge`.

Then, by following instructions, we see that we must use the argument `--svxelj NUM` where NUM has to be 720 in order for the flag to be printed.

Hence,the final commands will be `/challenge/challenge --svxelj 720`.

**pwn.college{sv7URPxeS2lNKAjg06SAACCKRmO.dRTM4QDL4kDN0czW}**

# #4 searchin-manual

This time, we invoke the manual using `man challenge` and then use the command `/flag` to search for any file that has the flag in it. Then, navigate thru them by using arrow keys. One such argument will say "run this to find the flag".

Now all we have to do is `/challenge/challenge --<argument>`


**pwn.college{w7xsefc_EQmhTa2cWu2JTmGGfjf.dVTM4QDL4kDN0czW}**

# #5 searching-for-manuals

Since the name of the file that we have to `man` isn't given to us, I used `man man` and since our flag comes under section 5, and the synopsis to search for the word flag was ` man -K [man options] [section] term ...`;

 All i had to do was input `man 5 flag`
 where:
 5: The section.
 flag: the word to be searched in man.
 
 Then it printed out a bunch of commands among which one was
 
  "cszwadnjfc (1)       - print the flag!"
 
Our next command is `man cszwadnjfc` as "cszwadnjfc" is the name of the unknown file that wasn't given to us. After this, we just follow the steps from challenge #4 (mentioned above) and get the flag.

**pwn.college{cFszUZwAaJ5dUMS1JF5VnFjfSca.dZTM4QDL4kDN0czW}**

# #6 helpful-programs


Firstly, to use the `--help` argument, the command is `/challenge/challenge --help`.   Then it shows a bunch of optional arguments where we get the secret value first by using `challenge/challenge -p`.

Then, we simply use the command `/challenge/challenge -g 228` whcih gives the flag as the output.

**pwn.college{EZtGrMe2rL2P8CKWjUbK0Mlc3y1.ddjM4QDL4kDN0czW}**

# #7 help-for-builtins

This time, `challenge` is a builtin. So all we have to do is use the command `help challenge`, where it gives you a bunch of options.

First it gives you a secret value, which is to be used after the `challenge --secret` command. That's enough to give you the flag this time.

**pwn.college{AtTsVn94lyq32i8XgFrxB_OHahj.dRTM5QDL4kDN0czW}**