# #1 chaining-with-semicolons

We learn that we can chain a bunch of commands instead of running them one after the other if we know what we have to do.  For this challenge, we had to chain the commands `/challenge/pwn` and `/challenge/college`.

Our final command is 
```bash
/challenge/pwn ; /challenge/college
```

**pwn.college{IbaK9J5dWD-LvEx6FMor6VdMHN1.dVTN4QDL4kDN0czW}**

# #2 your-first-shell-script

Now we're learning how to create a shell script. First we shall create a bash file using the command `touch x.sh`.

Now, since we want the output flag to be in `x.sh` and to do so, we must simultaneously run both commands given to us i.e. `/challenge/pwn` and `/challenge/college`, we're gonna use semi-colons to separate them. Our command now is 

```bash
echo "/challenge/pwn ; /challenge/college" > x.sh
```

Now, as directed, we just run the command 
```bash
bash x.sh
```

and it outputs the flag.

**pwn.college{EmhVwAzS5WemiRYrU4ZiBPUTT31.dFzN4QDL4kDN0czW}**

# #3 redirecting-script-output

For this challenge, we need to create a script just like the previous one. So we're gonna use `touch x.sh` again.

Next, as directed, we call both commands together by using `/challenge/pwn ; /challenge/college` and `echo` it into `x.sh` using the command 
```bash
echo "/challenge/pwn ; /challenge/college" > x.sh
```

(exactly like our previous chall)

Now, we execute the script `x.sh` using `bash x.sh` and pipe the output to `/challenge/solve`. The final command will be:

```bash
bash x.sh | /challenge/solve
```

# #4 executable-shell-scripts

We learnt that we don't even need to use the `bash` command if our shell script file is *executable*.  For this we need to create a bash file `x.sh` (same steps as the last 2 challs sigh). 

Now, first, we echo the contents of the command `/challenge/solve` to `x.sh` using the commands: 

```bash
echo "/challenge/solve" > x.sh
```

Now, like I said in the beginning, it needs to be an executable file, so for this we need to change the permissions, we do this by using the command:

```bash
chmod u=rwx x.sh
```

Now, all we have to do is execute it is `./x.sh`.

**pwn.college{g3QuinJFGI2QXCCZVzgFBrSDhb_.dRzNyUDL4kDN0czW}**