Now we will check /etc/passwd

there we find this line: `flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`

if we try that thing (`42hDRfypTqqnw`) as password it doesnt work so lets use john the ripper to break the hash

echo `42hDRfypTqqnw` > passwd

*install JTR*

To instal John The Ripper we will go to kali and run the next command (it should be installed anyway)

```bash
apt-get install john -y
```

In case we don't have a kali machine or we can't' (or don't want to) install it in our machine we can install a simple docker with it running the following commands (we will need to copy the password into the docker to decrypt it):

```bash
docker pull phocean/john_the_ripper_jumbo
docker run -it phocean/john_the_ripper_jumbo
```


Now we will decrypt the password

```bash
john passwd
```

We get the password `abcdefg`

Let’s try this password and go on

```bash
su flag01
password: abcdefg
```

Here is the token so lets go to the next one:

```bash
su level02
password: f2av5il02puano7naaf6adaaf
```