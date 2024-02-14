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
level09@SnowCrash:~$ ./level09 aaaaaa
abcdef
```

When I executed the string character, I got the result as below. Same logic for the same character of string.

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
// the index value has been added for each char.
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

👉 So, all we have to do is to create the code to decode it. **This function transforms a given string by subtracting an index value from each character's ASCII value and then prints the result.**

👉 Here's how the transformation works using an example with the input string "abcd":

```
level09@SnowCrash:~$ ./level09 aaaa
abcd
```

This explains how "abcd" transformed as "aaaa".

- The ASCII value of 'a' is 97, and its index value is 0. So, the output value is 'a' - 0 = 'a'.
- The ASCII value of 'b' is 98, and its index value is 1. So, the output value is 'b' - 1 = 'a'.
- The ASCII value of 'c' is 99, and its index value is 2. So, the output value is 'c' - 2 = 'a'.
- The ASCII value of 'd' is 100, and its index value is 3. So, the output value is 'd' - 3 = 'a'.

Therefore, each character in the input string is transformed based on its position in the string, and the transformed result is printed.

<details>
  <summary> Decode code 👻 </summary>
  
```
#include <stdio.h>

int main (int argc, char *argv[])
{
char *arg;
int i = 0;

// protection for only one argument
if (argc != 2) {
fprintf(stderr, "Only one argument is accepted!!👾\n");
return 1;
}

// arg = first argv[1]. So the first arugments after ./a.out (execution)
arg = argv[1];
// while loop until the end of the argv[1]
while (*arg) {
printf("%c", *arg -i);
// \*arg: current index position
// this statement prints the current character pointed to by 'arg', after subtracting the value of i, This substraction operation modifies the ASCII value of the character.
i++; // increments the value of i by 1 in each iteration of the loop
arg++; // move the arg pointer to the next chararacter in the string
}
printf("\n");
return 0;
}

/\* Detailed example for i++; and arg++;

Suppose we have the input string `"hello"`. Here's how the loop would work:

- For the first character `'h'`, `*arg` points to `'h'` and `i` is 0. So, `'h' - 0` equals `'h'`, and `'h'` is printed.
- Then, `i` is incremented to 1 and `arg` is moved to the next character, `'e'`.
- For the second character `'e'`, `*arg` points to `'e'` and `i` is now 1. So, `'e' - 1` equals `'d'`, and `'d'` is printed.
- This process continues for each character in the string.

\*/

```

How to use ?
Try to compile inside the tmp directory to avoid the permission denied issues.

```

level09@SnowCrash:/tmp$ gcc decode.c -o decode

```

  </details>


So, we got our token

```

level09@SnowCrash:~$ cat token | xargs /tmp/decode
f3iji1ju5yuevaus41q1afiuq

```
This command performs a transformation on the content of the file named "token" using a custom program located at "/tmp/decode". Let's break it down:

- `cat token`: This command reads the content of the file named "token" and outputs it to the standard output.

- `|`: This symbol is called a pipe and it is used to pass the output of one command as the input to another command.

- `xargs /tmp/decode`: This command takes each line of input from the previous command (the content of "token") and passes it as an argument to the program located at "/tmp/decode".

- `:`: This is an argument separator. It separates the command from its arguments.

So, in summary, this command reads the content of the file "token", passes each line of content as an argument to the program "/tmp/decode", and performs some transformation or decoding operation on it. The ":" is just an argument separator and doesn't have any special meaning in this context.



Try to retrieve the password

```

level09@SnowCrash:~$ su flag09
Password: f3iji1ju5yuevaus41q1afiuq
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z

```


<details>
  <summary> Password </summary>
  s5cAJpM8ev6XHw998pRWG728z
  </details>

```
