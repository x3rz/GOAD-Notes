1. Increase the value of buffer x2 ex: 1024, 2048 and so on.
2. In some cases if you sent 4 INT3 but there you seeing 3 INT3 and then do one thing that is scroll up and check if they are still 3 or now they are 2 INT3 but now if you see 2 then try appeneding Z  after EIP and before esp
3. In the case of normal input like in Overflow1 or vulnserver then `\r\n` should be appeneded at the end of sending via python.
4. At the single commmand send do try adding `\n` e.i: buff += '\n'.
5. *From overflow2* : 

6. We can also use `!mona findmsp -distance 1000` here 1000 is equal to the number of buffer you sent like if 1024 then `!mona findmsp -distance 1024` it will find you the position of EIP automatically. after creating payload with offset_create.rb
7. If you think that you found right bad characters and still your payload doesnt work then try using this `!mona jmp -r esp -cpb <all_badchars>` use this jmp esp addr of this command.
8. **If you see that sub_esp and badchars contains same value like \x83 in overflow2 then use nop sub_esp = "\x90"\*20**

---------------

### Apaar bhaiya tricks

1.  automate the fuzzing increase by 100 and tellls at what it crashes
2. Manually by seeing dump right side.
3. overfloow subesp change in due to bad chars sub = "\x90"*
23 3c 83 ba 