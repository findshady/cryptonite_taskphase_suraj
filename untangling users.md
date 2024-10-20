# #1 becoming-root-with-su

  

This challenge tells us more about users in linux. For now, we're looking at the command `su` (switch user). Now, for this challenge, to allow the user to elevate privileges to root, we need to enter the `su` command and then it asks for a password.

  

In the challenge,we're given the password as "hack-the-planet". Once we enter that, all we have to do is `cat /flag`.

  

**pwn.college{UQakAWTLfw7aH6IF3DAuAckptSm.dVTN0UDL4kDN0czW}**

  

# #2 other-users-with-su

  

Well here, we start a root shell using `su` followed by an argument, which in this case is a username: `zardus`. After that we just run `/challenge/run` to get the flag.

  

**pwn.college{cbYh4wAf00fxvGsZgjezDwgV6JJ.dZTN0UDL4kDN0czW}**

  

  

# #3 cracking-passwords

  

We're told that the password is cross-checked against the database `/etc/passwd`, which were moved to `/etc/shadow`. Further reading tell us the basics of how to access a leaked `/etc/shadow` file to get a password. Also learned about the `john` command. So the get the password of a user called **Zardus**, we're going to be working with a leaked `/etc/shadow` file that is already provided to us. 

Our first command is 
```
john /challenge/shadow-leak
```

and then it tells us to enter basically any key (except "Q") and then it gives us a list. However, a better and more user friendly output is achieved by using the command-

```
john /challenge/shadow-leak --show
```

where we see the following lines: 
```
hacker:NO PASSWORD:20000:0:99999:7:::
zardus:aardvark:20014:0:99999:7:::
```

We can clearly see that the password for the user **Zardus** is `aardvark` . To access the bash as the user **Zardus**, we now use the command 
```
su Zardus
```

where it asks us for the password (that we just cracked). Once entered, we just have to run `/challenge/run` to get the flag.

**pwn.college{IRYfEJwXmQQZcoAHgL8l4IKajin.ddTN0UDL4kDN0czW}**


# #4 using-sudo

We now learn the concept of `sudo` (superuser do). The only major difference is that `sudo` defaults to running a command as root, instead of defaulting to launching it's own specified user shell.

For this last challenge, we are given `sudo` access and we're supposed to read the flag, which is usually in `/flag` .  Our one and only command is:

```
sudo cat /flag
```

**pwn.college{s5Z8QWYKEe_L4xmFFeNWDNEauj_.dhTN0UDL4kDN0czW}**

