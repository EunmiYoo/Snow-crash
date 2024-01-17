## Level 04

Put the password for the level04.

```
qi0maab88jeaj46qoumi7maus
```

With a command of ls, we found a file written in perl script. We can see that this script is running on **localhost:4747**

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
