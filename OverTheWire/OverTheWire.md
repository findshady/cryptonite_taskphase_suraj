# LEVEL 4

The challenge involved looking for a file that's in a human-readable format. There were 10 files and they had to be opened with the command:

```bash
cat -- -file0X
```

where **X**=your desired file number.

# LEVEL 5

This challenge involved knowledge of the `find` command. The criteria was that the file must :
1. Not be executable.
2. Be of the size 1033 bytes.
3. Be in a human-readable format.

The commands for the aforementioned conditions are: 

```bash
find ./ -type f -size 1033c !-executable
```

where: 
`./` corresponds to the current directory.
`-type f` specifies the search to be limited to files only, not folders.
`-size 1033c` specifies the search to be for files that are exactly 1033 **c**haracters of size.

# LEVEL 6 

In this challenge, we had to use the `find` command again. This time we're looking straight in the root directory so we exclude the initial `.` from the previous command.  Also, we add `2>/dev/null` to the end to find one that doesn't hit us with a "permission denied" like 700 other files. Our command is: 
```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

# LEVEL 8

Here, the password is found in a line that occurs only **once**. We are going to need a command called `uniq` that has filters based on identical lines. This can also count (`-c`) or only return words that are duplicated (`-d`).  This command on it's own will not suffice, we need to use this with `sort`, which is used to sort the lines of a text file. It performs functions such as sorting in reverse (`-r`)  and sorting numerically (`-n`).  Our final command will be 

```bash
sort data.txt | uniq -u
```

# LEVEL 11

This level indirectly wants us to decipher using a well known cipher known as `ROT13` where all lower and uppercase letters are rotated by 13 positions. We're gonna do this the hard way using the command `tr`. Our command is going to be :

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

`'A-Za-z'`: This specifies the range of all alphabetic characters (`A-Z` for uppercase, `a-z` for lowercase).
`'N-ZA-Mn-za-m'`: This specifies a **rotated** range of alphabetic characters, starting from `N` and wrapping around to `M`.

# LEVEL 12

Here, we start off by creating a temporary directory to put 