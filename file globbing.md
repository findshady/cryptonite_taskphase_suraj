# #1 matching-with-*

We learn about how we can shorten or "glob" the name of a directory by letting `*` complete the rest of the name of us. 

In this case, since we're restricted to 4 characters only, our argument after `cd`  should be `/ch*`, which takes us into the `/challenge` directory where, as directed, we input `/challenge/run` to get the flag.

**pwn.college{Q6TsYAySzqdrqrB-upZHhjRU6ZA.dFjM4QDL4kDN0czW}**


# #2 matching-with-?

Simpler than the first challenge. Does the same wildcard stuff as `*` but only for **ONE** character this time. All we had to do was,(again) as directed, `/?ha??enge` to get into that directory and then `/challenge/run` to get the flag.

**pwn.college{oQ0g2bu2JgpXq3vTUtIJxxrpBwG.dJjM4QDL4kDN0czW}**

# #3 matching-with-[]

Again, `[]` is just `?` but more specific where we input the stuff that it kinda searches against. Their prompt was to glob 4 files in an argument which is simply just `/challenge/run file_[a,b,s,h]`

**pwn.college{4qKY6ncJTTFDokj1LAhD2BHE_ab.dNjM4QDL4kDN0czW}**

# #4 matching-paths-with-[]

As the name suggests, literally just specify your path, it's that simple. Command was `/challenge/run challenge/files/file_[a,b,s,h]`

**pwn.college{Ym0eFI6WEIOLD6d6h9pMzTAERFJ.dRjM4QDL4kDN0czW}**  


# #5 mixing-globs

We need to figure out a way to include common letters from the words 'challenging', 'educational' and 'pwning' and address it under 6 characters using globs. Since the starting three letters of each word are c,e and p respectively, the arguement must be `[cep]*`.

The letters inside the `[]` make sure that the first letters allign and hence help us find the words and the `*` fills in the rest of the word.

**pwn.college{A84UepyTCM8EeI7JzpY0S0GJWdW.dVjM4QDL4kDN0czW}**

# #6 exclusionary-globbing

Basically the same as whatever we've learnt but this time we learnt to exclude the given set of words. The argument is simply `[!pwn]*`

where `[]` signify the brackets where any of the characters, when detected, will be counted. HOWEVER, since there is a `!`, our function is inverted. Which means the words that do NOT start with `p,w` and `n` will be considered. The `*` just fills in the rest of the word lol.

**pwn.college{Y7veJ_Z53V1LR3P4OIVjnFyrNHS.dZjM4QDL4kDN0czW}**
