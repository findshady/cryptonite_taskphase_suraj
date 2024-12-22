
#  GDB baby step 1

**Flag:** `picoCTF{549698}`

**Thought Process**

* This challenge obviously requires the use of `gdb` which is a debugging tool that can be accessed thru Linux. In previous challenges and CTFs, I have used to tool to perform various tasks such as finding the address of the flag in the Binary Exploitation challenge **`flag leak`** . 
* Since I'm familiar with this tool, I started off with trying to disassemble the entire file, but first I had to execute it. (please note I renamed the challenge file to "bruh").
* The command for this was `gdb ./bruh`.

which then hit me with a 
```bash
Reading symbols from ./bruh...
(No debugging symbols found in ./bruh)
```

which then prompted me to re-read the question, which had mentioned the use of the `main` symbol. My next instinct was to set a breakpoint so I could run the file from where it's required (the `main` function). For that, my command was `break main`, which gave me a successful message: 

```bash
Breakpoint 1 at 0x1131
```

Now that we have set the breakpoint, I'm gonna run the command using `run` and it's gonna list out all the processes(please note that I installed `gef` as someone on reddit recommended it to me)


![Pasted image 20241108221508](https://github.com/user-attachments/assets/5ccf7d31-2d9d-4b71-95b9-c7064290ca6a)


and as we can see in the line under the section `code:x86:64` (which was also a hint on the website), the hex next to `eax` is 0x86342, which in decimal is 549698. 

**Mistakes made**
* Initially overthought the question and assumed that the hex file was **INSIDE** the register `eax`(which would've made for a trickier question tbh). So i accessed the contents and tried converting that hex to decimal but that was not the flag.
```bash
gef➤  info registers eax
eax            0x55555129          0x55555129
```

**References**

* https://www.prepostseo.com/tool/hex-to-decimal



# Vault Door 1

**Flag: `picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4}
`**

**Thought Process**

* The given code consisted of this check password string: 
```java
return password.length() == 32 &&

               password.charAt(0)  == 'd' &&

               password.charAt(29) == 'a' &&

               password.charAt(4)  == 'r' &&

               password.charAt(2)  == '5' &&

               password.charAt(23) == 'r' &&

               password.charAt(3)  == 'c' &&

               password.charAt(17) == '4' &&

               password.charAt(1)  == '3' &&

               password.charAt(7)  == 'b' &&

               password.charAt(10) == '_' &&

               password.charAt(5)  == '4' &&

               password.charAt(9)  == '3' &&

               password.charAt(11) == 't' &&

               password.charAt(15) == 'c' &&

               password.charAt(8)  == 'l' &&

               password.charAt(12) == 'H' &&

               password.charAt(20) == 'c' &&

               password.charAt(14) == '_' &&

               password.charAt(6)  == 'm' &&

               password.charAt(24) == '5' &&

               password.charAt(18) == 'r' &&

               password.charAt(13) == '3' &&

               password.charAt(19) == '4' &&

               password.charAt(21) == 'T' &&

               password.charAt(16) == 'H' &&

               password.charAt(27) == '6' &&

               password.charAt(30) == 'f' &&

               password.charAt(25) == '_' &&

               password.charAt(22) == '3' &&

               password.charAt(28) == 'd' &&

               password.charAt(26) == 'f' &&

               password.charAt(31) == '4';

    }
```

Now, I sorted this in ascending order of integers present in the () brackets and our new code is

```java
password.charAt(0)  == 'd' &&
password.charAt(1)  == '3' &&
password.charAt(2)  == '5' &&
password.charAt(3)  == 'c' &&
password.charAt(4)  == 'r' &&
password.charAt(5)  == '4' &&
password.charAt(6)  == 'm' &&
password.charAt(7)  == 'b' &&
password.charAt(8)  == 'l' &&
password.charAt(9)  == '3' &&
password.charAt(10) == '_' &&
password.charAt(11) == 't' &&
password.charAt(12) == 'H' &&
password.charAt(13) == '3' &&
password.charAt(14) == '_' &&
password.charAt(15) == 'c' &&
password.charAt(16) == 'H' &&
password.charAt(17) == '4' &&
password.charAt(18) == 'r' &&
password.charAt(19) == '4' &&
password.charAt(20) == 'c' &&
password.charAt(21) == 'T' &&
password.charAt(22) == '3' &&
password.charAt(23) == 'r' &&
password.charAt(24) == '5' &&
password.charAt(25) == '_' &&
password.charAt(26) == 'f' &&
password.charAt(27) == '6' &&
password.charAt(28) == 'd' &&
password.charAt(29) == 'a' &&
password.charAt(30) == 'f' &&
password.charAt(31) == '4';

```

Now, extracting only the characters in between the '', our string (and contents of our flag) is `d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4`


# Vault Door 3

**Flag: `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}`**

**Thought Process:**

* So we're given a java code that basically sets a bunch of rules that have been applied on a string and we're given the output string, that is `jU5t_a_sna_3lpm12g94c_u_4_m7ra41`.
* The set of rules are: 
```java
public boolean checkPassword(String password) {

        if (password.length() != 32) {

            return false;

        }

        char[] buffer = new char[32];

        int i;

        for (i=0; i<8; i++) {

            buffer[i] = password.charAt(i);

        }

        for (; i<16; i++) {

            buffer[i] = password.charAt(23-i);

        }

        for (; i<32; i+=2) {

            buffer[i] = password.charAt(46-i);

        }

        for (i=31; i>=17; i-=2) {

            buffer[i] = password.charAt(i);

        }

        String s = new String(buffer);

        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");

    }

}
```

* I found two ways to go about this, the first is the manual way, which is manually creating a table and writing the corresponding addresses next to it. 
* The second way is to write a reverse code to directly get the flag. Initially I was going to try to write a reverse code in java but my friend suggested that a C code would be much simpler so the corresponding program to find the original inputted flag is 

```C
#include<stdio.h>
int main(){
    char *password = "jU5t_a_sna_3lpm18gb41_u_4_mfr340";
    char new[32];
    int i;
    for (i=0; i<8; i++) {
        new[i] = password[i];
    }
    for (; i<16; i++) {
        new[i] = password[23-i];
    }
    for (; i<32; i+=2) {
        new[i] = password[46-i];
    }
    for (i=31; i>=17; i-=2) {
        new[i] = password[i];
    }
    printf("%s",new);
}
```

which directly outputted our flag.

**Incorrect Tangents**
* Tried doing it manually the first time, while it can be done, it is time consuming and the room for error is extremely high.



# ARMssembly 1

**Flag: `picoCTF{00000D2A}`**

**Thought Process**
* Did a lot of reading online about the format of the code.

The code I was given for this chall was: 

```assembly
	.arch armv8-a
	.file	"chall_1.c"
	.text
	.align	2
	.global	func
	.type	func, %function
func:
	sub	sp, sp, #32
	str	w0, [sp, 12]			
	mov	w0, 79					
	str	w0, [sp, 16]		
	mov	w0, 7				
	str	w0, [sp, 20]			
	mov	w0, 3					
	str	w0, [sp, 24]			
	ldr	w0, [sp, 20]			
	ldr	w1, [sp, 16]			
	lsl	w0, w1, w0				
	str	w0, [sp, 28]		
	ldr	w1, [sp, 28]			
	ldr	w0, [sp, 24]			
	sdiv	w0, w1, w0		
	str	w0, [sp, 28]			
	ldr	w1, [sp, 28]		
	ldr	w0, [sp, 12]		
	sub	w0, w1, w0			
	str	w0, [sp, 28]			
	ldr	w0, [sp, 28]
	add	sp, sp, 32
	ret
	.size	func, .-func
	.section	.rodata
	.align	3
.LC0:
	.string	"You win!"
	.align	3
.LC1:
	.string	"You Lose :("
	.text
	.align	2
	.global	main
	.type	main, %function
main:
	stp	x29, x30, [sp, -48]!
	add	x29, sp, 0
	str	w0, [x29, 28]
	str	x1, [x29, 16]
	ldr	x0, [x29, 16]
	add	x0, x0, 8
	ldr	x0, [x0]
	bl	atoi
	str	w0, [x29, 44]
	ldr	w0, [x29, 44]
	bl	func
	cmp	w0, 0
	bne	.L4
	adrp	x0, .LC0
	add	x0, x0, :lo12:.LC0
	bl	puts
	b	.L6
.L4:
	adrp	x0, .LC1
	add	x0, x0, :lo12:.LC1
	bl	puts
.L6:
	nop
	ldp	x29, x30, [sp], 48
	ret
	.size	main, .-main
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits
```


* Now, the  `str` command  basically stores user input to the stack and the `mov` command moves a value to the corresponding register.
* Here are the comments I made for self reference along the way:

![Pasted image 20241208215853](https://github.com/user-attachments/assets/5a99b07b-d2a7-480d-8532-e6f41c250681)


I shall explain the first few commands and new commands as we come across them:

* First, [sp,12] basically refers to the memory in the stack +12, and in that memory we're going to store (**str**) the RHS, that is `w0` in this case.
* Next, `mov` w0,79 refers to moving the number **79** into the register w0.
* This continues till line 16, where we're met with a new command `ldr`, which is used to load a value from memory into a register. So it moves the value corresponding to [sp,20] which is 7 (line 13) to the register `w0`. 
* In line 18, we come across the command `lsl`, which performs **Logical Left Shift** operation on the given registers.  The computation is in the above image. All computations were made using a simple python program.
* Coming to line 22, `sdiv` performs **Signed Integer Division** operation on the given numbers. It basically just divides them and considers the decimals redundant. 
* Following the commands till line 25 is extremely straight-forward. Now, at this point, `3770` is loaded into `w1`, and **x is the argument that the question requires**. 

Upon analysis of the complete code, I saw that the `win` condition would only be activated when
-`w0` and 0 are compared and found to be equal, i.e. `w0`= 0.

![Pasted image 20241208220418](https://github.com/user-attachments/assets/4aa0ac90-f0db-415a-b494-ebfbdfe66cd8)

Oh and `bne` stands for **Branch if Not Equal**, which basically gets skipped if the statement is true, which we want it to be, as `.L4` leads to a `loss` result. In line 57, we have `.LC0` which leads us to the `win` condition. 

![Pasted image 20241208220639](https://github.com/user-attachments/assets/cace94f5-896c-4cd9-93f2-13ce3af29d0b)

What do we take away from this? That x must be 3370 in order for `w0` to be 0 (line 26).

Since that is our argument, **3370** in hex is D2A, or in our requested format, **00000D2A**.

**Incorrect Tangents**
* Initially failed to understand sequencing of the code and how to arrive at the win condition.
* Spent a lot of time on understanding the code for this challenge and then making notes at every step. 

**References**
* https://www.programiz.com/python-programming/online-compiler/
* https://www.rapidtables.com/convert/number/decimal-to-hex.html?x=3370
* https://armasm.com/docs/registers-and-memory/memory-copy/

# GDB baby step 2

**Flag: `picoCTF{307019}`**


**Thought Process**
* This is extremely similar to the previous gdb challenge, all i had to do was start up gdb and disassemble the file.
* Then, I disassembled main using 

```bash
gef➤  disassemble main
```

which led me to this

![Pasted image 20241209181147](https://github.com/user-attachments/assets/11b73bcb-9cd5-414a-bab6-f58c3e05cee5)

Since our question asks the contents of the `eax `register at the END of the `main` function, as we see in the above screenshot, the last instance of the `eax` register is found at the memory location `0x000000000040113e`, so we're going to add a breakpoint at the very next memory location i.e

```bash
 b *0x0000000000401141
```

![Pasted image 20241209181412](https://github.com/user-attachments/assets/824a7031-0cb3-449d-9535-a6cbec8104c5)

After setting the breakpoint, we're going to run the program from there using the command `r`, and then all we have to do is use 

```bash
info registers eax
```


which gives us the hex value `0x4af4b`, which is `307019` in decimal.

**New Things Learnt**
* The value of registers change as we perform operations on it, instead of manually performing the operation on the `eax` register, i simply accessed the value at the register after all the operations had be performed. ****


**References**
* https://www.rapidtables.com/convert/number/hex-to-decimal.html?x=4AF4B


# GDB baby step 3

**Flag: `picoCTF{0x6bc96222}`**

**Thought Process**
* This one was a little different from the other gdb challenges. I started off with the same procedure of disassembling the function `main`. 

```bash
gef➤  disass main
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   rbp
   0x000000000040110b <+5>:     mov    rbp,rsp
   0x000000000040110e <+8>:     mov    DWORD PTR [rbp-0x14],edi
   0x0000000000401111 <+11>:    mov    QWORD PTR [rbp-0x20],rsi
   0x0000000000401115 <+15>:    mov    DWORD PTR [rbp-0x4],0x2262c96b
   0x000000000040111c <+22>:    mov    eax,DWORD PTR [rbp-0x4]
   0x000000000040111f <+25>:    pop    rbp
   0x0000000000401120 <+26>:    ret
End of assembler dump.
```

* In the challenge description, we're asked to examine byte-wise the memory that the constant is loaded in. Also, in one of the hints it is mentioned that any registers used in our command should be prepended with `$`. 

* In this case, we can see that the register that is corresponding to our memory location is `[rbp-0x4]`.  Also, we're asked not to use square brackets for registers in the command.

Now, all that's left to do is add a breakpoint after the memory location provided, and run it and then use the command that we're asked to use.

![Pasted image 20241210001012](https://github.com/user-attachments/assets/feb4aa1d-674f-40d3-abcc-80b1f834d61f)

The command that we're asked to use is the `x/4xb addr` command where `addr` is the register at the corresponding memory location. In this case it is the register [rbp-0x4]. Following their instructions of formatting, the command will be 

```bash
x/4xb $rbp-0x4
```

Where, 
* `x` stands for examine.
* `4` is the number of bytes.
* `b` stands for **Bytes**.
* `$` is mandatory to be prepended before a register.

![Pasted image 20241210001354](https://github.com/user-attachments/assets/0d0d526c-be0d-49ff-841b-4e2bdb830b2c)

Thus the contents for our flag will be `0x6bc962222`

**New Things Learnt**
* Examining memory using the `x` command. 
* Getting comfortable with setting breakpoints and reading contents of registers in `gdb`.

**References**
* https://ftp.gnu.org/old-gnu/Manuals/gdb/html_node/gdb_55.html

# GDB baby step 4

**Flag: `picoCTF{12905}`**

**Thought Process**
* This challenge only took me one command under `gdb`.
* Once the file was downloaded, i disassembled the main function (like the previous programs) and noticed that the function was called `func1`.

```bash
gef➤  disass main
Dump of assembler code for function main:
   0x000000000040111c <+0>:     endbr64
   0x0000000000401120 <+4>:     push   rbp
   0x0000000000401121 <+5>:     mov    rbp,rsp
   0x0000000000401124 <+8>:     sub    rsp,0x20
   0x0000000000401128 <+12>:    mov    DWORD PTR [rbp-0x14],edi
   0x000000000040112b <+15>:    mov    QWORD PTR [rbp-0x20],rsi
   0x000000000040112f <+19>:    mov    DWORD PTR [rbp-0x4],0x28e
   0x0000000000401136 <+26>:    mov    DWORD PTR [rbp-0x8],0x0
   0x000000000040113d <+33>:    mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401140 <+36>:    mov    edi,eax
   0x0000000000401142 <+38>:    call   0x401106 <func1>
   0x0000000000401147 <+43>:    mov    DWORD PTR [rbp-0x8],eax
   0x000000000040114a <+46>:    mov    eax,DWORD PTR [rbp-0x4]
   0x000000000040114d <+49>:    leave
   0x000000000040114e <+50>:    ret
End of assembler dump.
```


Now, to know what constant the function multiplied the register `eax` by, I needed to disassemble the function using 

```bash
gef➤  disass func1
```

and then i saw this

```bash
gef➤  disass func1
Dump of assembler code for function func1:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   rbp
   0x000000000040110b <+5>:     mov    rbp,rsp
   0x000000000040110e <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x0000000000401111 <+11>:    mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401114 <+14>:    imul   eax,eax,0x3269
   0x000000000040111a <+20>:    pop    rbp
   0x000000000040111b <+21>:    ret
End of assembler dump.
```

Clearly, the constant in question has to be `0x3269` which when converted to binary gave me `12905` which was the answer.

**New Things Learnt**
* Disassembling functions other than main

**References**
* https://www.rapidtables.com/convert/number/hex-to-decimal.html?x=3269


#  keygenme-py

**Flag:**`picoCTF{1n_7h3_|<3y_of_f911a486}`

**Thought Process**
* We're given a python file that was basically a license key validation process which includes a trial code.
* At the start of the code, we're given a format that is `picoCTF{1n_7h3_|<3y_of_xxxxxxxx}`
* Now, the key includes a dynamic part, meaning it's going to be different for everybody. This is based on the SHA-256 hash of the `username_trial` variable.
* SHA-256 (Secure Hash Algorithm 256-bit) is a cryptographic hash function that generates a fixed size, 256-bit (32-byte) hash value from input data of any size.


* Now, from this part of the code

```python
if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:

            return False

        else:

            i += 1

  

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:

            return False

        else:

            i += 1

  

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:

            return False

        else:

            i += 1

#and so on
```


we can see that specific characters from the hash are used to form the dynamic part.

* The dynamic part of the flag is simply:
    * Character at 4th position in the hash.
    * Character at 5th position in the hash.
    *  Character at 3rd position in the hash.
and so on

Now our script to craft the entire flag including the dynamic part would be

```python
import hashlib

username = "GOUGH"

part_1 = "picoCTF{1n_7h3_|<3y_of_"
part_2 = "}"

hash_result = hashlib.sha256(username.encode()).hexdigest()

positions=[4, 5, 3, 6, 2, 7, 1, 8]
dynamic = "".join([hash_result[pos] for pos in positions])

# Assembling the entire key
full_key = part_1 + dynamic + part_2
print("Flag:", full_key)
```

which gave me the output

```python
$ python3 new.py
Flag: picoCTF{1n_7h3_|<3y_of_f911a486}
```

**New Things Learnt**
* Gained more knowledge about SHA-256 hashing.

**References**
* https://www.simplilearn.com/tutorials/cyber-security-tutorial/sha-256-algorithm

# crackme-py

**Flag:**`picoCTF{1|\/|_4_p34|\|ut_502b984b}`

**Thought Process**
* I'm gonna keep it real for this one. I saw the `bezos_cc_secret` variable and upon reading the code, I found out that this was in ROT-47. 
* So I headed over to [CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT47(47)&input=QTo0QHIldUxgTS1eTTBjMEFiY00tTUZFMGRfYTNoZ2MzTg) and it directly gave me the flag. Because this is literally just cipher text.
* A major issue that the code had was in the comparison, where

```python
if user_value_1 > user_value_2:

        greatest_value = user_value_1

    elif user_value_1 < user_value_2:

        greatest_value = user_value_2
```

they're comparing numbers alphabetically instead of numerically. This means that it would output 9 to be greater than 10, because the letter "n" comes after the letter "t" in the alphabet.

A simple fix would be to take in the inputs as integers using int().


# speeds and feeds

**Flag:**`picoCTF{num3r1cal_c0ntr0l_f3fea95b}`

**Thought Process**
* Upon running the instance, we're met with a whole bunch of what looks like G-Code (confirmed due to the hint)
* All I did was google gcode viewer and it led me to [this](https://ncviewer.com/) website in which I inputted all the contents of the G code and it gave me the flag. 

![Pasted image 20241222173301](https://github.com/user-attachments/assets/d47f5893-b489-409f-a05c-0f3fa1adb70c)

**New Things Learnt**
* Learned about G-Code and how it can be visualized using softwares and websites.
* G-Code is a language used to control CNC (Computer Numerical Control) machines. It includes instructions for moving a cutting tool along specified paths, including straight lines and arcs.

**References**
* https://makezine.com/article/digital-fabrication/machining/get-to-know-your-cnc-how-to-read-g-code/



# Safe Opener

**Flag:**`picoCTFP{pl3as3_l3t_m3_1nt0_th3_saf3}`

**Thought  Process**

* Opened the java file. Looked at the variable `encodedkey`.
* Knew it had to be Base64 since it was mentioned in the code in the first few lines.
* All I had to do was input it in [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=Y0d3ellYTXpYMnd6ZEY5dE0xOHhiblF3WDNSb00xOXpZV1l6) and it gave me the password, which was `pl3as3_l3t_m3_1nt0_th3_saf3`


# Safe Opener

**Flag:**`picoCTF{SAf3_0p3n3rr_y0u_solv3d_it_3dae8463}`

**Thought Process**
* We're given a `.class` file and we're hinted towards decompiling it.
* Opened the file in [DogBolt.org](https://dogbolt.org/?id=ec8f85ac-40c2-4697-ad8d-f00e9ce1d49b#Ghidra=84) (Could've opened in in Ghidra aswell) and one of the variables literally had the flag written out.

# patchme.py

**Flag:**`picoCTF{p47ch1ng_l1f3_h4ck_f01eabfa}`

**Thought Process**
* We're given a python file that when run, asks for a password.
* In the code, we see this function

```python
def level_1_pw_check():

    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):

        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")
        print(decryption)
        return
    print("That password is incorrect")
```

* Obviously, the password string would be all the given strings appended one after the other, which is `ak98-=90adfjhgj321sleuth9000`.
* Upon entering the password,

```bash
$ python3 flag.py
Please enter correct password for flag: ak98-=90adfjhgj321sleuth9000
Welcome back... your flag, user:
picoCTF{p47ch1ng_l1f3_h4ck_f01eabfa}
```

# unpackme.py

**Flag:**`picoCTF{175_chr157m45_cd82f94c}`

**Thought Process**
* Well, the code we were given was

```python
import base64

from cryptography.fernet import Fernet

payload = b'gAAAAABkzWGSzE6VQNTzvRXOXekQeW4CY6NiRkzeImo9LuYBHAYw_hagTJLJL0c-kmNsjY33IUbU2IWlqxA3Fpp9S7RxNkiwMDZgLmRlI9-lGAEW-_i72RSDvylNR3QkpJW2JxubjLUC5VwoVgH62wxDuYu1rRD5KadwTADdABqsx2MkY6fKNTMCYY09Se6yjtRBftfTJUL-LKz2bwgXNd6O-WpbfXEMvCv3gNQ7sW4pgUnb-gDVZvrLNrug_1YFaIe3yKr0Awo0HIN3XMdZYpSE1c9P4G0sMQ=='
key_str = 'correctstaplecorrectstaplecorrec'

key_base64 = base64.b64encode(key_str.encode())

f = Fernet(key_base64)

plain = f.decrypt(payload)

exec(plain.decode())
```

Which confused me because they had given us everything in place, Literally all i did was add a line that printed `plain.decode()` and my output was

```bash
$ python3 flag.py
Flag:

pw = input('What\'s the password? ')

if pw == 'batteryhorse':
  print('picoCTF{175_chr157m45_cd82f94c}')
else:
  print('That password is incorrect.')
```

# Reverse

**Flag:**`picoCTF{3lf_r3v3r5ing_succe55ful_c83965de}`

**Thought Process**
* For some reason, I thought this would work and it did. LMAO.

```bash
$ strings ret | grep pico
picoCTF{H
Password correct, please see flag: picoCTF{3lf_r3v3r5ing_succe55ful_c83965de}
```

* Using the actual method tho, upon using to decompile the file, in the code I see the line

```C
if (iVar1 == 0) {
    puts("Password correct, please see flag: picoCTF{3lf_r3v3r5ing_succe55ful_c83965de}");
    puts(local_38);
  }
```

# GDB Test Drive

**Flag:**`picoCTF{d3bugg3r_dr1v3_72bd8355}`

**Thought Process**
* Now, by simply following the given instructions, we get the flag but I wanted to understand how it works.
* Once I ran the `layout asm` command, I saw this 

![Pasted image 20241222184443](https://github.com/user-attachments/assets/466b8dca-ab4b-4bd1-85b7-b18ea3f2fcc9)

* In the instructions, we're told to create a breakpoint at `(main+99)` which is `0x132a`. This memory location is where we see a command **sleep** which probably explains why the program does nothing when executed.
* Now, we're told to jump to `(main+104)` which is at `0x132f`, clearly right after the sleep command, so that it can be evaded, thus making the program run and give us the flag.


# Fresh Java

**Flag:**`picoCTF{700l1ng_r3qu1r3d_9332a6be}`

**Thought Process**
* just popped the file into my favorite decompiling website (https://dogbolt.org/) and it gave me this 

```C
#include "out.h"


void main_java_lang_String___void(String[] param1)

{
  PrintStream pPVar1;
  String objectRef;
  int iVar2;
  char cVar3;
  Scanner objectRef_00;
  
  objectRef_00 = new Scanner(System_in);
  pPVar1 = System_out;
  pPVar1.println("Enter key:");
  objectRef = objectRef_00.nextLine();
  iVar2 = objectRef.length();
  if (iVar2 != 0x22) {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x21);
  if (cVar3 != '}') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x20);
  if (cVar3 != 'e') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1f);
  if (cVar3 != 'b') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1e);
  if (cVar3 != '6') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1d);
  if (cVar3 != 'a') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1c);
  if (cVar3 != '2') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1b);
  if (cVar3 != '3') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1a);
  if (cVar3 != '3') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x19);
  if (cVar3 != '9') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x18);
  if (cVar3 != '_') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x17);
  if (cVar3 != 'd') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x16);
  if (cVar3 != '3') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x15);
  if (cVar3 != 'r') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x14);
  if (cVar3 != '1') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x13);
  if (cVar3 != 'u') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x12);
  if (cVar3 != 'q') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x11);
  if (cVar3 != '3') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x10);
  if (cVar3 != 'r') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xf);
  if (cVar3 != '_') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xe);
  if (cVar3 != 'g') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xd);
  if (cVar3 != 'n') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xc);
  if (cVar3 != '1') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xb);
  if (cVar3 != 'l') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(10);
  if (cVar3 != '0') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(9);
  if (cVar3 != '0') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(8);
  if (cVar3 != '7') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(7);
  if (cVar3 != '{') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(6);
  if (cVar3 != 'F') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(5);
  if (cVar3 != 'T') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(4);
  if (cVar3 != 'C') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(3);
  if (cVar3 != 'o') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(2);
  if (cVar3 != 'c') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(1);
  if (cVar3 != 'i') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0);
  if (cVar3 != 'p') {
    pPVar1 = System_out;
    pPVar1.println("Invalid key");
    return;
  }
  pPVar1 = System_out;
  pPVar1.println("Valid key");
  return;
}

```


* Very clearly, the flag is in the the if statement bracket in reverse order, after extracting the digits, we get `picoCTF{700l1ng_r3qu1r3d_9332a6be}`


# Bbbbloat

**Flag:**`picoCTF{cu7_7h3_bl047_695036e3}`

**Thought Process**
* On running the given executable file, it asks for it's favorite number and I'm surprised 69 wasn't the obvious answer.
* Again, putting the file in the website gave me and then while going thru the code I saw this line

```C
if (var_48 != 0x86187)
        puts("Sorry, that's not it!");
    else
```

Converting 0x6187 to binary, we get **549255** and sure enough

```bash
$ ./bloat
What's my favorite number? 549255
picoCTF{cu7_7h3_bl047_695036e3}
```

# Shop

**Flag:**`picoCTF{b4d_brogrammer_9c118bbf}`

**Thought Process**
* We're given an instance and it's a shop with different kinds of options to buy and sell stuff.
* Upon trying to get my coins above 100 (you need a 100 coins to buy the flag), I kept trying things.
* Initially, I tried to sell negative amounts of things to see if the shop accepted it and it did, but it then gave me negative amount of coins.
* Now, when I try to BUY negative amount of things

![Pasted image 20241222193851](https://github.com/user-attachments/assets/11474618-b9be-402a-a262-ad8aa8d29e1c)

* All I had to do was convert the flag from binary to ASCII.
* Yes, I did try to open the file in Ghidra and try to understand it but it was a vast program,

# Classic Crackme 0x100

**Flag:**`picoCTF{s0lv3_angry_symb0ls_31b29976}`

**Thought Process**
* This one was fun. We're given a binary to crack. Upon decompiling and reading the source code, I came across the main function (I changed a lot of variable name to make it more understandable)
  
```C
int main()

{

    int var1;

    __builtin_strcpy(&var1, "xjagpediegzqlnaudqfwyncpvkqneusycourkguerjpzcbstcc");

    setvbuf(__TMC_END__, nullptr, 2, 0);
    printf("Enter the secret password: ");
    void var5;
    __isoc99_scanf("%50s", &var5);
    int i = 0;
    int x = strlen(&var1);
    int y = 0x55;
    int z = 0x33;
    int var2 = 0xf;
    char char = 0x61;
    for (; i <= 2; i += 1)
    {

        for (int j = 0; j < x; j += 1)

        {
            int var3 = ((j % 0xff) >> 1 & y) + ((j % 0xff) & y);
            int var4 = (var3 >> 2 & z) + (z & var3);
            *(&var5 + j) = char + ((var4 >> 4 & var2) + *(&var5 + j) - char + (var2 & var4)) % 0x1a;
        }
    }

    if (memcmp(&var5, &var1, x))
        puts("FAILED!");
    else
        printf("SUCCESS! Here is your flag: %s\n", "picoCTF{sample_flag}");
    return 0;
}
```

* Now, I can tell that the input is going thru a bunch of different operations in the for loop and the output we're getting is `xjagpediegzqlnaudqfwyncpvkqneusycourkguerjpzcbstcc`.

* To craft the perfect input for the flag, I needed to write a script that reverses the operations performed on the string.

```python
var1 = "xjagpediegzqlnaudqfwyncpvkqneusycourkguerjpzcbstcc"
x = len(var1)  
y = 0x55  
z = 0x33  
var2 = 0xf  
char = 0x61 

# first do it for a single char
def reverse_transform_char(char1, j):
    var3 = ((j % 0xff) >> 1 & y) + ((j % 0xff) & y)
    var4 = (var3 >> 2 & z) + (z & var3)

# reversing the transformation applied to the char
    transformed_char = ord(char1) - char
    result_char = (transformed_char - ((var4 >> 4 & var2) + (var2 & var4))) % 0x1a + char
    return chr(result_char)

# now for every char
def reverse_transform(v0):
    result = []
    for j in range(x):
        result.append(reverse_transform_char(v0[j], j))
    return ''.join(result)
  
# now reversing the loop, by running 3 iterations
def find_password():
    transformed = var1
    original = transformed

    for i in range(3):
        original = reverse_transform(original)
    return original

  
password = find_password()
print("the og password is :", password)
```


**New Things Learnt**
* Beginner level binary cracking by reversing the program.


# unpackme

**Flag:**`picoCTF{up><_m3_f7w_5769b54e}`

**Thought Process**
* First off, the file is compressed using UPX, which is a software used to compress binaries and make files harder to reverse engineer. How I found that out is by attempting to decompile the file and so many of the strings mentioned UPX and I googed it.
* Now, after downloading UPX on my computer, I ran it's command and after successfully unpacking it, I could now see the proper source code.
* After running Ghidra on it, the main function looked like this

![Pasted image 20241222213335](https://github.com/user-attachments/assets/ec31c9e7-93d4-42b9-aa62-63a62d481cc5)

* Right off the bat, we notice that the input (`local_44`) is being compared to a hex `0xb83cb` which is `754635` in binary.

* So now, executing the file, we have

![Pasted image 20241222213453](https://github.com/user-attachments/assets/992b1190-b47f-4142-8b05-1790441cc73e)

