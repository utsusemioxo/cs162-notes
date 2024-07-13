# 持久存储 persistent storage
持久存储不是内存，内存在断电的时候，内容会丢失！

hard disk drive 和 solid-state storage device 都是 persistent storage

## 存储虚拟化的关键抽象
1. file
	1. 一个线性字节数组，每个字节都能读写
	2. inode number
2. directory
	1. inode number
	2. 目录层次结构 tree

文件系统提供了文件的方便的命名方式

[[文件描述符]]
