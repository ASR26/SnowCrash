Use find to “find” what can we access with this user so we do this command

```bash
find / -user flag00 2>/dev/null
```

we get /usr/sbin/john

then we see something interesting: 

`cdiiddwpgswtgt`

We will use cyberchef to decode this, using rot13 to find something in clean text (set it to rot 11)

there we get: nottoohardhere

now we make 

```bash
su flag00
password: nottoohardhere
getflag
```

and there we have the flag: 

`x24ti5gi3x0ol2eh4esiuxias`

now lets

```bash
su level01
password: x24ti5gi3x0ol2eh4esiuxias
```