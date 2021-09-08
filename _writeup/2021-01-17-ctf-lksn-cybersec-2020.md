---
title: "CTF: LKSN CyberSecurity 2020 - Reversing Module"
tag:
  - ltrace
  - strings
  - gdb
---

Recently I have been done for reversing challenge on module LKSN CyberSecurity 2020
~~not as competitor :v~~. Very excited to get the flag on the binary. Unfortunately,
I don’t have complete last challenge called bongkarzzz because website not available.
Probably, this closed after done the competition. Enjoy!

> **Notice** <br />
> The information provided in this site is for educational purposes regarding
> pentesting. The author of the site will not be held any responsibility for any
> misuse of the information from this site.

#### Summary

* [ltrace](#reverse-1)
* [strings](#reverse-2)
* [gdb](#bongkarzzz)

## Reverse 1

Given `ELF` 64-bit not stripped file, we have task to reverse the binary to get the
flag. I use file and readelf to get information this file.

```bash
$ file reverse1
reverse1: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV),
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=73dfc3067c6b6a300c90ed9c29f3925c9d2f20a7, for
GNU/Linux 3.2.0, not stripped
$ readelf -h reverse1
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Shared object file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x10c0
  Start of program headers:          64 (bytes into file)
  Start of section headers:          15680 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         11
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```

When I Execute that file then i get password prompt. So I should get the password,
to get the flag.

```bash
$ chmod a+x ./reverse1
$ ./reverse1

            .--``..---.
.CTF:`    .LKS-SMK28`
             `.--..`

CTF LKS SMK28
 password:> password
 SMK BISA!
```

I tried to use `ltrace` to tracing library when i execute the file. and i see
`strcmp` operations that mean the program try to compare the input string with
another string

```bash
$ ltrace ./reverse1
_ZNSt8ios_base4InitC1Ev(0x5604e12382b9, 0xffff, 0x7fff7d84b2f8, 224) = 0
__cxa_atexit(0x7f23c9f64a40, 0x5604e12382b9, 0x5604e1238060, 6) = 0
strcpy(0x7fff7d84b0a3, "k0o")                   = 0x7fff7d84b0a3
strcat("k0o", "pi_h")                           = "k0opi_h"
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 4, 0x685f6970
) = 0x5604e1238080
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236010, 0, 3072) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072             .--``..---.
) = 0x5604e1238080
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236039, 0, 3072) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072 .CTF:`    .LKS-SMK28`
) = 0x5604e1238080
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236058, 0, 3072) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072             `.--..`
) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072
) = 0x5604e1238080
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236077, 0, 3072) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 0x38324b4d5320534bCTF LKS SMK28
) = 0x5604e1238080
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236085, 0, 3072) = 0x5604e1238080
_ZStrsIcSt11char_traitsIcEERSt13basic_istreamIT_T0_ES6_PS3_(0x5604e12381a0, 0x7fff7d84b0e0, 0x7f23ca0733d0, 0x203e3a64726f7773 password:>  <no return ...>
--- SIGWINCH (Window changed) ---
--- SIGWINCH (Window changed) ---
--- SIGWINCH (Window changed) ---
--- SIGWINCH (Window changed) ---
k0opi_h
<... _ZStrsIcSt11char_traitsIcEERSt13basic_istreamIT_T0_ES6_PS3_ resumed> ) = 0x5604e12381a0
strcat("k0opi_h", "ita")                        = "k0opi_hita"
strcat("k0opi_hita", "m_pht")                   = "k0opi_hitam_pht"
strcmp("k0opi_h", "k0opi_hitam_pht")            = -105
_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e123609f, 105, 0x68705f6d) = 0x5604e1238080
_ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 0x202141534942204b SMK BISA!
) = 0x5604e1238080
+++ exited (status 0) +++
```

I am interersting with the string `k0opi_hitam_pht` so i tried input into password
prompt. and i get the flag! #PWNED

```bash
$ ./reverse1

             .--``..---.
.CTF:`    .LKS-SMK28`
             `.--..`

CTF LKS SMK28
 password:> k0opi_hitam_pht

 LKSSMK28{01c9fsd3gt34zxxcb0eb8a42d3c534rf3c570703e3t}
```

## Reverse 2

Next, given ELF 64-bit not stripped file called reverse2. and i get detail
information this file with `file` command.

```bash
$ file ./reverse2
./reverse2: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically
linked, interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=20f01148e7568ccfd9e0162018d8ca8111e419c4, for GNU/Linux 3.2.0, not stripped
```
I tried to see string that contains on the binary, and I see the flag that I
think it's not fake. So, i get one. But, i still want to know the realpassword.

```bash
$ strings ./reverse2
/lib64/ld-linux-x86-64.so.2
libc.so.6
__isoc99_scanf
puts
putchar
printf
__cxa_finalize
strcmp
__libc_start_main
GLIBC_2.7
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u/UH
0x00007fH
trolf
p4ssw0rdH
LKSSMK28H
yoik, yoH
ur flag H
is: LKSSH
MK28{r3vH
3rs1ng}
%16s
48661925H
4b9c
You FailH
[]A\A]A^A_
...
...
...
```

I interesting for string what i found `p4ssw0rd`. So, i tried to input into
password prompt. I used `ltrace` to tracing when file executed.

```bash
$ ltrace ./reverse2
puts("||=============================="...||====================================================================||
)     = 73
puts("||//////////////////////////////"...||////////////////////////////////////////////////////////////////////||
)     = 73
puts("||()========================| CT"...||()========================| CTF |==========================()||
)     = 66
puts("||()======================LKS SM"...||()======================LKS SMK 28=========================()||
)     = 66
puts("||//////////////////////////////"...||////////////////////////////////////////////////////////////////////||
)     = 73
puts("||=============================="...||====================================================================||
)     = 73
puts("Password:"Password:
)                               = 10
__isoc99_scanf(0x556fd5dab13c, 0x7ffe756f6920, 0, 0x7fea1e3c8ed3) = 1
strcmp("0x00007fff", "puts("||========")        = -64
puts("You Failed"You Failed
)                              = 11
+++ exited (status 0) +++
```

i see `strcmp` operation that mean comparing string with `0x00007fff`. i tried
input this string and get the flag! alhamdulillah.


```bash
$ ./reverse2
||====================================================================||
||////////////////////////////////////////////////////////////////////||
||()========================| CTF |==========================()||
||()======================LKS SMK 28=========================()||
||////////////////////////////////////////////////////////////////////||
||====================================================================||
Password:
0x00007fff
You Win
LKSSMK28{LKSSMK28_486619254b9c9f6e6800cfae77}
```

## crackme

Next, given ELF 64-bit not stripped file called crackme. and i get detail
information this file with `file` command.

```bash
$ file crackme
crackme: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV),
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=9283bfe84e4b274b954a775f733a5c9de669675a, for
GNU/Linux 3.2.0, not stripped
```

I tried use `strings` to see strings that available on the binary. I found
`strcmp` that mean string compare operation.

```bash
$ strings crackme
/lib64/ld-linux-x86-64.so.2
NK'K
Jw_s:\
mgUa
libc.so.6
puts
stdin
printf
fgets
memset
malloc
__cxa_finalize
strcmp
__libc_start_main
free
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u/UH
[]A\A]A^A_
Input Your Password
Please try later.
MANTUL, flag is LKSSMK28{s}
Password Salah!
;*3$"
sGCC: (Debian 9.2.1-19) 9.2.1 20191109
crtstuff.c
deregister_tm_clones
....
....
....
```

I used `ltrace` to see what binary do when executed. So, i input "password" into
password prompt and i see `strcmp` compared "password" with "JJJJJJJJJJJJJJBxs".

```bash
$ ltrace ./crackme
puts("Hi!\nInput Your Password"Hi!
Input Your Password
)                = 24
malloc(18)                                      = 0x5565bf7216b0
memset(0x5565bf7216b0, '\0', 18)                = 0x5565bf7216b0
fgets(password
"password\n", 18, 0x7f14edf29980)         = 0x5565bf7216b0
strcmp("password\n", "JJJJJJJJJJJJJJBxs")       = 38
puts("Password Salah!"Password Salah!
)                         = 16
free(0x5565bf7216b0)                            = <void>
+++ exited (status 0) +++
```

after that, i tried to use "JJJJJJJJJJJJJJBxs" as password. and i get the flag!

```bash
$ ./crackme                                                            127 ⨯
Hi!
Input Your Password
JJJJJJJJJJJJJJBxs
MANTUL, flag is LKSSMK28{JJJJJJJJJJJJJJBxs}
```

## bongkarzzz

In the bongkarzzz challenge, we have task reverse ELF 32-bit not stripped binary
file called bongkarzzz to have login website with username and password found in
bongkarzzz. For the first, i used `file` to get information type of file.

```bash
$ file bongkarzzz
bongkarzzz: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV),
dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux
2.6.32, BuildID[sha1]=2e97060d42116e12d32b27d76c5af8202e980d2f,
not stripped
```

Next, i tried to execute the file, and i get "Cari password :(" when i input wrong
password.

```bash
$ chmod +x bongkarzzz
$ ./bongkarzzz
Cari Username : ;alskdjf
Cari Password :(
```

I used GNU Debugger to see value of binary file, and show available function on
binary.

```bash
$ gdb bongkarzzz
GNU gdb 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from bongkarzzz...
(No debugging symbols found in bongkarzzz)
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x080482f4  _init
0x08048330  printf@plt
0x08048340  puts@plt
0x08048350  __gmon_start__@plt
0x08048360  __libc_start_main@plt
0x08048370  __isoc99_scanf@plt
0x08048380  _start
0x080483b0  __x86.get_pc_thunk.bx
0x080483c0  deregister_tm_clones
0x080483f0  register_tm_clones
0x08048430  __do_global_dtors_aux
0x08048450  frame_dummy
0x0804847b  main
0x080484f0  __libc_csu_init
0x08048560  __libc_csu_fini
0x08048564  _fini
(gdb)
```

i tried to see value of main function. and then i have `cmp` register
before `je` register this means that only equal value to have jump into next
statement.

```bash
(gdb) disassemble main
Dump of assembler code for function main:
    0x0804847b <+0>:     lea    0x4(%esp),%ecx
    0x0804847f <+4>:     and    $0xfffffff0,%esp
    0x08048482 <+7>:     pushl  -0x4(%ecx)
    0x08048485 <+10>:    push   %ebp
    0x08048486 <+11>:    mov    %esp,%ebp
    0x08048488 <+13>:    push   %ecx
    0x08048489 <+14>:    sub    $0x14,%esp
    0x0804848c <+17>:    sub    $0xc,%esp
    0x0804848f <+20>:    push   $0x8048580
    0x08048494 <+25>:    call   0x8048330 <printf@plt>
    0x08048499 <+30>:    add    $0x10,%esp
    0x0804849c <+33>:    sub    $0x8,%esp
    0x0804849f <+36>:    lea    -0xc(%ebp),%eax
    0x080484a2 <+39>:    push   %eax
    0x080484a3 <+40>:    push   $0x8048591
    0x080484a8 <+45>:    call   0x8048370 <__isoc99_scanf@plt>
    0x080484ad <+50>:    add    $0x10,%esp
    0x080484b0 <+53>:    mov    -0xc(%ebp),%eax
    0x080484b3 <+56>:    cmp    $0x111ee0,%eax
    0x080484b8 <+61>:    je     0x80484cc <main+81>
    0x080484ba <+63>:    sub    $0xc,%esp
    0x080484bd <+66>:    push   $0x8048594
    0x080484c2 <+71>:    call   0x8048340 <puts@plt>
    0x080484c7 <+76>:    add    $0x10,%esp
    0x080484ca <+79>:    jmp    0x80484e1 <main+102>
    0x080484cc <+81>:    sub    $0x8,%esp
    0x080484cf <+84>:    push   $0x65a4d9
    0x080484d4 <+89>:    push   $0x80485a6
    0x080484d9 <+94>:    call   0x8048330 <printf@plt>
    0x080484de <+99>:    add    $0x10,%esp
    0x080484e1 <+102>:   mov    $0x0,%eax
    0x080484e6 <+107>:   mov    -0x4(%ebp),%ecx
    0x080484e9 <+110>:   leave
    0x080484ea <+111>:   lea    -0x4(%ecx),%esp
    0x080484ed <+114>:   ret
End of assembler dump.
```

if we see in `cmp` register `EAX` has comparing with `$0x111ee0`. So, i assume
`$0x111ee0` is real username.

```bash
0x080484b3 <+56>:    cmp    $0x111ee0,%eax
```

I tried to see value of `$0x111ee0` with python, and i get int `1122016`.

```python
$ python3
Python 3.8.5 (default, Jul 28 2020, 12:59:40)
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 0x111ee0
1122016
```

So, i tried input int `1122016` as username and i get different output yeay!

```bash
$ ./bongkarzzz
Cari Username : 1122016
6661337
```

#### Reference

* [Introduction To Reverse Engineering](https://geekpradd.github.io//intro-to-reverse-engineering)
* [TryHackMe: Reversing ELF Writeup](https://medium.com/bugbountywriteup/tryhackme-reversing-elf-writeup-6fd006704148)
* [hackme: Deconstructing ELF an file](http://www.manoharvanga.com/hackme/)

