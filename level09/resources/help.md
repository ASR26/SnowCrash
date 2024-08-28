We find a similar problem here, an executable which ask for a file and a file to read from. 

The difference is that this time we can read the token file but it seems to be encrypted

In this case when we execute it we find this

```jsx
./level09 token
tpmhr
```

If we try to execute it with other inputs we get this

```jsx
./level09 1
1
./level09 12345
13579
./level09 abcde
acegi
```

It seems the file adds to each character its index

Lets try with the token

```jsx
./level09 'f4kmm6p|=pnDBDu{'
f5mpq;vEyxONQ
su flag09
Password f5mpq;vEyxONQ
su: Authentication failure

```

It seems to be already encrypted so let’s decrypt it

We will create a simple decrypter that substracts the index to each character (in out machine)

```jsx
import sys

code = sys.argv[1]
decrypted = ""
for i in range(0, len(code)):
	decrypted = decrypted + chr(ord(code[i]) - i)
print(decrypted)
```

- `ord` convierte un caracter en su valor ASCII
- `chr` convierte un valor ASCII en el caracer

Now we will copy the token file to our machine using `scp`

```jsx
scp -P 4242 decrypt.py level09@10.12.250.234:/tmp/.
```

We will execute it giving the content of `token` as parameter and we will get the right token

```jsx
python /tmp/decrypt.py `cat token`
f3iji1ju5yuevaus41q1afiuq

su flag09
Password: f3iji1ju5yuevaus41q1afiuq
```

Having the flag we can go to level10

```jsx
su level10
Password: s5cAJpM8ev6XHw998pRWG728z
```