---
layout: post
title: Docker Overlayfs 源码分析
---
## Overlayfs
### 概述
*  Developer:  Miklos Szeredi mszeredi@suse.cz
*  Kernel Upstream at 2014/10/24, tag as 3.18
*  Pseudo file-system 
*  Faster than other union mount file-system, almost native performance

### 使用
overlayfs能将两个目录“合并”，比如如下两个目录

		dir1/
			file1

> dir2/
>>     file2



------
## Init流程
------
## Create流程

------

## Reference
[叠合文件系统--overlayfs](http://wenku.baidu.com/view/2c82473ca32d7375a41780ab.html)