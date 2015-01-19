---
layout: post
title: Docker On Overlayfs
---

## Overlayfs

### 概述

*  Developer:  Miklos Szeredi mszeredi@suse.cz
*  Kernel Upstream at 2014/10/24, tag as 3.18
*  Pseudo file-system 
*  Faster than other union mount file-system, almost native performance

### 使用

overlayfs能将两个目录“合并”，比如如下两个目录

		/dir1/
			./file1
		/dir2/
			./file2

mount -t overlayfs overlayfs -olowerdir=/dir1,upperdir=/dir2,workdir=/workerdir /merged

		/merged/
			./file1
			./file2

overlayfs示意图如下

![image]({{ site.baseurl }}/image/overlayfs.png)

### overlayfs规则

* merged目录相当于dir1和dir2的联合
* merged目录中属于dir1的file/dir是read-only的
* merged目录创建file/dir，实际是在dir2创建
* merged目录删除属于dir2目录的file/dir，将会直接删除
* merged目录删除属于dir1目录的file，将会在dir2目录设置该文件为whiteout，实际没有删除dir1目录的file，上层不可见
* merged目录删除属于dir1目录的dir，将会在dir2目录设置该文件为opaque，实际没有删除dir1目录的dir，上层不可见
* merged目录修改属于dir1的目录的file，将会copy dir1的file到dir2，然后修改dir2的file

------
## Init流程

* 调用**supportsOverlay**查看当前系统是否支持overlayfs
* 如果不支持overlayfs，初始化失败退出，返回错误码
* 成功则继续，创建overlayfs graph home目录，初始化成功返回graphdriver.Driver实例

相关数据结构如下
```c++
type ActiveMount struct {
	count   int
	path    string
	mounted bool
}
type Driver struct {
	home       string
	sync.Mutex // Protects concurrent modification to active
	active     map[string]*ActiveMount
}
```
------
## Create流程

------

## Reference
[叠合文件系统--overlayfs](http://wenku.baidu.com/view/2c82473ca32d7375a41780ab.html)