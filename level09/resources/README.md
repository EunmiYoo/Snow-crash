## Level09

Login as usual wih a password

```
ssh -i ~/.ssh/id_rsa.pub level09@127.0.0.1 -p 4242
25749xKZ8L7DkSCwJkT9dyv6f
```

We can see token file and level09 file.

```
level09@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level09 level09  140 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level09 level09  220 Apr  3  2012 .bash_logout
-r-x------ 1 level09 level09 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
-r-x------ 1 level09 level09  675 Apr  3  2012 .profile
----r--r-- 1 flag09  level09   26 Mar  5  2016 token
```

Execute ltrace with level09 file.

```
level09@SnowCrash:~$ ltrace ./level09
__libc_start_main(0x80487ce, 1, 0xbffff7b4, 0x8048aa0, 0x8048b10 <unfinished ...>
ptrace(0, 0, 1, 0, 0xb7e2fe38)                                                 = -1
puts("You should not reverse this"You should not reverse this
)                                            = 28
+++ exited (status 1) +++
```

Can't read the token. I exectued the token with level09 file and we got "tpmhr", hmm. let's dig in to it more.

```
level09@SnowCrash:~$ cat token
f4kmm6p|=�p�n��DB�Du{��
level09@SnowCrash:~$ ./level09 token
tpmhr
```

I put different value after the ./level09 and seems like there's a pattern.

```
level09@SnowCrash:~$ ./level09 1
1
level09@SnowCrash:~$ ./level09 12
13
level09@SnowCrash:~$ ./level09 123
135
level09@SnowCrash:~$ ./level09 1234
1357
level09@SnowCrash:~$ ./level09 12345
13579
level09@SnowCrash:~$ ./level09 a
a
level09@SnowCrash:~$ ./level09 ab
ac
level09@SnowCrash:~$ ./level09 abc
ace
```

When I executed the string character, I got the result as below.

```
level09@SnowCrash:~$ ./level09 abcd
aceg
```

```
input: abcd

a - index 0, ascii + 0, output a
b - index 1, ascii + 1, output c
c - index 2, ascii + 2, output e
d - index 3, ascii + 3, output g


output: acegi
```

Same logic for the number

```
level09@SnowCrash:~$ ./level09 1234
1357
```

```
1 - index 0, ascii + 0, output 1
2 - index 1, ascii + 1, output 3
3 - index 2, ascii + 2, output 5
4 - index 3, ascii + 3, output 7
```

I also, put the value that we've got from the token and it printed the non-printable charachters.

```
level09@SnowCrash:~$ ./level09 "f4kmm6p|=p�n��DB�Du��"

f5mpq;v�Ey���{�����XW��]�
��
```

So, if we resume this situation:

1. The first value after executable value is incremented based on the ascii chart.

2. And there's also non printbale value to be printed in non readable way for humans.

👉 So, all we have to do is to create the code to decode it. I will use the C decode
