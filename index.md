[toc]
# command

## command-line interpreter

### 常用的命令解析器：

1. shell -- Bourne Shell

> /bin/sh

1. bash -- Bourne Again Shell

> /bin/bash

### 当前系统所使用的shell

> echo $SHELL

### 当前系统下有哪些shell

> cat /etc/shells

## shortcut
### Main keyboard shortcuts
#### Cursor position move
- 光标左移： ctrl + b （**←**）
- 坐标右移： ctrl + f （**→**）
- 移动到头部： ctrl + a（**home**）
- 移动到尾部： ctlr + e（**end**）
#### character delete

- 删除光标前边的字符：ctrl + h（Backspace）

- 删除光标后边(光标后边的字符即光标覆盖的字符)的字符：ctrl + d
- 删除光标前所有内容：ctrl + u
- 删除光标后所有内容：ctrl + k
## linux directory organization

![image-20210710155803264](image/image-20210710155803264.png)

### Introduction to the main directories under Linux

- `/bin`: binary，二进制文件，可执行程序，shell命令

  - 如: ls , rm , mv, cp等常用命令

- `/sbin`: s是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
  - 如ifconfig, halt, shutdown, reboot等系统命令
- `/dev`: device，在linux下一切皆文件
  - 硬盘, 显卡, 显示器
  - 字符设备文件、块设备文件
    - 如: 在input目录下执行: sudo cat mouse0, 移动鼠标会显示有输入.

- `/lib`: linux运行的时候需要加载的一些动态库

  - 如: libc.so、libpthread.so等

- `/mnt`: 手动的挂载目录, 如U盘等
- `/media`: 外设的自动挂载目录, 如光驱等。
- `/root`: linux的超级用户root的家目录
- `/usr`: unix system resource--类似于WINDOWS的programe files目录
  - include目录里存放头文件, 如: stdio.h、stdlib.h、string.h、pthread.h
  - games目录下的小游戏-如: sl小火车游戏
- `/etc`: 存放配置文件
  - /etc/passwd
    - man 5 passwd可以查看passwd文件的格式信息
  - /etc/group

    - man 5 group可以查看group文件的格式信息
    
  - /etc/profile
  
      - 系统的配置文件, 修改该文件会影响这个系统下面的所有的用户
- `/opt`: 安装第三方应用程序

    - 比如安装oracle数据库可以在这个目录下

- `/home`: linux操作系统所有用户的家目录
  - 用户家目录：(宿主目录或者主目录）
    - /home/itcast
- `/tmp`: 存放临时文件
  - 新建在这个目录下的文件会在系统重启后自动清除

## file and directory command
### ls

- `ls -l`

![image-20210712092755128](image/image-20210712092755128.png)
  - file type

    `-` ： 普通文件

    `d` ： 目录

    `l` ：  符号链接，相当于windows中的快捷方式

    `s` ： 套接字

    `p` ： 管道

    `b` ： 块设备

    `c` ： 字符设备
### head

- 从文件头部开始查看前n行的内容

- 使用方式：`head -n[行数] 文件名`

	- `head -20 hello.txt`
- 如果没有指定行数，默认显示前10行内容

### ln
- softlink

	- 如何创建软连接
        ```bash
        # file
        ln -s file file.soft
        # directory
        ln -s tmp tmp.link
        ```

	- 创建软链接应注意事项
	
		- ln创建软连接要用绝对路径，因为如果不使用绝对路径，一旦这个连接文件发生位置变动，就不能找到那个文件了。
	
		- 软连接文件的大小是: 路径+文件名的总字节数

- hardlink

    - `ln test.log test.log.hard`
    - 使用硬链接应注意事项
      - 硬链接不能建在目录上
      - 硬连接对绝对路径没有要求
      - 硬连接不能跨文件系统
        - 硬链接文件和源文件的inode是相同的，文件系统的inode要求唯一，跨文件系统可能会使inode不同, 所以硬链接不能跨文件系统

    - 硬链接的本质


      - ls -i 文件名 ------可以查看文件的i节点
      - stat 文件名 ---可以查看i节点信息
      - 当新创建了一个文件, 硬链接计数为1
      - 给文件创建了一个硬链接后, 硬链接计数加1
      - 删除一个硬链接后, 硬链接计数减1
      - 如果删除硬链接后, 硬链接计数为0, 则该文件会删除

  - 硬链接应用场合

    - 可以起到同步文件的作用

    修改file的内容, 会在其余三个硬链接文件上同步.

    - 可以起到保护文件的作用

    删除文件的时候, 只要硬链接计数不为0, 不会真正删除, 起到保护文件的作用.
### wc
- 显示文件行数, 字节数, 单词数
    - `wc -l file`显示文件的总行数
    - `wc -c file`显示文件的总字节数
    - `wc -w file`显示文件的总单词数
    - `wc file` 显示文件的总行数, 单词数和总字节数 
### chmod

- commond：`chmod [who] [+|-|=] [mode] file`

    - u 操作对象【who】

        `u` -- 用户（user）

        `g` -- 同组用户（group）

        `o` -- 其他用户（other）

        `a` -- 所用用户（all）【默认】

    - 操作符【+-=】

      `+` -- 添加权限

      `-` -- 取消权限

      `=` -- 赋予给定权限并取消其他权限
### chown
- 修改文件所有者chown

  `sudo chown mytest file.txt`
- 修改文件所有者和所属组chown

  `sudo chown mytest:mytest file.txt`
### chgrp
- usage `chgrp 用户组 文件或目录名`

  `sudo chgrp mytest file.txt`
### find
- type
- size
  `find ~/ -size +50k -size -100k`
- ctime
  - 创建日期`-ctime -n/+ n`
    - `-n`: n天以内
  - 修改日期`-mtime -n/+n`
  - 访问日期`-atime -n/+n`
- -maxdepth n(层数)
### grep

- grep -r（有目录） &quot;查找的内容&quot; 搜索的路径
  - `-r`参数, 若是目录, 则可以递归搜索
  - `-n`参数可以显示该查找内容所在的行号
  - `-i`参数可以忽略大小写进行查找
  - `-v`参数不显示含有某字符串

- 搜索当前目录下包含hello world字符串的文件
  - grep -r -n &quot;hello world&quot; ./ ------显示行号
  - grep -r -i -n &quot;HELLO world&quot; ./ -------忽略大小小查找
# vim


# gcc

# makefile 

# gdb

# file

# process

# interprocess communication

# signal

# deamon

# thread

# thread synchronization 

