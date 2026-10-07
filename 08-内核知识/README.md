# 八、Linux 内核知识入门

> 了解操作系统的心脏，理解进程、内存、文件系统如何工作。

---

## 1. 什么是 Linux 内核？

- 内核是操作系统的核心，负责管理硬件资源、提供系统调用接口。
- 主要功能：进程调度、内存管理、文件系统、设备驱动、网络协议栈。

查看内核版本：

```bash
uname -r
```

---

## 2. 内核架构

- **宏内核**（Monolithic Kernel）：Linux 采用，大部分服务在内核空间运行，性能高。
- 模块化设计：可通过内核模块动态加载/卸载功能。

查看已加载的模块：

```bash
lsmod
```

加载/卸载模块：

```bash
sudo modprobe <模块名>      # 加载
sudo modprobe -r <模块名>   # 卸载
```

---

## 3. 进程管理

- 进程是正在执行的程序实例，由 PID 标识。
- 内核通过调度器分配 CPU 时间片。

常用命令：

```bash
ps aux                # 查看所有进程
top                   # 实时监控进程
kill <PID>            # 终止进程
nice -n <优先级> <命令>  # 以指定优先级运行
```

进程状态：运行(R)、睡眠(S)、不可中断(D)、僵尸(Z)、停止(T)。

---

## 4. 内存管理

- 虚拟内存：每个进程拥有独立的虚拟地址空间。
- 物理内存：通过页表映射。
- OOM Killer：内存不足时内核杀死进程释放内存。

查看内存使用：

```bash
free -h
cat /proc/meminfo
```

交换分区（Swap）：

```bash
swapon --show          # 查看 swap 使用
```

---

## 5. 文件系统

- Linux 支持多种文件系统：ext4, xfs, btrfs, ntfs, fat32 等。
- VFS（虚拟文件系统层）统一接口。

常用操作：

```bash
df -hT                 # 查看磁盘挂载及文件系统类型
mount /dev/sda1 /mnt   # 挂载
umount /mnt            # 卸载
```

---

## 6. 设备驱动

- 设备文件位于 `/dev` 下。
- 字符设备（如键盘）、块设备（如硬盘）、网络设备。

查看设备：

```bash
lsblk                  # 块设备
lspci                  # PCI 设备
lsusb                  # USB 设备
```

---

## 7. 系统调用

- 用户态程序通过系统调用进入内核态执行特权操作。
- 常见系统调用：`open()`, `read()`, `write()`, `fork()`, `exec()`。

查看系统调用列表：

```bash
strace <命令>          # 跟踪程序执行的系统调用
```

---

## 8. 编译内核（进阶）

获取内核源码：

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.tar.xz
tar -xf linux-6.1.tar.xz
cd linux-6.1
```

配置：

```bash
make menuconfig        # 图形化配置
```

编译并安装：

```bash
make -j$(nproc)        # 编译
sudo make modules_install
sudo make install
```

更新引导：

```bash
sudo update-grub       # Debian/Ubuntu
```

---

## 9. 内核调试与日志

查看内核日志：

```bash
dmesg                  # 显示内核环缓冲区消息
journalctl -k          # systemd 系统查看内核日志
```

调试工具：
- `kgdb`：内核调试器
- `perf`：性能分析
- `ftrace`：跟踪函数调用

---

## 10. 学习资源

- 《Linux 内核设计与实现》（LKD）
- 《深入理解 Linux 内核》（ULK）
- 内核文档：`/usr/src/linux/Documentation/`
- 在线：kernel.org, lwn.net

---

> 提示：内核编程需要 C 语言和操作系统基础，建议先从用户态编程入手。生产环境不要随意编译或替换内核。

