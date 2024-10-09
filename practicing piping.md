# #1 redirecting-output

 Learnt the use of `>`, which is just assigning the output of whatever comes before it to the file that is inputted after it.
 
 The one in our case was `echo PWN > COLLEGE`
 which just inputted the word "PWN" to the file `COLLEGE`.
 
**pwn.college{EQYXImWQC1D6g5vsLhjVWCcmroh.dRjN1QDL4kDN0czW}**

# #2 REDIRECTING-MORE-OUTPUT

The flag is to be redirected to a file named `myflag`. Argument will be `/challenge/run > myflag`, simple as that. After that we just read the flag file using `cat myflag`.

**pwn.college{4_ROD3YhgTIAbZ5QsslAJeEvQeN.dVjN1QDL4kDN0czW}**

# #3 appending-output

Learnt that we can `append` a bunch of files into the same output file. Int the challenge, all we had to do was redirect the output of the flag to another file but append it so that the first half will just be read to the file but the second, which is now appended to the first and which would've otherwise not been printed(as the second half is written to stdout and stdout is now redirected to the file), will now be printed. 

**pwn.college{4fKTBra5IYYTYlRH2p92B8B4xsd.ddDM5QDL4kDN0czW}**

# #4 redirecting errors

Pretty much the same as before, but with we learnt that we can send the log file of the errors to another file if we want. In this challenge, we had to do just that, the argument was just `/challege/run 1> myflag 2>instructions`
where the extra numbers from the last challenge just signify the following: 

0: Standard Input
1: Standard Output
2: Standard Error

# #5 redirecting-input

Firstly, we `echo` the word "COLLEGE" into a file named "PWN". Now we just redirect the contents of PWN to `/challenge/run` by using `/challenge/run < PWN`.

**pwn.college{c4OUnGx_o6stAN87LyEVOoQqMwX.dBzN1QDL4kDN0czW}**

# #6 grepping-stored-results

Well for this one, we just had to redirect the output of `/challenge/run` to `/tmp/data.txt` using the command `/challenge/run > /tmp/data.txt`.

Then, to look for the flag, we just had to `grep pwn.college /tmp/data.txt`.


**pwn.college{4Rs6edjkHOWym7S6mB428QP3Cxd.dhTM4QDL4kDN0czW}**

# #7 grepping-live-output

We were introduced to the pipe operator (nice name, i must say). Challenge required the output to be the flag in a shit ton of lines of text, all we had to do was hit it with a simple pipe operator along with our usual grep command. 

`/challenge/run pwn.college-pwn.college | grep pwn.college`

**pwn.college{gxxq9PC6PI7GVhBMW5vhG2T4H6H.dlTM4QDL4kDN0czW}**

# #8 grepping-errors

Mainly what i understood from this was that we need to direct one file descriptor to another. 

In very simple terms,we gotta combine stderr and stdout by using the command `/challenge/run 2>&1`
that we use a pipe command(which usually pipes out only stdout to the next), but since this is combined, we can use a grep command.

The final command is `/challenge/run 2>&1 | grep pwn.college`

**pwn.college{gSzOHviUy7hmxo6ZJWu4mThsArD.dVDM5QDL4kDN0czW}**

# #9 duplicating-piped-data-with-tee

In this challenge, we need to intercept the output of `/chjallenge/challenge` and read the contents using the `cat` command. The first command we are going to use is `/challenge/pwn | tee challenge | /challenge/college`. Upon reading the contents of 'challenge' using `cat challenge`, it gives us a secret code and tells us the format to use it. Once we have succesfully used the secret code in the right format, it gives us the flag.

**pwn.college{kqKFODx2KvYHiaZb6YiKYYDVvRi.dFjM5QDL4kDN0czW}**

# #10 writing-to-multiple-programs

Since we're supposed to use `tee` to process two substitutions, the simple command would be ` /challenge/hack | tee >(/challenge/the) >(/challenge/planet)`.

# #11 
