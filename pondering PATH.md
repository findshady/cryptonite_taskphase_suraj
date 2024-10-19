# #1 the-path-variable

Firstly, we learn how to disrupt the operation of the `/challenge/run` program.  We can either find where the `rm` file is using `which rm` or simply just clear the entire variable, making it impossible for `/challenge/run` to find the program. In order to clear the entire thing, use this command:

```bash
PATH=""
```

Now all we have to do is execute it using our good old: 

```bash 
/challenge/run
```

**pwn.college{sS0uNQElGUnbEazq2YtHtRPuiNc.dZzNwUDL4kDN0czW}**

# #2 setting-path

Basically, here we learn that we don't explicitly have to type out the entire path address in order to launch a program like we did before. Big day for people who hate typing. The way to launch them using their bare names is to add/replace directories in the list `PATH`. 

In the given challenge, we're told that our childhood friend `/challenge/run` will run the `win` command with it's bare name and we need to help it do that. The only catch is that the command `win` exists in the directory `/challenge/more_commands`. 

Now all we have to do is overwrite `PATH` with the directory `/challenge/more_commands`. We can do that using the command: 
```bash
PATH="challenge/more_commands`"
```

Now that we have successfully overwritten `PATH`, we're officially on first name basis with the command `run`.

Now all that's left is the heinous task of typing out 
```bash
/challenge/run
```


**pwn.college{kZ16hGPnzJu6FBK7LpZMXU2UdWx.dVzNyUDL4kDN0czW}**

# #3 adding-commands

Let's understand the challenge first: Unlike the previous challenge, the file `win` does not exist. So we need to make our own. We shall do that using the freaky aah command  `touch win`. Now, we use open the `win` file using the text editor ***nano***. 

```bash
nano win
```

We need to specify the interpreter the command we want to put it inside of `win`. Also, we need the `win` file to perform the `cat` operation. The commands are as follows: 

```bash
#!/bin/bash

cat /flag
```

`#!` at the beginning is known as a "shebang" or "hasbang" (more freaky names hell yeah). Basically, it specifies the interpreter (such as bash, in this case) to run the script. 

But this isn't enough, we're supposed to give it the permission to be executed by using the command 

```bash
chmod +x win
```

Next, we need to adjust the variable `PATH` so that `/challenge/run` can find `win`. We do this by using the command 

```bash
export PATH="/home/hacker:$PATH"
```

The above command specifies that the address `/home/hacker`(which is where our `win` file is) is being accessible to `/challenge/run` because when we run `/challenge/run` by itself, `win` is the ***only*** command that it needs.

Finally, we can use `/challenge/run` to get the flag.

**pwn.college{orkFHZ0XoKrzl0AOjHK0FwwCOI4.dZzNyUDL4kDN0czW}**

# #4 hijacking-commands

This time, just like the first challenge in this module, challenge will delete the flag using the devious command `rm`.  The basic approach I'm using to solve this challenge is that we can create another file `rm` and make `/challenge/run` invoke THAT instead of the original `rm`. In this quest to become the master of delusion, we first start by `touch`ing the imposter `rm` file:

```bash
touch rm
```

Now, we need to give it a script to make it output the flag as it is mentioned in the challenge that the lazy `/challenge/run` will **not** do it this time. Hence, we open the file using `nano` and then use the same commands as the previous challenge: 

```bash
#!/bin/bash

cat /flag
```

and don't forget to give this pesky lil mf executable permissions too

```bash
chmod +x /home/hacker/rm
```

Now, we need to set the `PATH` variable to the directory where our imposter `rm` file exists, which is `/home/hacker`.  This is done using the command: 

```bash
PATH="/home/hacker":$PATH
```

Finally, we can check if our imposter did it's job by running `/challenge/run` one final time for this module. This time, it looks for the file `rm` but instead it executes our imposter `rm`, because we've set that as the path. Now that our `rm` file is being executed, it invokes our `cat /flag` command and displays the flag.

**pwn.college{IWMyzBMAdT0t9BmpoEDdZZHsXFe.ddzNyUDL4kDN0czW}**

