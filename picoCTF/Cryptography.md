
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

**Flag**: `picoCTF{custom_d2cr0pt6d_751a22dc}`

**Thought Process/Approach**

So, in this challenge, we were given a python encryption program that we're supposed to use to add a decryption element to it. My basic approach was to add a decryption function after each encryption function. 

Firstly, the given code contains a multiplicative encryption code that involves the usage of a key.

```python
def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(ord(char) * key * 311)
    return cipher
```

As we can see in the above snippet, to reverse the function:
* We must divide the number by `key * 311` to get back the ASCII value. 
* Then, we convert the ASCII number back to a character and add it to the list
The above process is iterated for as long as our number isn't zero. The code for this decryption function is:

```python
def decrypt_multiplicative(cipher, key):
    plaintext_chars = []
    for num in cipher:
        if num != 0:
            char_code = num // (key * 311)
            plaintext_chars.append(chr(char_code))
        else:
            plaintext_chars.append('\0')  
    return ''.join(plaintext_chars)
```

Second, we look at the raw code of the XOR encryption. 

```python 
def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text
```

We notice that it loops over `text_key` in reverse order. For this all we have to do is make it traverse through `text_key` and have characters added in reverse order.

Our decryption code for XOR will be:

```python
def dynamic_xor_decrypt(cipher_text, text_key):
    decrypted_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        decrypted_text = decrypted_char + decrypted_text  
    return decrypted_text
```

Now, the `test_decryption` function uses `generator` to create shared keys (which we will check to see if they match, if they do, communication was successful). Furthermore, `test_decryption` calls the `decrypt_multiplicative` and `dynamic_xor_decrypt` functions to reverse encryption. 

Finally, we run the main code using the provided values of `a`, `b`, and `cipher` to print out the flag.

The final code is appended below.

```python
from random import randint
import sys

def generator(g, x, p):
    return pow(g, x, p)

def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(ord(char) * key * 311)
    return cipher

def decrypt_multiplicative(cipher, key):
    # Reverse the multiplicative step
    plaintext_chars = []
    for num in cipher:
        if num != 0:
            char_code = num // (key * 311)
            plaintext_chars.append(chr(char_code))
        else:
            plaintext_chars.append('\0')  
    return ''.join(plaintext_chars)

def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text

def dynamic_xor_decrypt(cipher_text, text_key):
    # Reverse the XOR encryption
    decrypted_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        decrypted_text = decrypted_char + decrypted_text  # Reverse the order
    return decrypted_text

def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True

def test_decryption(cipher, text_key, a, b):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers.")
        return

    # Shared key calculation
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    if key != b_key:
        print("Invalid key")
        return

    # Step 1: Reverse the multiplicative encryption
    intermediate_text = decrypt_multiplicative(cipher, key)
    print(f"Intermediate text after multiplicative decryption: {intermediate_text}")

    # Step 2: Reverse the XOR encryption
    decrypted_text = dynamic_xor_decrypt(intermediate_text, text_key)
    print(f"Decrypted text: {decrypted_text}")

if __name__ == "__main__":
    # Replace `cipher`, `a`, `b`, and `text_key` with actual values
    cipher = [260307, 491691, 491691, 2487378, 2516301, 0, 1966764, 1879995, 1995687,
              1214766, 0, 2400609, 607383, 144615, 1966764, 0, 636306, 2487378, 28923,
              1793226, 694152, 780921, 173538, 173538, 491691, 173538, 751998, 1475073,
              925536, 1417227, 751998, 202461, 347076, 491691]
    a = 94
    b = 29
    text_key = "trudeau"
    
    test_decryption(cipher, text_key, a, b)

```

**New Concepts Learnt**
* Intermediate knowledge of python functions and calling them.
* Looked into the basics of Diffie-Hellman key exchange/generator function.
* Learnt about the usage of `__name__ == "__main__"` command in python.
* Learned somewhat of the logic behind writing a decoding program for a given encoding program.

**Mistakes**
* Messed up in the main function as I didn't understand it properly at first.
* Had chunks of my code written by AI.
* Didn't consider `num=0` case at first.



# miniRSA

**Flag: `picoCTF{n33d_a_lArg3r_e_d0cd6eae}`**

**Thought Process**
* After doing my research on RSA challenges, I noticed that in this challenge, e is extremely small (smallest value it can hold infact) and that there is no padding constant.
* A padding constant *k* is used when $m^n$, and it's formula is m = $\sqrt[e]{C+k.N}$, where:
    * C: Cipher Key
    * N: Part of both public and private key
    * *e*: Public Key Exponent
* In this case, padding constant (*k*) is zero. This means that we can directly find the *e*th root of c.
* To find this, online calculators weren't precise enough so I found a really simple code to find the inverse power of a number on [StackOverflow](https://stackoverflow.com/questions/52313392/compute-cube-root-of-extremely-big-number-in-python3) and modified it.

```python
def find_invpow(x, n):
    low = 0
    high = 1

	while high ** n < x:
        high *= 2

    while low < high:
        mid = (low + high) // 2

        if mid ** n < x:
            low = mid + 1
	        else:
            high = mid
    return low - 1  

f = find_invpow(2205316413931134031074603746928247799030155221252519872650080519263755075355825243327515211479747536697517688468095325517209911688684309894900992899707504087647575997847717180766377832435022794675332132906451858990782325436498952049751141, 3)

h = hex(f)

print(h)
```


The modifications I made to the code:
* Removed the case that tended to calculation of inverse of even powers.
* Added a few lines to make the output in hexadecimal, as we want it in ASCII.

![Pasted image 20241213164457](https://github.com/user-attachments/assets/f714fad6-45b1-4c01-bf04-1b45dc58e09a)

The output I got was `0x7069636f4354467b6e3333645f615f6c41726733725f655f64306364366561657c`.
Now, removing the `0x` and converting this to ASCII, we get the string `picoCTF{n33d_a_lArg3r_e_d0cd6eae}`.


**Alternative Method**
* The easiest way to solve this challenge was to simply use https://www.dcode.fr/rsa-cipher.
* Here all we had to do was input the required values and it spit out the flag.

**New Concepts Learnt**
* Learnt the basic theory and working of RSA.
* Learnt the case that e (Public Key Exponent) is much much lesser than N(Part of both public and private key), and that we could directly just take the cube root.

**Incorrect Tangents**
* Tried to find the cube root using an online calculator which led to answers with insufficient precision and therefore, the wrong flag.
* Forgot to exclude `0x` while decoding from hex to ASCII.

**References**
* https://ir0nstone.gitbook.io/crypto/rsa/public-exponent-attacks/small-e
* https://en.wikipedia.org/wiki/RSA_(cryptosystem)#Encryption
* https://stackoverflow.com/questions/52313392/compute-cube-root-of-extremely-big-number-in-python3
* https://www.rapidtables.com/convert/number/hex-to-ascii.html