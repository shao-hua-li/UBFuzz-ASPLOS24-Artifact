UBGen
=====

This is a guide of using UBGen, the UB program generator used in UBFuzz, to generate UB programs.

The sourcecode is located in `/artifact/UBGen/` and is also available online `https://github.com/shao-hua-li/UBGen`.

To generate a UB program, e.g., buffer-overflow, execute
```shell
cd /artifact/UBGen/
./ubgen.py --ub buffer-overflow --out ./mutants/
```
This script will generate buffer-overflow programs by mutating one seed.
The location of generated UB programs are specified by `--out`.

Alternatively, you can use an integer index to specify the UB. For example, the above command is equivalent to
```shell
./ubgen.py --ub 0 --out ./mutants
```

You can use `./ubgen --help` to find detailed help information.

Suppose there are generated programs under `./mutants/` and one of the file is `./mutants/mutated_0_tmp6a83k7sn.c`. All generated files of the same prefix are from the same seed Csmith program.
For example, `./mutants/mutated_1_tmp6a83k7sn.c` would be another UB program from the same seed. To compile the generated UB program with AddressSanitizer, execute
```shell
gcc -fsanitize=address -g -w ./mutants/mutated_0_tmp6a83k7sn.c -o test.out
```
Executing `./test.out` would normally cause sanitizer warning as such
```shell
=================================================================
==2109518==ERROR: AddressSanitizer: global-buffer-overflow on address 0x55c88aeb20e4 at pc 0x55c88ae8060c bp 0x7ffefdf319e0 sp 0x7ffefdf319d0
WRITE of size 4 at 0x55c88aeb20e4 thread T0
    #0 0x55c88ae8060b in func_1 out/mutated_0_tmp1hg6ngof.c:1134
    #1 0x55c88aeaa956 in main out/mutated_0_tmp1hg6ngof.c:6901
    #2 0x7f500578dd8f  (/lib/x86_64-linux-gnu/libc.so.6+0x29d8f)
    #3 0x7f500578de3f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x29e3f)
    #4 0x55c88ae7d404 in _start (/artifact/UBGen/test.out+0xc404)

0x55c88aeb20e4 is located 0 bytes to the right of global variable 'g_9' defined in 'out/mutated_0_tmp1hg6ngof.c:781:16' (0x55c88aeb20e0) of size 4
0x55c88aeb20e4 is located 60 bytes to the left of global variable 'g_11' defined in 'out/mutated_0_tmp1hg6ngof.c:782:16' (0x55c88aeb2120) of size 4
SUMMARY: AddressSanitizer: global-buffer-overflow out/mutated_0_tmp1hg6ngof.c:1134 in func_1
Shadow bytes around the buggy address:
  0x0ab9915ce3c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ab9915ce3d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ab9915ce3e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ab9915ce3f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0ab9915ce400: 00 00 00 00 04 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
=>0x0ab9915ce410: f9 f9 f9 f9 04 f9 f9 f9 f9 f9 f9 f9[04]f9 f9 f9
  0x0ab9915ce420: f9 f9 f9 f9 04 f9 f9 f9 f9 f9 f9 f9 00 02 f9 f9
  0x0ab9915ce430: f9 f9 f9 f9 00 f9 f9 f9 f9 f9 f9 f9 00 f9 f9 f9
  0x0ab9915ce440: f9 f9 f9 f9 04 f9 f9 f9 f9 f9 f9 f9 01 f9 f9 f9
  0x0ab9915ce450: f9 f9 f9 f9 00 f9 f9 f9 f9 f9 f9 f9 02 f9 f9 f9
  0x0ab9915ce460: f9 f9 f9 f9 02 f9 f9 f9 f9 f9 f9 f9 02 f9 f9 f9
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
  Shadow gap:              cc
==2109518==ABORTING
```