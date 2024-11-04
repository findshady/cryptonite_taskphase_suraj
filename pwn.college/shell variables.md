# #1 printing-variables

We learnt how to print variables out by using `echo` followed by the variable name in the format `echo $<variable-name>`. Since we were told that the flag was in a variable called "FLAG", our command was `echo $FLAG`

**pwn.college{QP95vzY3FMismqs3aN7Z-vByvAH.ddTN1QDL4kDN0czW}**

# #2 setting-variables

After learning that `$` is only used when *accessing* a variable, we're now asked to set the variable "PWN" to the value *college*. We can do that by doing `PWN=COLLEGE`, which then spits out the flag.

**pwn.college{MFIyFA_uwohRoJzbXFjJlWU5F_M.dlTN1QDL4kDN0czW}** 
 
  # #3 multi-word variables

Now, we're learning how to assign a variable that contains a space because if we just try to use `variable=no space`, it only assigns the variable *variable* to the word *no* and the word *space* is being considered as a command. 

For this challenge, we're told that we need to set the variable *PWN* to the words *COLLEGE YEAH*. We shall do this by simply inputting the command `PWN="COLLEGE YEAH"`.

**pwn.college{ISYU0XYj37PtAFinTJ3NeNIF-xM.dBjN1QDL4kDN0czW}** 


# #4 EXPORTING-variables 

Here we're supposed to invoke `/challenge/run` so first we assign the value *COLLEGE* to the variable *PWN*, BUT we're supposed to export it so our command is gonna be `export PWN=COLLEGE`. Next we're told that we need to set the value *PWN* to the variable *COLLEGE*. This time our command would be `COLLEGE=PWN`. Finally, as instructed, we run `/challenge/run` to claim our flag

**pwn.college{0IvizyewW6botc5OC8UV__yQpSG.dJjN1QDL4kDN0czW}** 


# #5 printing-exported-variables

We learnt an alternative to the `echo` command. The command we're gonna use for this challenge is `env`. As soon as we type the command out, we get a load of every exported variable and one of which is the flag.

**pwn.college{4WXS9gbycdkdPkyKfhn89VfWwvD.dhTN1QDL4kDN0czW}** 

# #6 storing-command-output

In this challenge, we're taught to read out the output of a command and imprint it onto a variable. For this, we had to read out the command `/challenge/run` into the variable `PWN`, so our final command was `PWN=$(/challenge/run)` or we could have also used `PWN=(backtick)/challenge/run(backtick)`.

**pwn.college{4UDBReix_YEdmDPv8H9ZwBsS_GW.dVzN0UDL4kDN0czW}**

# #7 reading-input

Here we're taught how to take an input from a user, which is using the `read -p` command where `-p` is an argument which lets us specify a prompt. Now,we're told to take an input `COLLEGE` from the user and put it into a variable `PWN`. 

This can be done using the command `read -p "input= " PWN`. Which will prompt the user to enter an input, in which we shall enter the required value, i.e; `COLLEGE`.

**pwn.college{wlAAduqc1IEOcyEheqgndU1dc0u.dhzN1QDL4kDN0czW}**

# #8 reading-files

Learned that we can read files with the shell.
This is like how we executed user input into a variable AND redirected files into command input at the same time.

Since we had to read `/challenge/read_me` INTO the variable PWN, our simple one line command was `read PWN < /challenge/read_me` 

**pwn.college{IfubwLNF3_vaTUzuFWajE_qd71x.dBjM4QDL4kDN0czW}**