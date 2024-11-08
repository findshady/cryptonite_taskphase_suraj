
#  GDB baby step 1

**Flag:** `picoCTF{549698}`

**Thought Process**

* This challenge obviously requires the use of `gdb` which is a debugging tool that can be accessed thru Linux. In previous challenges and CTFs, I have used to tool to perform various tasks such as finding the address of the flag in the Binary Exploitation challenge **`flag leak`** . 
* Since I'm familiar with this tool, I started off with trying to disassemble the entire file. (please note I renamed the challenge file to "bruh").
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

