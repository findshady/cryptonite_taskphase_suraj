# #1 changing-file-ownership

As we learn how to give any file the power of the `root` user, this challenge simply involved usage of the `chown` command to change the owner of a certain file. So our final command is just `chown hacker /flag` as we're hacker in this case. Next we just do `cat /flag` to get our flag.

**pwn.college{wOQXDCHVeQ15U64ctOmffUaJB9l.dFTM2QDL4kDN0czW}**

# #2 groups-and-files

Well, just like how users had permissions, so do groups, and groups are just a collection of multiple users. Now, to change the permissions of a file to add group ownership. We shall use the command `chgrp` to do so. So our final command would be `chgrp hacker /flag` followed by a cheeky `cat /flag` to get our flag.

**pwn.college{csD5nl3rru3OpKO8qiNGXFXTeIP.dFzNyUDL4kDN0czW}**

# #3 fun-with-group-names

Now,we're not the user `hacker` anymore, we're in a random group and we're supposed to figure out the name of it so that we can change the permissions to make it possible for us to read the flag.

 We do this by using the command `id` where it lists out the groups that our user is a part of, which in turns gives us our name. Now we just use the command `chgrp <name> /flag` followed by `cat /flag` to get our flag.

**pwn.college{UJgxLIE5YdyOVfMCIEJ_BYygxRs.dJzNyUDL4kDN0czW}**

# #4 changing-permissions

Here we must change the permissions of the `/flag` file to make it such that we (the user `hacker`) can read it. We must do this just by changing the permissions of the `flag` file using `chmod` but this time we can't transfer ownership to us. 

All we have to do is use `chmod go+rwx /flag` where `go+rwx` will be explained below: 

`go` stands for 'group and other'

`rwx` stands for read, write and executable permissions respectively

and the `+` just means we're adding permissions and not removing it.

**pwn.college{s3p1KYCWp05y5zKJI_HLWpgBCjJ.dNzNyUDL4kDN0czW}**

# #5 executable-files

In this straightforward challenge, we're told that we must give ourselves the permission to execute the `/challenge/run` file. This can be done by simply inputting `chmod +x /challenge/run`. 

**pwn.college{EQ38tsZuWWxQxGgfCHElQRPnFlm.dJTM2QDL4kDN0czW}**

# #6 permission-tweaking-practice

Alright so finally one that makes us work a lil. This one just gives us a bunch of requirements for permissions, while showing us the current one. We just keep chasing permissions till we're done with all 8 rounds of them. I shall attach an example for one of such round. 

`Current permissions of "/challenge/pwn": rwx-wxrwx` 

`Needed permissions of "/challenge/pwn": rwxrwxrwx`

So the command that we need to use to make the required changes is `chmod g+r /challenge/pwn`. Now, here, `g` represents the fact that we're changing permissions for the group, the `+` represents that we're **adding** permissions rather than removing them and the `r` signifies the permission to **read** the flag file. 

Now, how did I know i to change permissions for group and not user or others? Well, the order of the list of permissions(aka the string of characters with "-" sprinkled in between) is 
UUUGGGOOO, where `U: user, G: group and O: others` respectively.

We just do 8 of these and then, as instructed, change the permissions of the file `/flag` to make sure we can read it. We can do that by using the command `chmod ugo+r /flag`, and then a cheeky lil `cat /flag` to get us the flag.

**pwn.college{gjmE-u_BfN42etwaRtBbp6RRnuv.dBTM2QDL4kDN0czW}**

# #7 permissions-setting-practice

Very similar to the previous one, an easier way tbh. Here no matter the current permissions of the file `/challenge/run`, all we had to do was use the command `chmod` followed by `u=<required permissions>`, `g=<required permissions>`, `o=<required permissions>` and the required permissions are set by the question. This goes on for 8 rounds. The following is an example of one of my rounds:

```
 Current permissions of "/challenge/pwn": rw---xr--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": ---r--rw-
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

hacker@permissions~permissions-setting-practice:~$ chmod u=-,g=r,o=rw /challenge/pwn

```

Oh and if the user/group/world is required to have no permissions whatsoever, we just write `=-`. This is literally the previous question except that we can separate the permissions assignments using commas.

**pwn.college{Ef2oMF-RtxRjeb8bhkQC6VC1Gqj.dNTM5QDL4kDN0czW}**

# #8 the-SUID-bit

We learned the function of the SUID bit which basically gives the user permissions (temporarily) to execute files as the owner of them(in this case, the *root* user). 

For this challenge, we had to add the *SUID* bit to `/challenge/getroot`. To do this, our command was simply just `chmod u+s /challenge/getroot`. Then we input the command `/challenge/getroot`, in order to enter a root shell where we're the owner of the file and now all we have to do is use the `cat` command to get the flag

**pwn.college{EzjXXzpCMDGdBe6jQyudvKdcTTR.dNTM2QDL4kDN0czW}**