## MY-OS

### 1 调试平台

一开始用的是MAC系统来调试引导程序，BIOS切换CPU执行权，执行boot程序是没有问题的。

之后在boot程序中构建了FAT12文件系统，用其加载loader程序，这个时候在挂载目录文件这个地方遇到了坎，因此放弃了MAC平台的调试方案，转而到Linux系统。

* [MAC平台](./docs/MAC.md)
* [Linux平台](./docs/CENTOS.md)

### 2 程序文件

操作系统程序也是源码，分为3批加载到计算机，加载的语义是，程序都在软盘上，要加载到计算机内存上，供CPU寻址。

| 程序   | 名称     | 持久化位置 | 加载方(谁加载到内存)   | 内存位置                                                     | 主要功能职责                                                 |
| ------ | -------- | ---------- | ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| BIOS   | BIOS     | 主板ROM    | BIOS ROM存储器内存映射 | [0xffe0 0000...0xffff ffff] High BIOS<br />[0x0xf 0000...0x10 0000]System BIOS<br />[0xe 0000....0xf 0000]Extended System BIOS | 构建BIOS中断向量表<br />加载bootsect程序<br />跳转执行bootsect程序 |
| 第一批 | bootsect | 软盘扇区0  | BIOS程序               | [0x7c00...0x7e00]                                            | 构建软盘文件系统<br />加载loader程序<br />跳转执行loader程序 |
| 第二批 | loader   | 软盘       | bootsect               | [0x1 0000...]视具体的开发大小                                | 构建软盘文件系统<br />加载kernel程序<br />16位实模式切换32位保护模式<br />32位保护模式切换IA-32e模式<br />跳转执行kernel程序 |
| 第三批 | kernel   | 软盘       | loader                 | [0x10 0000...]视具体的开发大小                               |                                                              |

### 3 FAT12文件系统

[为1.44M软盘设计文件系统](./docs/FD.md)

### 4 内存布局

[内存布局和规划](./docs/MEM.md)

### 5 CPU模式

[通过Bochs虚拟机调试模式查看处理器运行模式](./docs/CPU-MODE.md)
