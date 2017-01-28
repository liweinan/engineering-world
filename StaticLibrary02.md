#『阿男的工程世界』＊`.o`文件和`.a`文件（2）＊

首先阿男问大家一个问题，`.o`文件是什么格式？其实稍加思考就应该得出答案，`.o`文件只不过是不完整的可执行程序，所以格式应该就是可执行程序文件格式，而Linux默认格式是`ELF`，那么`.o`文件就应该是`ELF`格式的。我们可以用`objdump`命令来验证这一点：

```asm
$ objdump -D lib1.o | head

lib1.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <hello>:
   0:	55                   	push   %rbp
   1:	48 89 e5             	mov    %rsp,%rbp
   4:	bf 00 00 00 00       	mov    $0x0,%edi
```

可以看到，`lib1.o`的`file format`是`elf64-x86-64`，验证了我们的分析推测。接下来说说`.a`文件，`.a`文件就是`.o`文件的一个大包。假如我们有多个`.o`文件，那么我们可以使用`ar`命令把多个`.o`文件打包成一个`.a`文件。`ar`命令的`man page`如下：

![](imgs/weli@fedora/~-projs-engineering-world-codes_006.png)

