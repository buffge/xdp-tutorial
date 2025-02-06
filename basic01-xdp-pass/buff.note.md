# 笔记



```llvm-objdump -S xdp_pass_kern.o``` 反汇编elf文件 因为编译时带了debug信息 可以看到指令与代码对应关系

如下所示: 
```
buff@m-1:~/projects/cc/xdp-tutorial/basic01-xdp-pass$ llvm-objdump -S   xdp_pass_kern.o

xdp_pass_kern.o:        file format elf64-bpf

Disassembly of section xdp:

0000000000000000 <xdp_prog_simple>:
;       return XDP_PASS;
       0:       b4 00 00 00 02 00 00 00 w0 = 0x2
       1:       95 00 00 00 00 00 00 00 exit
```

bpf程序需要将其加入到内核中才可以运行.
用户空间需要一个 ELF 加载器来读取该文件并以正确的格式将其传递到内核

所以这里有2个文件 一个是bpf程序,一个是用户态程序 加载bpf程序到内核

也可以通过  iproute2 ip, xdp-loader  加载

## 代码加载

```sudo ./xdp_pass_user -d lo```

输出
```
libbpf: elf: skipping unrecognized data section(7) xdp_metadata
libbpf: elf: skipping unrecognized data section(7) xdp_metadata
libbpf: elf: skipping unrecognized data section(7) xdp_metadata
libbpf: elf: skipping unrecognized data section(7) xdp_metadata
Success: Loading XDP prog name:xdp_prog_simple(id:50222) on device:lo(ifindex:1)
```
## 卸载
```sudo ./xdp_pass_user -d lo -U 50222```

输出:

```
buff@m-1:~/projects/cc/xdp-tutorial/basic01-xdp-pass$ sudo ./xdp_pass_user -d lo -U 50222
Detaching XDP program with ID 50222 from lo
Success: Unloading XDP prog name: xdp_prog_simple
```


