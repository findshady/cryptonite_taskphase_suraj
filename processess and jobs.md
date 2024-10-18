# #1 listing-processes

In this challenge we learned about how to list process and make them user-readable using arguments such as `-ef` where the `e` is to list every process and the `f` is for everything to be in its full format to make the experience better for the user. 

The command that I am going to use for this challenge is `ps aux` where `ps` is just to list the current processes and `aux` is a special command that combines the elements of 3 different commands, 

`**a**: lists processes for all users.
**u**: makes the output user-readable.
**x**: lists process that aren't running in a terminal.`

So to get the flag, all I did was to enter the command `ps aux` and look for a funny named file which i found and when opened, gave me the flag.

**pwn.college{AyZBXLTjD8ZiDmHMEADD7CBdfke.dhzM4QDL4kDN0czW}**

# #2 killing-processes


Getting violent with processes now, we use the command `kill`. To get the flag, we must terminate the process `/challenge/dont_run`. In order to find it, we list the running process in our desired manner by using the command we did in the previous challenge, `ps aux`, where all we have to do now is find the `/challenge/dont_run` file, note it's **PID** and enter the command `kill <PID>`.

**pwn.college{oLxe2vLiE8oxATn-0KhPnT3M8Hc.dJDN4QDL4kDN0czW}**

# #3 interrupting-processes

Now if we don't want to get too aggressive, we can choose to merely interrupt a process instead of straight up killing it. First, as instructed, we enter the command `/challenge/run` and then it instructs us to input the combination of keys `CTRL+C` or in a terminal format, `^C`. This straight up spits out the flag.

**pwn.college{sKC6J5NVQkB-KwsMV7boqOqh9ZD.dNDN4QDL4kDN0czW}**  


# #4 suspending-processes

Well now we're learning how to **suspend** processes in the background using the key combo `CTRL+Z`.

Now, this challenge requires us to run two instances of `/challenge/run`, which can be achieved by simply suspending the first one and then running the second. The commands to achieve this were just `/challenge/run` followed by the keyboard combo `CTRL+Z` followed by another `challenge/run`.

**pwn.college{4mWGJokKjUXaVBmYAlYxTEr0qcW.dVDN4QDL4kDN0czW}**

# #5 resuming-processes

Once the post suspend clarity hits, we can wash our sins by using the `fg` command. To win the flag this time, we must run our age old command `/challenge/run` and then suspend it using `^Z` and then using our newly learned command `fg /challenge/run` which spits out the flag. 

**pwn.college{4rDp6Yg7Tk3Y1I93Kun8cbTNOxk.dZDN4QDL4kDN0czW}**

# #6 backgrounding-processes

This challenge involves us running `/challenge/run`, suspending it by using `^Z` and then bring it back but in the **background** this time by using `bg /challenge/run`. Now all we have to do is run `challenge/run` again to get our flag.

**pwn.college{kkAIjpD0QWWrc0mwCkI41Bpvhri.ddDN4QDL4kDN0czW}
**
# #7 foregrounding-processes

Like the title says, we can bring processes that are in the background to the foreground just by using a simple `fg` followed by the process name. This level included us bringing `/challenge/run` to the foreground AFTER we resumed it in the background (the process has been explained in my previous challenge write-up).

**pwn.college{glQ5M0ue_eXPCtpUC_nZs3FET06.dhDN4QDL4kDN0czW}**

# #8 starting-backgrounded-processes

Now, one of the only things that's left is to learn how to start a process backgrounded. Yep, turns out you didn't have to kill the poor thing first. All we were missing was a `&` in the command. Our one line command for this challenge is `/challenge/run &`.

**pwn.college{MV8pmJlG33aFzNbGwBtAKRfMbo1.dlDN4QDL4kDN0czW}**

# #9 process-exit-codes

Now, we need to access exit codes of a program in order to complete this challenge. This can be done by using the variable `?` which we're using in combination with the `$` variable (which helps us read the value, duh).

 So we're supposed to read the exit code of the program `/challenge/get-code` by using the commands `/challenge/get-code $?` which gives us an output that says "Exiting with an error code!". Now, we read the exit code by using the command `echo $?` and then use the code as an argument with our next command, i.e; `/challenge/submit code`, so our command was `/challenge/submit-code <exit code>`.
 
**pwn.college{oqjfosFaJkFN2JqMTqiFB3uuP5T.dljN4UDL4kDN0czW}**