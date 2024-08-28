In the main directory we find a file `level02.pcap` 

It is a packet data file so we will copy it into our kali to analyze it using the command scp:

```jsx
scp -P 4242 level02@<ip>:~/level02.pcap ~/.
```

We will use tshark to analyze the file so we will install it with the next command:

```jsx
apt-get install tshark
```

We will look for strings with the next command:

```jsx
tshark -r level02.pcap -z follow,tcp,hex,0 -q
```

In this output we can see some lines with letter but one of the lines says `Password:` so we will see what follows this line

```jsx
        000000D5  01                                                .
        000000D6  00 0d 0a 50 61 73 73 77  6f 72 64 3a 20           ...Passw ord:
000000B9  66                                                f
000000BA  74                                                t
000000BB  5f                                                _
000000BC  77                                                w
000000BD  61                                                a
000000BE  6e                                                n
000000BF  64                                                d
000000C0  72                                                r
000000C1  7f                                                .
000000C2  7f                                                .
000000C3  7f                                                .
000000C4  4e                                                N
000000C5  44                                                D
000000C6  52                                                R
000000C7  65                                                e
000000C8  6c                                                l
000000C9  7f                                                .
000000CA  4c                                                L
000000CB  30                                                0
000000CC  4c                                                L
000000CD  0d                                                .
        000000E3  00 0d 0a                                          ...
        000000E6  01                                                .
        000000E7  00 0d 0a 4c 6f 67 69 6e  20 69 6e 63 6f 72 72 65  ...Login  incorre
        000000F7  63 74 0d 0a 77 77 77 62  75 67 73 20 6c 6f 67 69  ct..wwwb ugs logi
        00000107  6e 3a 20                                          n:
```

It’s followed by letters: `ft_wandr...NDRel.lol`

After that we can see the login incorrect message but it’s worth a try.

As expected we receive that message

Let’s take a look at the hex values. If we look at the ASCII table`7f` means `DEL` so we should delete the previous character, so we would get this: `ft_waNDReL0L`

Let’s try this one:

```jsx
su flag02
password ft_waNDReL0L
```

There it is, let’s got to the next level:

```jsx
su level03
passwd kooda2puivaav1idi4f57q8iq
```