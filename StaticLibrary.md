#『阿男的工程世界』＊`.o`文件和`.a`文件＊

Linux下的`.o`文件是`object file`，用来最终链接成可执行文件。概念比较抽象，我们用代码来说话。首先写这样一个代码：

```c
// lib1.c
#include <stdio.h>

int hello() {
  printf("Hello, World!\n");
}
```

这个代码就是提供一个`hello`函数，我们编译这个代码：

```bash
$ cc -g -c lib1.c
```

编译成功后我们就得到了`.o`文件：

```bash
$ ls
lib1.c  lib1.o
```

这个编译出来的`lib1.o`已经包含代码对应的机器码了，但它本身还不是可执行文件，因为它还没有经过`link`，也就是`链接`的工作。什么是`link`？首先我们可以使用`nm`命令看看`lib1.o`里面的内容：


```bash
$ nm lib1.o
0000000000000000 T hello
                 U puts
```

可以看到`lib1.o`里面已经有了`hello`函数，但是`hello`函数里面调用`printf`函数，而`printf`函数是由`libc`库提供的，所以这个`.o`文件如果不和`libc`库做连接，它就没法在执行的时候找到`printf`的实现。

此外，这个代码本身也没有`main`方法，而`libc`标准C库要求代码的入口函数是`main()`，这是一种约定，因此这个`lib1.o`目前并不是一个可执行文件。因此`link`过程是必不可少的，让`.o`文件把所需要的相关的其它的库文件连接起来变成一个真正的可执行文件。

我们可以再写一个代码：

```c
// main.c
int main() {
  hello();
  return 0;
}
```

上面这个`main.c`提供`main()`入口函数，并且在里面使用`hello()`方法。我们编译这个代码：

```bash
$ cc -g -c main.c
main.c: In function ‘main’:
main.c:3:3: warning: implicit declaration of function ‘hello’ [-Wimplicit-function-declaration]
   hello();
   ^~~~~
```

可以看到在编译的过程中，`cc`警告我们没有找到`hello`的实现，这个是意料之中的。接下来我们把`lib1.o`和`main.o`给link到一起。Linux下的`Linker`是`ld`这个工具，但我们一般不直接使用它，因为我们一般自己没法保证把所有需要的文件全都找到并link在一起，并且可能传给`ld`的选项也不准确。所以我们基本上都是使用`cc`来做link的工作，而`cc`实际上会去调用`ld`来做链接的工作。下面我们来把`main.o`和`lib1.o`给link到一起：

```bash
$ cc -Xlinker -v main.o lib1.o
collect2 version 6.1.1 20160621 (Red Hat 6.1.1-3)
/usr/bin/ld -plugin /usr/libexec/gcc/x86_64-redhat-linux/6.1.1/liblto_plugin.so -plugin-opt=/usr/libexec/gcc/x86_64-redhat-linux/6.1.1/lto-wrapper -plugin-opt=-fresolution=/tmp/cckV9XiE.res -plugin-opt=-pass-through=-lgcc -plugin-opt=-pass-through=-lgcc_s -plugin-opt=-pass-through=-lc -plugin-opt=-pass-through=-lgcc -plugin-opt=-pass-through=-lgcc_s --build-id --no-add-needed --eh-frame-hdr --hash-style=gnu -m elf_x86_64 -dynamic-linker /lib64/ld-linux-x86-64.so.2 /usr/lib/gcc/x86_64-redhat-linux/6.1.1/../../../../lib64/crt1.o /usr/lib/gcc/x86_64-redhat-linux/6.1.1/../../../../lib64/crti.o /usr/lib/gcc/x86_64-redhat-linux/6.1.1/crtbegin.o -L/usr/lib/gcc/x86_64-redhat-linux/6.1.1 -L/usr/lib/gcc/x86_64-redhat-linux/6.1.1/../../../../lib64 -L/lib/../lib64 -L/usr/lib/../lib64 -L/usr/lib/gcc/x86_64-redhat-linux/6.1.1/../../.. -v main.o lib1.o -lgcc --as-needed -lgcc_s --no-as-needed -lc -lgcc --as-needed -lgcc_s --no-as-needed /usr/lib/gcc/x86_64-redhat-linux/6.1.1/crtend.o /usr/lib/gcc/x86_64-redhat-linux/6.1.1/../../../../lib64/crtn.o
GNU ld version 2.26.20160125
```

我在上面的`cc`命令里面加上了`-Xlinker -v`选项，这样我们就可以看到`cc`实际调用`ld`的命令展开了。从上面的输出可以看到实际展开的`ld`命令很复杂，除了我们指定的两个`.o`文件，还链接了一大堆其它的东西，其中当然有标准c库，还有很多其它东西，阿男后续慢慢给大家讲。最后我们得到了可执行文件`a.out`：

```bash
$ ls a.out
a.out
```

执行试试看：

```bash
$ ./a.out 
Hello, World!
```

用`nm`看看`a.out`里面的符号：

```c
$ nm ./a.out 
000000000060102c B __bss_start
000000000060102c b completed.6938
0000000000601028 D __data_start
0000000000601028 W data_start
0000000000400460 t deregister_tm_clones
00000000004004e0 t __do_global_dtors_aux
0000000000600e18 t __do_global_dtors_aux_fini_array_entry
00000000004005d8 R __dso_handle
0000000000600e28 d _DYNAMIC
000000000060102c D _edata
0000000000601030 B _end
00000000004005c4 T _fini
0000000000400500 t frame_dummy
0000000000600e10 t __frame_dummy_init_array_entry
0000000000400740 r __FRAME_END__
0000000000601000 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
00000000004005f0 r __GNU_EH_FRAME_HDR
000000000040053b T hello
00000000004003c8 T _init
0000000000600e18 t __init_array_end
0000000000600e10 t __init_array_start
00000000004005d0 R _IO_stdin_used
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
0000000000600e20 d __JCR_END__
0000000000600e20 d __JCR_LIST__
                 w _Jv_RegisterClasses
00000000004005c0 T __libc_csu_fini
0000000000400550 T __libc_csu_init
                 U __libc_start_main@@GLIBC_2.2.5
0000000000400526 T main
                 U puts@@GLIBC_2.2.5
00000000004004a0 t register_tm_clones
0000000000400430 T _start
0000000000601030 D __TMC_END__

```

可以看到最终生成的可执行文件里面，除了`hello`和`main`，还有很多其它东西，这里面链接进来的符号，很多是`libc`和`Linux Kernel`的标准接口，阿男后续给大家慢慢讲。

所以我们看到，其实C语言写的代码编译出来的程序就已经挺重的了，我们如果直接使用汇编代码，会`轻`的多。

