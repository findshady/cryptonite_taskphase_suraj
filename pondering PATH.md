# #1 the-path-variable

Firstly, we learn how to disrupt the operation of the `/challenge/run` program. We NEED to find the `rm` command or else the flag will be deleted. To start off, we do 

```bash
PATH="/run/challenge/bin"
```

Now all we have to do is execute it using 

```bash 
/challenge/run
```

**pwn.college{sS0uNQElGUnbEazq2YtHtRPuiNc.dZzNwUDL4kDN0czW}**

# #2 setting-path

We just have to run commands using their bare names. We do this by adding/replacing directories in the list.  First, we shall write a command using `PATH` to include the folder `more_commands`. The command is 
```bash
PATH="/challenge/more_commands"
```

and then run it using `/challenge/run`.

**pwn.college{kZ16hGPnzJu6FBK7LpZMXU2UdWx.dVzNyUDL4kDN0czW}**

# #3 adding-commands

To solve this challenge, we need to create a file called `win`. We just use `touch win`, then we read the contents of `/flag` into the file `win` using
```bash
echo "read /flag" > win
```

Now, we change the permissions to make it into an executable file

```bash
chmod u=rwx win
```

Now, all we have to do is add the default home path to the file `PATH` by using 

```bash
PATH='~'
```

Finally, we run this using `/challenge/run`


# #4 hijacking-commands


