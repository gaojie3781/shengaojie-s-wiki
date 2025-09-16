
# ext4文件系统

ext4 文件系统会把分区主要分为两个大部分:
* Inode: 用于保存文件的节点信息。**每一个文件都需要占用一个 Inode，Inode 并不会存储文件的名称**。Inode 的默认大小是 128Byte。主要保存的内容如下：
	* 用来记录文件的权限(r、w、x)
	* 文件的所有者和和组
	* 文件大小
	* 文件的状态改变时间
	* 文件的最近一次读取时间
	* 文件最近一次的修改时间
	* 文件的数据真正保存的 block 的编号
* block：的大小可以是 1KB、2KB、4KB，默认为 4KB。block 用于实际的数据存储，如果一个 block 放不下数据，则可以占用多个 block。
	* **文件的 block**：存放文件的**实际数据**（如文本内容、图片二进制数据）；
	- **目录的 block**：存放该目录下**一级子文件 / 子目录的 “文件名→inode 号” 映射关系**（类似 “索引表”）。

![[CleanShot 2025-09-03 at 15.32.10@2x.png]]


> [!summary] 
> 1. 每个文件都需要占用一个 Inode，文件内容由 Inode 中的记录来指向
> 2. 如果想要读取文件中的内容，就必须要借助目录中记录的文件名找到该文件的 Inode，才能找到文件内容所在的 block 块。


# ln 命令

```bash
ln [选项] 源文件 目标文件
```

选项：
* -s: 建立软连接。如果不加 -s 选项，则建立硬链接文件
* -f: 强制。如果目标文件已经存在，则删除目标删除后再建立链接文件


创建硬链接

```bash
touch book
ln /root/book /tmp
```
建立硬链接文件，目标文件没有写文件名，会和原名一致 。也就是`/tmp/cangls` 是硬链接文件


创建软连接
```bash
touch money
ln -s /root/money /tmp
```


> [!attention] 
> 软链接文件的源文件必须写成绝对路径，而不能写成相对路径（硬链接没有这样的要求）



# 硬链接

一句话解释硬链接：**源文件和链接文件的 inode 指向同一个 block 块**
```bash
[root@localhost ~]# touch test  
#建立源文件  
[root@localhost ~]# ln /root/test /tmp/test-hard  
#给源文件建立硬链接文件 /tmp/test-hard  
[root@localhost ~]# ll -i /root/test /tmp/test-hard  
262147 -rw-r--r-- 2 root root 0 6月 19 10:06 /root/test  
hard  
262147 -rw-r--r-- 2 root root 0 6月 19 10:06 /tmp/test-hard  
#查看两个文件的详细信息，可以发现这两个文件的 inode 号是一样的
```


> [!note] 硬链接的流程

在给源文件 `/root/test` 建立了硬链接文件` /tmp/test-hard `之后，在` /root/ `目录和` /tmp/` 目录的 block 中就会建立 `test` 和 `test-hard` 的信息，这个信息主要就是文件名和对应的 inode 号。但是因为 `test` 和 `test-hard` 的 inode 信息是一样的，因此无论访问那个，最终都会访问 inode 号是 262147 的文件信息

![[CleanShot 2025-09-03 at 15.57.43@2x.png]]


> [!summary] 
> 
- 不论是修改源文件（test 文件），还是修改硬链接文件（test-hard 文件），另一个文件中的数据都会发生改变。
- 不论是删除源文件，还是删除硬链接文件，只要还有一个文件存在，这个文件（inode 号是 262147 的文件）都可以被访问。
- 硬链接不会建立新的 inode 信息，也不会更改 inode 的总数。
- 硬链接不能跨文件系统（分区）建立，因为在不同的文件系统中，inode 号是重新计算的。
- 硬链接不能链接目录，因为如果给目录建立硬链接，那么不仅目录本身需要重新建立，目录下所有的子文件，包括子目录中的所有子文件都需要建立硬链接，这对当前的 Linux 来讲过于复杂。

# 软连接

一句话解释软链接：**链接文件会创建新的 inode ，功能和 windows 系统中的快捷方式类似**

```bash
[root@localhost ~]# touch check  
#建立源文件  
[root@localhost ~]# ln -s /root/check /tmp/check-soft  
#建立软链接文件  
[root@localhost ~]# ll -id /root/check /tmp/check-soft  
262154 -rw-r--r-- 1 root root 0 6月 19 11:30 /root/check  
917507 lrwxrwxrwx 1 root root 11 6月 19 11:31 /tmp/ check-soft -> /root/check  
#软链接和源文件的 inode 号不一致，软链接通过 -> 明显地标识出源文件的位置  
#在软链接的权限位 lrwxrwxrwx 中，l 就代表软链接文件
```

> [!note] 软连接流程

软链接和硬链接在原理上最主要的不同在于：**硬链接不会建立自己的 inode 索引和 block（数据块），而是直接指向源文件的 inode 信息和 block**，所以硬链接和源文件的 inode 号是一致的；而**软链接会真正建立自己的 inode 索引和 block**，所以软链接和源文件的 inode 号是不一致的，而且在软链接的 block 中，写的不是真正的数据，而仅仅是源文件的文件名及 inode 号。

![[CleanShot 2025-09-03 at 15.57.53@2x.png]]


> [!summary] 
> 
- 不论是修改源文件，还是修改硬链接文件，另一个文件中的数据都会发生改变。
- 删除软链接文件，源文件不受影响。而删除原文件，软链接文件将找不到实际的数据，从而显示文件不存在。
- 软链接会新建自己的 inode 信息和 block，只是在 block 中不存储实际文件数据，而存储的是源文件的文件名及 inode 号。
- 软链接可以链接目录。
- 软链接可以跨分区


