```perl
ls -l
find / -user flag14 2>/dev/null
cat /etc/passwd
ls /var/mail
```

It seems there is nothing here for us, no mail or script or executable

Let’s try to use gdb with getflag script as we did in level13

```perl
gdb getflag
run
```

We see an error message so it seems protected

Let’s `disas main` to see if we can find something

We see it’s using ptrace (line 67 using gdb) so we can bypass it with [this](https://gist.github.com/poxyran/71a993d292eee10e95b4ff87066ea8f2)

```perl
catch syscall ptrace
commands 1
set ($eax) = 0
continue
end
```

Now we will see if there is more protection

In line 439 we see it’s using getuid

```perl
0x08048afd <+439>:   call   0x80484b0 <getuid@plt>
```

Because  we want to use the flag14 user we need to know its id

Go to another console and look its id:

```perl
id flag14
```

It is 3014 so let’s repeat the level13 process:

```perl
break getuid
```

now we run it again and change the `$eax` and go step by step until we get the token

```perl
run
set $eax=3014
step
```

Once we have it we are done here

`7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ`