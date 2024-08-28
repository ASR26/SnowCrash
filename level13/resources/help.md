We have an executable file which returns this message when executed:

`ID 2013 started us but we we expect 4242`

Using `strings`  we get this

```perl
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
exit
strdup
printf
getuid
__libc_start_main
GLIBC_2.0
PTRh`
UWVS
[^_]
0123456
UID %d started us but we we expect %d
boe]!ai0FB@.:|L6l@A?>qJ}I
your token is %s
;*2$"$
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
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
level13_back.c
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
__i686.get_pc_thunk.bx
data_start
ft_des
printf@@GLIBC_2.0
strdup@@GLIBC_2.0
_edata
_fini
getuid@@GLIBC_2.0
__DTOR_END__
__data_start
__gmon_start__
exit@@GLIBC_2.0
__dso_handle
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
__bss_start
main
_Jv_RegisterClasses
_init
```

And this is interesting:

```perl
[^_]
0123456
UID %d started us but we we expect %d
boe]!ai0FB@.:|L6l@A?>qJ}I
your token is %s
;*2$"$
```

We have to change our UID to get the token

We will use gdb for this

```perl
gdb level13
disas main
```

This will disasemble main function

We can see the call to getuid so let’s add a break there:

```perl
break getuid
```

Now we will run it again

```perl
run
```

And move step by step

```perl
step
```

Let’s see the register `eax` where the UID is stored

```perl
print $eax
```

We can see we are 2013 yet so let’s change it

```perl
set $eax=4242
```

We can check if it’s correct

```perl
print $eax
```

And now let’s go step by step until we get the token and move to the last user

```perl
su level14
Password: 2A31L79asukciNyi8uppkEuSx
```