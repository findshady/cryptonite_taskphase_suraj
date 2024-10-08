# #1 The ROot

Learnt about the root directory and how to invoke it. That it's called an absolute path. Just typed in `/pwn` to enter the pwn directory in the root.

**pwn.college{oilyZc-F1N45IlyEgkGlbqAvizk.dhzN5QDL4kDN0czW}**

# #2 Program and absolute paths

Learnt about absolute paths. Typed in ``` /challenge/run ```, which is the absolute path to get the flag. 


**pwn.college{ku6pQ1VIFX4S12Wfu6mUUqXZjeU.dVDN1QDL4kDN0czW}**

# #3 Position thy self

Learnt the significance of `~`. 
First type in `challenge/run` where it wills show you an error and tell you that you're not in the required directory.

Copy the path (`/etc/apt/sources.list.d` in this case) and cd into it.

Now that we're in the right directory, we can invoke the flag file using our good old `/challenge/run`.

**pwn.college{MlLDpDLn9kPjXJtnGmlxJzZxl-P.dZDN1QDL4kDN0czW}**

# #4 Position Elsewhere, #5 Position Yet Elsewhere 

Same as #3 with a different path.

**#4:** **pwn.college{Y5NTyWb0hzHDoDYPFuzXZhoZ9Tq.ddDN1QDL4kDN0czW}**

**#5**  **pwn.college{wjc1m-JgH0pNnRcvIbk1LKgsB-t.dhDN1QDL4kDN0czW}**

# #6 Implicit relative paths, from /

Since our root directory is `/`.

Lets go to the root directory by entering `cd /`.

Invoking the flag file by typing in `challenge/run` did the trick. 

Learnt about relative paths.

**pwn.college{Egh_6L0dkYxiVpPDo0IwOygChhf.dlDN1QDL4kDN0czW}**

# #7 Explicit Relative paths, from /
Since our root directory is `/`.

Lets go to the root directory by entering `cd /`

Since our current working path is `/`, our relative path is the same as our absolute path. Now, Invoke the flag file by using a `./` followed by `challenge/run`. The `./` basically refers to the same directory.

**pwn.college{Ar9rd5VTrsI9_3Bzwp1EP1P0MtI.dBTN1QDL4kDN0czW}**

# #8 implicit relative path
Let us enter the Challenge directory by putting in the command `cd /challenge`.

Now, since we can't just type in `/run` after our previous command, all have to do is **specify** that we want to run that specific program, the way to do that is adding a `./` before it. Thus our second command will be `./run`

**pwn.college{8q4Ve4oNvfIcJ4Rlh_ktsIDaNr3.dFTN1QDL4kDN0czW}**

# #9 Home sweet home

Learnt that `~` is basically short for `/home/hacker`. 

Firstly, type in `/challenge/run` to invoke the directory where the flag is present. Now we must provide an argument. Like the Challenge suggests, the argument must be an absolute path inside the home directory (which is invoked using `~/`).

Since the challenge specified that the argument is supposed to be only 3 characters long, and since two of our characters are crucial, I used `~/a` and it wrote the flag file there.

**pwn.college{Mj8-b4dkfaAr3D-46r3z2b2crUq.dNzM4QDL4kDN0czW}**

