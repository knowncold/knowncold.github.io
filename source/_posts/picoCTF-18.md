---
title: picoCTF 2018
date: 2018-10-03
category: wiki
---

国庆匆忙做了几题简单的，希望有空能补上。<!--more-->

### Forensics Warmup 1 - Points: 50

Can you unzip this file for me and retreive the flag?

解压得到图片

> picoCTF{welcome_to_forensics}


### Forensics Warmup 2 - Points: 50
Hmm for some reason I can't open this PNG? Any ideas?

其实是jpg文件

> picoCTF{extensions_are_a_lie}

### General Warmup 1 - Points: 50
If I told you your grade was 0x41 in hexadecimal, what would it be in ASCII?

> picoCTF{A}

### General Warmup 2 - Points: 50
Can you convert the number 27 (base 10) to binary (base 2)?

> picoCTF{11011}

### General Warmup 3 - Points: 50
What is 0x3D (base 16) in decimal (base 10).

> picoCTF{61}

### Resources - Points: 50
We put together a bunch of resources to help you out on our website! If you go over there, you might even find a flag! https://picoctf.com/resources (link)

> picoCTF{xiexie_ni_lai_zheli}

### Reversing Warmup 1
Throughout your journey you will have to run many programs. Can you navigate to /problems/reversing-warmup-1_4_6b2499250c4624337a1948ac374c4934 on the shell server and run this program to retreive the flag?

> picoCTF{welc0m3_t0_r3VeRs1nG}

### Reversing Warmup 2
Can you decode the following string `dGg0dF93NHNfczFtcEwz` from base64 format to ASCII?

> picoCTF{th4t_w4s_s1mpL3}

### Crypto Warmup 1

Crpyto can often be done by hand, here's a message you got from a friend, llkjmlmpadkkc with the key of thisisalilkey. Can you use this table to solve it?.

```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
   +----------------------------------------------------
A | A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B | B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C | C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D | D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E | E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
F | F G H I J K L M N O P Q R S T U V W X Y Z A B C D E
G | G H I J K L M N O P Q R S T U V W X Y Z A B C D E F
H | H I J K L M N O P Q R S T U V W X Y Z A B C D E F G
I | I J K L M N O P Q R S T U V W X Y Z A B C D E F G H
J | J K L M N O P Q R S T U V W X Y Z A B C D E F G H I
K | K L M N O P Q R S T U V W X Y Z A B C D E F G H I J
L | L M N O P Q R S T U V W X Y Z A B C D E F G H I J K
M | M N O P Q R S T U V W X Y Z A B C D E F G H I J K L
N | N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
O | O P Q R S T U V W X Y Z A B C D E F G H I J K L M N
P | P Q R S T U V W X Y Z A B C D E F G H I J K L M N O
Q | Q R S T U V W X Y Z A B C D E F G H I J K L M N O P
R | R S T U V W X Y Z A B C D E F G H I J K L M N O P Q
S | S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
T | T U V W X Y Z A B C D E F G H I J K L M N O P Q R S
U | U V W X Y Z A B C D E F G H I J K L M N O P Q R S T
V | V W X Y Z A B C D E F G H I J K L M N O P Q R S T U
W | W X Y Z A B C D E F G H I J K L M N O P Q R S T U V
X | X Y Z A B C D E F G H I J K L M N O P Q R S T U V W
Y | Y Z A B C D E F G H I J K L M N O P Q R S T U V W X
Z | Z A B C D E F G H I J K L M N O P Q R S T U V W X Y
```

> picoCTF{SECRETMESSAGE}

### Crypto Warmup 2
Cryptography doesn't have to be complicated, have you ever heard of something called rot13? cvpbPGS{guvf_vf_pelcgb!}

> picoCTF{this_is_crypto!}

### grep 1 - Points: 75
Can you find the flag in file? This would be really obnoxious to look through by hand, see if you can find a faster way. You can also find the file in /problems/grep-1_3_8d9cff3d178c231ab735dfef3267a1c2 on the shell server.

```
cat file | grep "CTF"
picoCTF{grep_and_you_will_find_cdf2e7c2}
```

> picoCTF{grep_and_you_will_find_cdf2e7c2}

### net cat - Points: 75
Using netcat (nc) will be a necessity throughout your adventure. Can you connect to 2018shell1.picoctf.com at port 10854 to get the flag?

```
nc 2018shell1.picoctf.com 10854
That wasn't so hard was it?
```

> picoCTF{NEtcat_iS_a_NEcESSiTy_c97963fe}

### strings - Points: 100
Can you find the flag in this file without actually running it? You can also find the file in /problems/strings_1_c7bac958dd6a4b695dc72446d8014f59 on the shell server.

```
strings strings | grep CTF
YzOejwCTF3GVzbdb8PkOKp1cKvAwEUvRSOLLm1yFFETiT
picoCTF{sTrIngS_sAVeS_Time_d7c8de6c}
7Oqu9T7p8SAoQcOcQVHM46k1xpt1M6Iu2ag4dw1OFCTFRbv6
```

> picoCTF{sTrIngS_sAVeS_Time_d7c8de6c}

### pipe
During your adventure, you will likely encounter a situation where you need to process data that you receive over the network rather than through a file. Can you find a way to save the output from this program and search for the flag? Connect with 2018shell1.picoctf.com 48696.

```
nc 2018shell1.picoctf.com 48696 > ctf.txt
cat ctf.txt | grep CTF
picoCTF{almost_like_mario_f617d1d7}
```

> picoCTF{almost_like_mario_f617d1d7}

### Inspect Me
Inpect this code! http://2018shell1.picoctf.com:53213 (link)

```
<!-- I learned HTML! Here's part 1/3 of the flag: picoCTF{ur_4_real_1nspe -->
/* I learned CSS! Here's part 2/3 of the flag: ct0r_g4dget_402b0bd3} */
/* I learned JavaScript! Here's part 3/3 of the flag:  */
```

> picoCTF{ur_4_real_1nspect0r_g4dget_402b0bd3}

### grep 2
This one is a little bit harder. Can you find the flag in /problems/grep-2_1_ef31faa711ad74321a7467978cb0ef3a/files on the shell server? Remember, grep is your friend.

```
grep picoCTF . -r      
./files9/file13:picoCTF{grep_r_and_you_will_find_4baaece4}
```

> picoCTF{grep_r_and_you_will_find_4baaece4}

### Client Side is Still Bad 

I forgot my password again, but this time there doesn't seem to be a reset, can you help me? http://2018shell1.picoctf.com:8930 (link)

```javascript
  function verify() {
    checkpass = document.getElementById("pass").value;
    split = 4;
    if (checkpass.substring(split*7, split*8) == '}') {
      if (checkpass.substring(split*6, split*7) == 'ebbd') {
        if (checkpass.substring(split*5, split*6) == 'd_d0') {
         if (checkpass.substring(split*4, split*5) == 's_ba') {
          if (checkpass.substring(split*3, split*4) == 'nt_i') {
            if (checkpass.substring(split*2, split*3) == 'clie') {
              if (checkpass.substring(split, split*2) == 'CTF{') {
                if (checkpass.substring(0,split) == 'pico') {
                  alert("You got the flag!")
                  }
                }
              }
      
            }
          }
        }
      }
    }
    else {
      alert("Incorrect password");
    }
  }
```
> picoCTF{client_is_bad_d0ebbd}

### admin panel
We captured some traffic logging into the admin panel, can you find the password?

```
))yE|@@~P0].[T
wPOST /login HTTP/1.1
Host: 192.168.3.128
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:59.0) Gecko/20100101 Firefox/59.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.168.3.128/
Content-Type: application/x-www-form-urlencoded
Content-Length: 53
Connection: keep-alive
Upgrade-Insecure-Requests: 1

user=admin&password=picoCTF{n0ts3cur3_13597b43}
```

> picoCTF{n0ts3cur3_13597b43}
