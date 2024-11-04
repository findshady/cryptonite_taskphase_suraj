
#  C3

**Flag**: `picoCTF{adlibs}` 

We shall begin this challenge by understanding the code given to us. 

**Thought Process**

The raw code given to us consists of two "lookup" strings that contain characters in a certain order. The crux of the code processes each character from one lookup by looking up it's index from the other lookup and adding it's result to the output. This entire process takes place inside of a loop.  Here's the raw code :

```python
import sys
chars = ""
from fileinput import input

for line in input():
  chars += line

lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""
prev = 0

for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur

sys.stdout.write(out)
```

The very first thing that we have to do is swap the lookups, as we notice that every single character from our cipher text falls under `lookup2` and that, by swapping them, we wouldn't just be referring to the mapping of a character in `lookup2` to `lookup1`, instead we'd be sort of converting them into one of the characters in `lookup1`.

Two more major changes here would be to: 

1. Change the operation in the function from `-` to `+`. That is, instead of cycling backwards, we cycle forward through the characters.
2. Create a new variable; `new` in my case. I assigned `new` the aforementioned function and added the variable `new` to the final output. This way, inside the loop, the previous output is iterated to be added for the following iteration. 

Our modified code would look like: 
```python
import sys
chars = ""
from fileinput import input
for line in input():
  chars += line

lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""
prev = 0

for char in chars:

  cur = lookup2.index(char)
  new = lookup1[(cur + prev) % 40]
  out+= new
  prev = lookup1.index(new)

sys.stdout.write(out)
```

and the output gave us another python code with comments as hints:

```python
#asciiorder
#fortychars
#selfinput
#pythontwo

chars = ""
from fileinput import input
for line in input():
    chars += line
b = 1 / 1

for i in range(len(chars)):
    if i == b * b * b:
        print chars[i] #prints
        b += 1 / 1
```

Now, based off the comments, `#selfinput` and `#python2` told me that i needed to give this exact code to itself to be processed by python. 

**For context:** 

To run my python programs and give them input, I used to use the following command in my terminal:

```bash
cat input | python3 convert.py
```
where `input` is simply the renamed cipher text file. 

To give the program to itself, we use the command 

```bash
cat input | python3 convert.py > file.py
```
which is basically just sending the contents of `convert.py` into a newly created file `file.py`.

Now, we simply run the newly made file, by giving itself as input 

```bash
cat file.py | python2 file.py
```
which finally outputs the flag.

**New concepts learnt**

* Conversion of code between versions of python.
* Cyclical ciphers that involved manipulation of given presets of data.
* Running a python file with it's own script as input.

**Mistakes made/Alternative methods**

* Didn't understand the logic behind creation of new variable at first. 
* Used the original ciphertext `input` as input for the final python2 file.
* I could've modified the given code again to make it more python2 friendly and then simply run that code but this path allowed me to expand my knowledge on the terminal.



# Custom Encryption

**Flag**: `picoCTF{}`

**Thought Process/Approach**

