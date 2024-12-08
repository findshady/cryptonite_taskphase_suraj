
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

Now, extracting only the characters in between the '', our string (and contents of our flag) is `d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4


# Vault Door 3

**Flag: `jU5t_a_s1mpl3_an4gr4m_4_u_c79a21`**

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


